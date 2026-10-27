# AOSP 进阶丛书（基于 Android 13 源码）

> 面向"应用开发者 → 系统开发者"进阶的十九册专题 PDF。源码基线：**AOSP Android 13（Tiramisu，TP1A，API 33，安全补丁 2022-08-05）**，全部行号在本机源码树实测核验。源码树只读研读，未做任何修改。

## 丛书目录

|册|文件|页数|内容一句话|
|第一册|[01-Android全栈架构总览.pdf](01-Android全栈架构总览.pdf)|40|五层架构、源码树地图、HAL 三代演进、构建与分区、ART、SELinux、属性系统、调试观测、进阶路线图|
|第二册|[02-系统启动主线-init-Zygote-SystemServer.pdf](02-系统启动主线-init-Zygote-SystemServer.pdf)|31|电源键到 Launcher 逐函数精读（含逐段注释）：两阶段 init、rc 触发瀑布、Zygote 预加载与 fork、SystemServer 三阶段、BOOT_COMPLETED|
|第三册|[03-Binder-IPC全链路剖析.pdf](03-Binder-IPC全链路剖析.pdf)|35|mmap 一次拷贝、BC/BR 协议、ProcessState/IPCThreadState、servicemanager、Java 层与 AIDL、端到端十跳、线程池/死亡通知/冻结|
|第四册|[04-AMS与四大组件调度.pdf](04-AMS与四大组件调度.pdf)|20|Activity 冷启动十步、ClientTransaction 事务化、Service/Broadcast/Provider 同构骨架、computeOomAdj 记账机器、ANR 体系|
|第五册|[05-WMS窗口与输入体系.pdf](05-WMS窗口与输入体系.pdf)|20|窗口层级树、addWindow 审验漏斗、performTraversals 三幕剧、Choreographer 双 VSync 三缓冲、触摸七段全链路|
|第六册|[06-PMS与包管理.pdf](06-PMS与包管理.pdf)|19|开机扫描建包宇宙、Computer 快照读写分离、会话化原子安装、权限三层闸门、ROM 预装姿势|
|第七册|[07-系统面试题集.pdf](07-系统面试题集.pdf)|28|112 道系统面试题（Binder/启动/Input/AMS/WMS/ROM-BSP/综合场景/深水区 20 题：ART GC·lmkd·传感器 FMQ·IME·OTA·SELinux·APEX·RPC 后端），每题=问题+带源码锚点的深度解答+追问链|

**体例**：第二至六册核心代码采用"逐段注释"（代码内讲解注释+代码后逐段精读）；十九册共 60 张架构图/时序图/流程图（矢量风格 @2x）。建议阅读顺序：①→②→③ 为主线，④⑤⑥ 按需专题，⑦ 作输出训练。

**深度扩写（2026-10-26 最终版）**：核心 6 册深度扩写（书1=40/书2=35/书3=31/书4=29/书5=23/书6=21），书8-19 全部加入核心速查表。全书 338 页 699 条大纲 63 张图。

**教科书升级（2026-10-26 终版）**：全 19 册加入教科书五件套（学习目标/先修链/分级习题/术语表/排障手册），核心三册（架构/启动/Binder）深度扩写至 40/35/31 页（含五层穿越教程、HAL 六文件走读、160+ 服务全表、属性五跳走读、ioctl 深度走读、AIDL 十决策解剖、ANR 完整解剖、OOM 因子教程、触摸七段走读、掉帧五步法、PMS 扫描走读、权限三层走读、安装管线走读、bootchart 实战等）。

**教科书化改造（2026-10-27，进行中）**：按七项标准（≥3 个逐行走读/≥10 道分级习题/≥5 个学习目标/≥15 条术语/≥3 个排障场景/≥2 图）逐册真扩写（五件套此前多为占位，现已全部替换为实内容）。已完成：**书13-19 全部**（SystemUI 34p/HAL 30p/构建 50p/车机 31p/无障碍 37p/CTS 31p/Telephony 32p，每册新增 3-4 章真实源码走读）+ **书8 渲染 39p**（VSync 调度/Gralloc 分配链/RenderEngine/帧生命周期四章）+ **书10 网络 32p**（ConnectivityService 决策内核/NetworkMonitor 状态机/eBPF 数据面/DNS 内核四章）；新增 9 张 SVG 图；书10-19 页眉页脚规范化；修复书2 bootchart 段重复围栏。进度检测器 `build/check_criteria.py`。

**终批（第八批）：书16-19《车机 AAOS》《无障碍体系》《CTS 认证实战》《Telephony》（2026-10-26）**：锚点 8/8 全通过。至此丛书所有规划方向全部出齐。

**第七批：书13-15《SystemUI 与通知体系》《HAL 驱动级实战》《构建系统实战》（2026-10-26）**：+5 张新图、+30 题至 192 题。锚点：SystemUIApplication:62（CoreStartable:34 模型）、NotificationManagerService:349/6397、Launcher.java:238 StatefulActivity、Power.cpp AIDL 模板。
**第六批：书10-12《网络栈》《存储与加密》《媒体管线》（2026-10-26）**：+15 张新图、+30 题至 162 题。路径新发现：Connectivity→packages/modules/Connectivity（Mainline）、NetworkMonitor→packages/modules/NetworkStack、Job/Alarm/Doze→frameworks/base/apex/jobscheduler。检查 A 语义抽验 16/16 通过（1 处 netd main.cpp:111 行号校正）。
**第五批：书8《图形渲染深潜》与书9《后台调度与电源》（2026-10-26）**：各含 6 张新图、逐段注释体例、附录实验室；书9 首章即讲"本树 Job/Alarm/Doze 已迁 apex/jobscheduler"的路径新布局；书7 扩至 132 题。生产采用 STATUS.md 断点续跑清单 + rebuild_all.py 幂等重建。四遍检查全绿（锚点语义回验 12/12、章序/稀疏/大纲、视觉抽检、内容在版）。

**复审修订（2026-10-26 五次修订）**：① 全丛书 107 处源码锚点自动化回验+人工复核，修正书6 两处事实错误（PMS 扫描真身是 `scanSystemDirs`：framework(:325)→priv-app(:337)/app(:342)→overlay(:320)，/data/app 对账(:248)；`ApkSignatureVerifier` 实际位于 `core/java/android/util/apk/`）；② 书4/5/6 章节统一重排为连续编号（消灭"第七章前置/第五章续"杂名与物理乱序），书7 全部〔册·章〕引用同步；③ 书1 补"运行时资源覆盖（RRO）"小节、书5 补 InsetsController 锚点。

**版面优化（2026-10-26 四次修订）**：消灭视觉空白页（稀疏页 9→2，剩余为自然尾页）——目录改为单页章级目录并在下方加"本册速览"卡；小节级导航迁入 **PDF 侧边栏大纲书签（全丛书 275 条 H1/H2/H3 层级，可点击跳转）**；章首防孤行与大图高度参数调优；合并管线修复 meta.brand 剥离大纲的缺陷。

**完整性补全（2026-10-26 三次修订）**：补上读者视角的 10 处机制断点——书1 增第十八章（JNI 三类引用/Context 家族/sp·wp·RefBase）；书4 增 Handler/Looper/MessageQueue 全解（Java→nativePollOnce→epoll 全链）与 PendingIntent/onActivityResult 回传/ReceiverDispatcher 三节；书5 增 SurfaceView 与动画接入；书6 增第八章（签名 v1-v4 四代/IntentResolver 匹配四关/安装后物理布局）；书7 扩至 112 题（+补全 10 题）。

**深水区增补（2026-10-25 二次修订）**：书1 增 SELinux 策略实战/APEX 回滚/Soong 实战/链接器命名空间/sanitizer；书2 增 bootloader 两级跳/fastboot 协议/GKI/update_engine；书3 增三层观测武器库与 RPC 后端；书4 增 ART CC 垃圾回收/MTERP/lmkd PSI/冻结与 cgroup 拓扑；书5 增按键拦截链/预测性返回/传感器 FMQ/IME 文本协议/无障碍；书6 增 installd 配额/多 APK/备份恢复；书7 扩至 102 题。

**出版级排版（2026-10-25 修订）**：正文两端对齐+两字首行缩进；页眉为"左书名｜右当前章名"的书籍式眉批；代码块中西文混排（代码 ASCII 用等宽、中文注释用思源，杜绝豆腐块）；封面主标题限两行并经几何重叠检测自动校验。

## 版式与质量

- 思源黑体（正文/标题）+ DejaVu Mono（代码），全部内嵌；矢量正文 + 2x 高清图表
- 每册含封面、可点击目录书签、页眉页脚
- 质检：字体嵌入、无溢出、无空白页、目录链接、表格居中、边距对称全部通过（自动化 QA + 视觉模型抽页复核）

## 目录结构

```
AOSP资料/
├── 01~07-*.pdf                       ← 七册交付物
├── README.md
├── notes/          ← 七路源码研读笔记（markdown，全部锚点可复查）
│   ├── 01-architecture.md / 02-boot-mainline.md / 03-binder.md
│   ├── 04-ams.md / 05-wms-input.md / 06-pms.md（含 ROM/BSP 笔记）
└── build/          ← 可复现构建管线（改内容后可重新出书）
    ├── content/book1~7.md    ← 七册正文源（轻量标记格式）
    ├── diagrams/*.html|png   ← 29 张图表源与渲染产物
    ├── docbuild.py / cff2ttf.py / finish.py
```

## 修订方法

1. 改 `build/content/bookN.md`（格式见 docbuild.py 文件头注释）
2. 用 `~/.venvs/pdfkit/bin/python` 调 docbuild.build_body(...) 出正文，再 `finish.py` 合并封面
3. 图表：改 `build/diagrams/*.html` 后用 docbuild.shoot_auto 重新截图

## 后续批次（候选）

- 渲染专册：BufferQueue/BLAST/RenderEngine/Display 管线深潜
- 网络专册：ConnectivityService/netd/iptables/eBPF
- 存储与加密专册：vold/FBE/Keymint/OTA 数据面
