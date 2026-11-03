StreamFetch Studio — 基于 yt-dlp 的全网视频下载工作台



StreamFetch Studio 是一款基于开源项目 yt-dlp 构建的现代化桌面视频下载应用程序。项目采用 零第三方 Python 依赖 架构，通过本地轻量级服务呈现深色流媒体工作台界面（另有原生 Tkinter 窗口版可选），内置 `yt-dlp` 与 `FFmpeg` 组件自动管理，支持 Bilibili（B站分P、番剧、4K/1080P60 大会员高码率）、YouTube、TikTok/抖音 等 1800+ 音视频站点的一键解析与高速下载。
核心功能特性
内置核心组件与自动热更新：
程序启动时自动检查 `~/.ytdlp_helper_app/bin/` 目录下的 `yt-dlp` 与 `FFmpeg` 可执行组件；若本地缺失，自动从 GitHub 官方 Releases 下载对应操作系统的独立二进制文件，无需手动配置系统环境变量。
每次启动自动比对 GitHub 最新 Release 版本号，发现新版支持一键在线升级。
粘贴链接解析与多站点支持：
支持直接粘贴 B站视频链接（`bilibili.com/video/BV...`、`b23.tv` 短链）、番剧剧集（`ep...`）、UP主合集以及 YouTube 等全网视频链接。
支持实时获取视频标题、UP主/频道名称、时长及源站可用分辨率列表，并同步生成可复制的 `yt-dlp` 命令行预览。
多档画质选择与无损混流：
提供 4K 超清 (2160P 60帧)、1080P 高码率 (60帧)、720P 标清流畅、480P 极速省流、最佳可用画质 (Max) 以及 仅提取音频 (MP3 320kbps) 六档预设。
自动调用内置 `FFmpeg` 将 B站/YouTube 的 DASH 音视频分离轨无损合并为标准 `.mp4` 文件，并自动清理临时分片与旁路残留文件。
B站扫码登录与多模式 Cookies 管理：
手机 B站 App 扫码一键登录：内置 Bilibili Passport 官方二维码登录流程，使用哔哩哔哩手机客户端「扫一扫」确认后，自动提取真实 `SESSDATA` 与 `bili_jct` 写入本地 Netscape 格式 `cookies.txt`，并实时调用接口验证账号昵称与大会员权限。
手动导入 / 文本转换：支持上传本地 `cookies.txt` 文件，或直接粘贴浏览器请求头中的 Raw Cookie 字符串（自动转换为 Netscape 标准格式）。
浏览器直接读取：支持通过 `--cookies-from-browser` 从 Chrome、Edge、Firefox、Brave 或 Safari 直接读取登录凭据。
持久化历史下载记录与文件定位：
本地 JSON 持久化存储每一条下载记录（标题、站点、画质、文件大小、完成时间与保存路径），支持按站点过滤、关键词搜索与一键重新下载。
支持在历史记录中点击 「📂 打开文件位置」，在 Windows 下调用系统原生 `Shell32` API（`SHOpenFolderAndSelectItems`）将资源管理器置于屏幕最前端并精准高亮选中目标视频文件（macOS 下调用 `open -R`）。
依赖环境说明
本项目在设计上追求极简部署，无需 `pip install` 任何第三方 Python 包，开箱即用。
依赖项	版本 / 要求	说明与安装方式
操作系统	Windows 10/11、macOS 11+、Linux	跨平台支持（Windows 下含原生 Shell32 文件高亮定位优化）
Python 环境	Python 3.8 及以上	仅使用 Python 标准库（`http.server`、`urllib`、`subprocess`、`ctypes`、`tkinter` 等）
yt-dlp 核心	自动下载 / 内置管理	首次启动自动从 yt-dlp Releases 下载至 `~/.ytdlp_helper_app/bin/`
FFmpeg 混流器	自动下载 / 内置管理	Windows 下若未检测到系统 FFmpeg，自动从 yt-dlp/FFmpeg-Builds 下载独立组件；macOS/Linux 用户可通过 `brew install ffmpeg` 或 `apt install ffmpeg` 安装
现代浏览器	Edge / Chrome / Firefox / Safari	用于渲染本地 `127.0.0.1` 深色可视化工作台界面
目录结构
```text
ytdlp-desktop-app/
├── app.py                      # 主程序入口（启动本地后端服务 + 自动打开深色工作台界面）
├── app_tkinter.py              # 轻量级 Tkinter 系统原生窗口版入口（可选）
├── 启动应用_Windows.bat         # Windows 一键启动脚本（双击即可运行）
├── 启动应用_Mac_Linux.command   # macOS / Linux 一键启动脚本
└── README.md                   # 项目说明文档
```
程序运行后，会在用户主目录下自动创建配置与组件工作区 `~/.ytdlp_helper_app/`（Windows 下位于 `C:\Users\<用户名>\.ytdlp_helper_app\`）：
```text
~/.ytdlp_helper_app/
├── bin/
│   ├── yt-dlp(.exe)            # 自动下载与更新的内置 yt-dlp 核心可执行文件
│   └── ffmpeg(.exe)            # 自动下载的内置 FFmpeg 音视频混流组件
├── config.json                 # 用户偏好设置（默认画质、保存目录、Cookie 模式等）
├── cookies.txt                 # Netscape 格式登录凭据文件（扫码或手动保存后生成）
└── history.json                # 本地持久化历史下载记录
```
快速开始与安装运行
1. 克隆或下载仓库
```bash
git clone https://github.com/<sg953104-sys>/streamfetch-studio.git
cd streamfetch-studio
```
2. 启动应用程序
Windows 用户：
直接双击仓库根目录下的 `启动应用_Windows.bat`，或在终端（PowerShell / CMD）中执行：
```bash
  python app.py
  ```
macOS / Linux 用户：
直接双击 `启动应用_Mac_Linux.command`，或在终端中执行：
```bash
  python3 app.py
  ```
启动后，终端会初始化内置 `yt-dlp` 组件并在默认浏览器自动打开 `http://127.0.0.1:8765` 进入可视化工作台。
> **备选原生窗口模式**：如果你偏好不依赖浏览器的传统桌面窗口，可执行 `python app_tkinter.py` 启动 Tkinter 原生界面版。
详细使用指南
1. 下载视频与选择画质
在左侧导航栏进入 「视频链接下载」 面板。
点击 「📋 粘贴链接」（或在输入框粘贴 B站 BV 号/视频链接、YouTube 链接），点击 「🔍 解析视频」 查看视频标题、时长与可用最高分辨率。
在画质选择区点击所需画质卡片（如 `1080P 高码率`、`4K 超清` 或 `仅提取音频 MP3`）。
如需下载 B站多 P 视频全集或番剧合集，勾选 「下载整个分P/合集列表 (`--yes-playlist`)」。
点击 「🚀 开始下载视频」，底部终端控制台将实时显示 `yt-dlp` 下载速度、百分比进度与 FFmpeg 混流状态。默认下载保存目录为系统下载文件夹下的 `~/Downloads/yt-dlp_Downloads`。
2. 解锁 B站 1080P 高码率与 4K 画质（导入 Cookies）
B站对未登录游客请求仅返回 360P/480P 低码率流。若需下载 1080P、1080P60帧 或 4K 超清画质，请切换到左侧 「导入 Cookies」 面板，任选以下一种方式配置：
方式 A：手机 B站 App 扫码一键登录（推荐）
点击 「📱 手机 B站 App 扫码一键登录 (推荐)」 按钮，页面将生成 B站官方登录二维码。
打开手机上的 哔哩哔哩 App，点击首页右上角或「我的」页面的「扫一扫」图标扫描二维码并在手机端点击「确认登录」。
登录成功后，程序会自动提取 `SESSDATA` 与 `bili_jct` 转为 Netscape 格式写入本地，并在底部状态栏显示你的 B站昵称与大会员激活状态。
方式 B：F12 复制粘贴或上传 `cookies.txt`
在已登录 B站的浏览器网页按 `F12` 打开开发者工具 -> 切换到「网络 (Network)」标签 -> 按 `F5` 刷新页面 -> 任选一条请求复制 Request Headers 中的 `Cookie:` 字符串。
粘贴到文本框中并点击 「💾 验证并保存 Cookies 设置」，程序会自动将其转换为 `yt-dlp` 标准 Netscape 格式并校验有效性。
3. 查看历史下载记录与定位本地文件
切换到左侧 「历史下载记录」 面板，可按站点（Bilibili / YouTube）过滤或通过关键词搜索已下载的视频。
点击任意记录右侧的 「📂 打开文件位置」，系统文件资源管理器将直接弹出至屏幕最前端，并自动高亮选中对应的 `.mp4` / `.mp3` 文件。
点击 「🔁 重新下载」 可将该视频链接重新载入下载队列。
4. 检查与更新 yt-dlp 组件
程序每次启动时默认在后台自动检查 `yt-dlp` 官方是否有新版本发布。
你也可以随时点击顶部状态栏的 「检查更新」 或进入左侧 「yt-dlp 组件与更新」 面板，点击 「🔄 立即检查并更新 yt-dlp」 在线升级至最新稳定版。
常见问题排查 (FAQ)
Q1：为什么选择了 4K 或 1080P，下载下来的视频却只有几 MB（360P）？
原因：一是未配置有效的新鲜 B站 Cookies（B站接口对未登录请求强制降级为 360P）；二是系统缺少 `FFmpeg` 混流器导致无法合并 DASH 音画分离的高清流。
解决：请先在「导入 Cookies」页面通过手机 B站 App 扫码登录（确认底部提示显示「B站账号验证成功」），并在「yt-dlp 组件与更新」页面点击一次检查更新，确保内置 `FFmpeg` 状态显示为「已就绪」。
Q2：使用「方式二：从本机浏览器直接读取 (`--cookies-from-browser`)」时报错怎么办？
原因：新版 Chromium 内核浏览器（Edge / Chrome）在运行期间会对 SQLite Cookie 数据库加独占锁，导致外部进程无法直接读取。
解决：建议使用 「方式一」 配合 手机 B站 App 扫码一键登录 或粘贴 Cookie 文本，生成的独立 `cookies.txt` 文件不会与浏览器进程产生文件锁冲突。
开源协议与免责声明
本项目遵循 MIT License 开源协议，核心音视频解析能力由开源项目 yt-dlp（Unlicense）与 FFmpeg 提供。
本工具仅供个人学习、技术研究与授权内容离线缓存使用，请严格遵守各音视频网站的服条款与版权规范，切勿用于任何商业侵权用途。
