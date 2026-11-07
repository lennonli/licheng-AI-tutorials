# 自建家庭设备远程控制网页：电视、空调与打印机部署教程

> **版本**：V2（2026-11-07；补充大文件上传的反向代理配置）  
> **适用场景**：一台常开 Linux VPS，通过私有网络连接家中 Android 电视、支持 Midea V3 局域网协议的空调，以及 HP Smart Tank 660–670 系列打印机。  
> **隐私说明**：本文使用占位符，不含实际域名、家用网络地址、设备编号、令牌、密钥、管理 PIN 或登录凭据。运行命令前，须在自己的私有配置中替换占位符。不同型号和固件支持的功能可能不同，应先按“设备能力核对”逐项确认。

## 1. 最终效果与连接方式

手机浏览器打开 HTTPS 控制台，顶部切换“电视 / 打印机 / 空调”三个页面。电视页显示可点击的遥控键和已安装应用，画面在手机上保持可见；切走电视页时停止画面轮询。空调页分别控制三台设备，并在每次设置后回读状态。打印机页显示状态、队列、墨量和扫描仪状态，可上传 PDF/JPEG、扫描到手机、单页复印，并打开设备自带的 Wi-Fi 管理页。

```text
手机浏览器
    │ HTTPS + 访问认证
    ▼
VPS 上的反向代理 ── 本机 Python 控制服务
    │                         ├─ ADB → Android 电视（私有网络节点）
    │                         ├─ msmart-ng → 空调 A/B/C（家庭局域网）
    │                         ├─ IPP / eSCL → 打印机状态与扫描
    │                         └─ CUPS + HP 驱动 → 打印与复印
    └─ 受认证保护的打印机管理页代理 → Wi-Fi / 设备设置

VPS ⇄ 私有组网 ⇄ 家中常开子网路由器 ⇄ 空调、打印机
```

这套方案要求家中有一台持续在线的路由器或小主机，把设备所在网段通过 Tailscale 子网路由公布给 VPS。电视可自行运行 Tailscale，通过自己的私有网络地址接受 ADB 连接。请只公布必要网段，并限制可访问的节点与端口。子网路由的启用、路由批准和 Linux 客户端接受路由，见 [Tailscale 官方文档](https://tailscale.com/docs/features/subnet-routers?tab=linux)及[路由注入说明](https://tailscale.com/docs/reference/route-injection)。

## 2. 设备能力先核对

| 设备 | 必须确认 | 本方案的边界 |
| --- | --- | --- |
| Android 电视 | 开发者设置中可启用局域网 ADB；从 VPS 执行 `adb devices` 显示 `device` | 部分 Android 9 厂商电视提供自己的网络调试入口；新版 Android 的无线调试配对步骤不能直接照搬。彻底断电或网络服务休眠时，远程控制无法唤醒设备。 |
| 三台空调 | `msmart-ng discover` 识别为受支持的空调设备，V3 设备取得对应 token/key | 只控制支持相应局域网协议的型号；发现广播最好在家庭局域网内执行，不要假设跨子网发现可用。 |
| 打印机 | IPP 可查询状态；eSCL 返回扫描能力；CUPS 可用对应 HP 驱动提交作业 | 此型号的扫描玻璃支持单页扫描。网页复印采用“扫描一页→提交打印队列”，不是模拟面板上的物理按键。Wi-Fi 设置由打印机自带管理页完成，可能要求机身 PIN。 |

先在家中为三台空调和打印机分配稳定地址，记录在**私有**运维配置里。不要把真实地址、SSID、设备编号、令牌或 PIN 写入公开教程和仓库。

## 3. 建立 VPS 到家庭设备的私有连接

1. 在家中常开节点与 VPS 上安装、登录 Tailscale；让它们属于同一私有网络。
2. 家中节点开启转发并公布设备网段，随后在管理后台批准路由。Linux VPS 接受路由。例如：

   ```bash
   # 家中子网路由节点；先按官方文档启用 IP forwarding
   sudo tailscale set --advertise-routes="<家庭设备网段/CIDR>"

   # VPS
   sudo tailscale set --accept-routes=true
   tailscale status
   ```

3. 从 **VPS** 分别测试电视私有地址、空调局域网地址和打印机局域网地址的可达性。Mac 能连接设备，不代表 VPS 已获得路由。只在私有网络内开放 ADB、空调控制端口和打印机端口；不要把这些端口映射到公网。
4. 给控制服务单独选一个本机端口，例如 `8765`。后端只监听 `127.0.0.1:8765`；公网请求统一先经过 HTTPS 反向代理及身份认证。用 `ss -lntp` 和外部无认证访问测试，确认后端端口无法绕过认证。

## 4. 接入电视：ADB、应用与实时画面

### 4.1 配对并验证按键

在电视上开启厂商提供的网络调试功能，并在电视上确认首次 ADB 授权。电视和 VPS 通过私有网络互通后，在 VPS 执行：

```bash
adb connect "<电视私有网络地址>:5555"
adb devices
adb -s "<电视私有网络地址>:5555" shell input keyevent KEYCODE_DPAD_CENTER
```

`adb devices` 必须显示 `device`，而不是 `offline` 或 `unauthorized`。如果电视关闭或系统停止了私有网络应用，先在电视侧恢复连接，再排查网页。网络 ADB 的启用方式随 Android 版本和电视厂商变化；可参照 [Android 官方 ADB 文档](https://developer.android.com/tools/adb)核对命令。

### 4.2 遥控键、应用列表与画面

- 遥控键：网页把方向、确定、返回、音量等操作发送到后端，由后端执行 `adb shell input keyevent`；浏览器不直接连接电视。
- 应用入口：后端查询电视的 `LEANBACK_LAUNCHER` 和 `LAUNCHER` 活动，并按包名去重。点击网页应用卡片后，以 `adb shell am start -n <组件名>` 启动。只有电视当前报告为可启动的应用才显示；不要把预设应用名称当作已安装证明。
- 实时画面：电视的 `screenrecord` 输出 H.264，VPS 上的 `ffmpeg` 转成单张 JPEG；手机依次请求 `/frame`。这样可在 Safari 等浏览器显示，`ETag` 可避免重复下载相同画面。画面容器只在电视页固定，切换到打印机或空调页时停止轮询。
- 若按键生效而画面不刷新，分别检查 ADB 状态、电视侧残留的录屏进程、`ffmpeg` 管道、`/frame` 是否返回 JPEG，以及浏览器是否仍在电视页。

## 5. 接入三台空调：发现、密钥与回读

在能发现设备的家庭局域网机器上安装 `msmart-ng`，先查帮助，再执行发现：

```bash
python3 -m venv .venv
.venv/bin/pip install "msmart-ng==2026.9.0"
.venv/bin/msmart-ng discover --help
.venv/bin/msmart-ng discover
```

逐台核对 `supported`、设备类型、地址和设备编号。V3 设备获取 token/key 时可能需要一次云端认证；取得后，本地控制可不依赖持续云连接，详见 [msmart-ng 项目说明](https://github.com/mill1000/midea-msmart)。发现输出可能含真实密钥，**不要贴到聊天、日志或 GitHub**。把三台设备分别标为“空调 A / B / C”，在服务器**私有文件**中保存各自的编号、IP、token 和 key；文件权限设为 `0600`：

```json
{
  "<设备编号A>": {"name": "空调 A", "ip": "<空调A局域网IP>", "token": "<私有token>", "key": "<私有key>"},
  "<设备编号B>": {"name": "空调 B", "ip": "<空调B局域网IP>", "token": "<私有token>", "key": "<私有key>"},
  "<设备编号C>": {"name": "空调 C", "ip": "<空调C局域网IP>", "token": "<私有token>", "key": "<私有key>"}
}
```

后端通过 `msmart.device.AirConditioner` 建立到端口 `6444` 的连接，认证、刷新状态、设置一个参数、`apply()`，等待后再 `refresh()`。网页支持开关、制冷/制热等模式、设备允许范围内的 0.5°C 温度步进、风速、摆风、节能和强力。每次只改一个参数并回读：接口返回“命令已发送”不能替代设备状态确认。空调能力和温度范围以每台设备回报为准。

## 6. 接入打印机：状态、打印、扫描与复印

### 6.1 状态与墨量

从 VPS 向打印机的 IPP 地址读取属性：

```bash
ipptool -tv "ipp://<打印机局域网IP>/ipp/print" \
  /usr/share/cups/ipptool/get-printer-attributes.test
```

读取 `printer-state`、`printer-is-accepting-jobs`、`queued-job-count`、`marker-levels` 等字段，展示“就绪/打印中/停止”、队列与墨量。`marker-levels` 的含义和顺序应与设备实际输出核对；状态接口不应暴露打印内容。

### 6.2 用 CUPS + HP 驱动打印 PDF/JPEG

在 Debian 系 VPS 上安装 `cups`、`cups-client`、`printer-driver-hpcups`，先用 `lpinfo -m` 确认本机确有该机型驱动，再建立队列：

```bash
sudo apt-get install cups cups-client printer-driver-hpcups
lpinfo -m | grep 'smart_tank_660-670'
sudo lpadmin -p HomePrinter -E \
  -v "socket://<打印机局域网IP>:9100" \
  -m "drv:///hpcups.drv/hp-smart_tank_660-670_series.ppd"
lpstat -p HomePrinter

# 在确认打印机已放纸后，打印自己的测试文件
lp -d HomePrinter -o media=A4 -o fit-to-page "<测试文件.pdf>"
```

这一队列让 CUPS 过滤器与 HP 驱动转换文档，再经打印机网络端口提交。**不要把 PDF 或第三方生成的裸 PCLm/URF/PWG 数据直接提交到设备 IPP 端口**：本方案的设备实测出现过乱码或卡住。此前逐页转 JPEG 再走 IPP 可以出纸，但较慢；使用 CUPS + HP 驱动打印一页 PDF 的测试曾在约 20 秒内完成，纸面清晰度仍须在打印机旁目视确认。CUPS 队列与 `lpadmin` 的用法见 [CUPS 官方管理文档](https://www.cups.org/doc/admin.html)。

网页上传只接受不超过 16 MB 的 PDF/JPEG。浏览器把文件字节发给本机后端，后端校验文件头并通过 `lp` 的标准输入提交，不在应用目录保存上传件。CUPS 处理时可能产生短暂的队列文件，应设置 `PreserveJobFiles No`、`PreserveJobHistory No`，并把 CUPS 监听限定在本机；这不等于“打印过程绝无磁盘暂存”。

### 6.3 扫描到手机与单页复印

先只读检查扫描仪能力：

```text
GET http://<打印机局域网IP>/eSCL/ScannerCapabilities
GET http://<打印机局域网IP>/eSCL/ScannerStatus
```

本方案的设备报告有扫描玻璃、JPEG/PDF、彩色/灰度和多档分辨率。网页提供 PDF/JPEG、彩色/灰度、150/300 dpi。后端按 eSCL 流程 `POST /eSCL/ScanJobs`，从响应 `Location` 中**只取任务路径并固定打印机主机**，再 `GET .../NextDocument` 获取扫描结果，完成后删除扫描任务。扫描文件直接返回当前浏览器下载；不能把打印机返回的任意 URL 当作可访问地址。eSCL 请求格式可参考 [SANE AirScan 的协议实现](https://github.com/alexpevzner/sane-airscan/blob/master/airscan-escl.c)。

“复印”按钮执行同一次单页扫描，再把 JPEG 送入 CUPS 队列，可选黑白/彩色和 1–5 份。请先把原稿放在玻璃上；网页无法替你放纸或确认纸面效果。本方案已实际验证单页 JPEG 和 PDF 扫描返回，网页复印出纸仍需现场验收。

### 6.4 Wi-Fi 与设备设置

打印机自带的 Embedded Web Server（EWS）有 Network、Scan、Copy、Tools 等页面；网页“Wi-Fi 设置”按钮通过现有 HTTPS 反向代理打开其 Network 页面。某些设置要求打印机机身标签的 PIN，**不要把 PIN 写入控制台代码、前端页面或公开仓库**。Wi-Fi 设置由 EWS 自身处理，不在控制台另存无线密码。改动 SSID、地址或网络模式后，当前远程连接可能立即断开，应预留在家中恢复连接的方式。HP 的 [EWS 使用说明](https://support.hp.com/gb-en/document/ish_2029438-1929621-16)可用于核对具体菜单。

EWS 页面会加载多组根路径资源。反向代理不能只转发 `/printer-admin/` 首页，还须按实际设备请求代理其静态资源和设备接口路径；每一条路径都继承同一身份认证。先通过仅本机可访问的临时代理测试首页、脚本、Network 页面和登录流程，再向公网入口开放。不要把打印机管理页或 CUPS 的 `631` 端口直接暴露到公网。

## 7. Web 服务与安全配置

将控制后端部署在 VPS，设置 `TV_ADB_ADDR`、`TV_PRINTER_IP`、`TV_AC_KEYS_FILE`、`TV_AC_PYTHON` 等私有环境变量。打印队列名可通过 `TV_PRINTER_QUEUE` 指定。空调密钥文件只在服务器上，以 `0600` 权限保存；部署时复制代码和网页文件，**不要把密钥文件加入代码仓库**。

反向代理示意如下，须替换域名、证书、认证文件与端口，并按打印机 EWS 的实际路径补足代理规则：

```nginx
server {
    listen <HTTPS端口> ssl;
    server_name <控制台域名>;
    ssl_certificate     <证书路径>;
    ssl_certificate_key <私钥路径>;
    auth_basic "Home Control";
    auth_basic_user_file <仅服务器可读的认证文件>;

    location /printer-admin/ {
        proxy_pass http://<打印机局域网IP>/;
        proxy_set_header Host <打印机局域网IP>;
    }
    # EWS 依赖的 /framework/、/webApps/、/DataObjects/、/DevMgmt/
    # 等路径也须单独反代，并继承上方的身份认证。

    location / {
        proxy_pass http://127.0.0.1:8765;
        proxy_http_version 1.1;
        proxy_request_buffering off; # 上传文件直接转给后端，避免落入 nginx 请求体临时目录
        proxy_read_timeout 180s; # 单页扫描可持续较久
        client_max_body_size 20m;
    }
}
```

`nginx -t` 通过后再重新载入。用未登录浏览器请求首页、`/frame`、`/printer/status`、`/printer-admin/` 等路径，均应得到 `401`；从公网直连后端端口应失败。对 TV、空调和打印机的控制接口，认证要求应与首页一致。建议为管理账号设置独立强密码，避免在日志中记录扫描内容、上传文件、token、key 与 PIN。

**上传大文件返回 500 时先查 nginx 错误日志。** 如果出现 `client body temp` 或 `open() ... failed (13: Permission denied)`，说明请求在 nginx 暂存阶段就失败，尚未到 CUPS。对上述本机后端代理设置 `proxy_request_buffering off` 后，先用超过 1 MB 的**无效测试文件**发往上传接口，确认返回后端的 `400`，再用自己的 PDF/JPEG 做实际打印验收。不要因网页显示 500 就反复点击打印，以免在另一种故障下产生重复作业。

## 8. 完工验收与排障

| 检查项 | 通过标准 | 失败时先查 |
| --- | --- | --- |
| 私有连接 | VPS 能连到电视、三台空调、打印机 | 子网路由在线、路由已批准、VPS 已接受路由、访问策略允许目标端口 |
| 电视 | `adb devices` 为 `device`；网页按键生效；应用列表来自电视；画面刷新 | 电视是否开机、私有网络应用是否存活、ADB 授权、录屏/转码进程 |
| 空调 | 三台各自显示在线；设置后返回实际状态 | IP 是否变化、V3 token/key 是否有效、设备是否支持该参数 |
| 打印 | IPP 返回 idle/accepting；CUPS 队列可用；纸面正确 | 驱动、队列 URI、纸张、卡住的作业；不要重复投递同一文件 |
| 文件上传 | 大文件请求到达后端；无效格式由后端返回 400 | nginx 错误日志、请求体临时目录权限、`proxy_request_buffering off` 是否生效 |
| 扫描 | 放入非敏感测试页，网页下载的 PDF/JPEG 可打开 | eSCL 状态、扫描玻璃、任务超时、浏览器下载权限 |
| 复印 | 一页原稿按所选颜色和份数出纸 | 先独立验证扫描与 CUPS 打印，再查复印任务 |
| Wi-Fi 管理 | 认证后打开 EWS Network 页面；需要时使用机身 PIN | EWS 资源路径代理、设备自身 PIN、改网后 IP 是否变化 |
| 公网边界 | 无认证访问全部拒绝；后端、ADB、CUPS 不在公网监听 | 反向代理认证是否覆盖新路径、服务绑定地址和防火墙 |

电视彻底关机后无法通过这套 ADB 链路“远程开机”；打印机断开家庭 Wi-Fi 后也无法经原私有路由修复其联网。这两种情况需要现场操作。

## 参考资料

- [Tailscale：子网路由](https://tailscale.com/docs/features/subnet-routers?tab=linux)
- [Android：ADB 使用说明](https://developer.android.com/tools/adb)
- [msmart-ng：支持设备、发现与 V3 密钥说明](https://github.com/mill1000/midea-msmart)
- [CUPS：打印队列管理](https://www.cups.org/doc/admin.html)
- [SANE AirScan：eSCL 请求实现](https://github.com/alexpevzner/sane-airscan/blob/master/airscan-escl.c)
- [HP：打印机 EWS 使用说明](https://support.hp.com/gb-en/document/ish_2029438-1929621-16)
