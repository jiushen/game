# PSV 名将改版玩法与模拟器优化指南

> 更新日期：2026-08-29
> 适用机型：破解 PSV（Henkaku/Enso，系统 3.72）

---

## 一、环境概况

| 项目 | 状态 |
|---|---|
| 模拟器 | RetroArch **1.22.1**（官方稳定版，原"全能模拟器"RETROVITA 已被其原地覆盖升级）|
| 街机核心 | **FinalBurn Neo (FBNeo) v1.0.0.03** |
| 配置目录 | `ux0:/data/retroarch/`（新旧模拟器共用）|
| 核心目录 | `ux0:/app/RETROVITA/`（精简后 23 个核心）|
| 核心备份 | `ux0:/core_backup/`（被移走的 101 个核心 + 267 个 info 文件）|

---

## 二、用哪个模拟器玩名将改版

**必须用 RetroArch 的 FinalBurn Neo 核心**（不能用 pFBA / FBAlpha，它们没有战神版驱动和 IPS 功能）。

### 原理

名将的修改版（战神版、梦魇版等）在 FBNeo 中通过 **IPS 补丁机制**运行：

1. 你加载的是**原版名将 ROM**（`captcomm.zip`，标准 FBNeo ROM 集，已过 CRC 校验）
2. FBNeo 核心扫描 `system/fbneo/ips/captcomm/` 下的 **`.dat` 描述文件**
3. 启用某个补丁后，核心先打 relay 转换补丁（把当前版 ROM 转成旧版基座），再打改版补丁，并自动扩展 ROM 缓冲区（128K 小 ROM 扩到 1MB）

### 关键文件位置

```
ux0:/roms/HACK/36 [HACK] 名将 战神版 [FBNeo IPS]/captcomm.zip   ← 原版 ROM（已校验）
ux0:/data/retroarch/system/fbneo/ips/captcomm/                    ← 补丁目录
├── captcmzs_20240622.dat   战神版描述文件
├── captcmmy_20240705.dat   梦魇版描述文件
├── captcommr1/             relay 转换补丁（各改版共用）
├── captcmzs/               战神版补丁本体
├── captcmmy/               梦魇版补丁本体
└── ...（共 17 个改版）
```

### 操作步骤

1. RetroArch → **载入核心 → FinalBurn Neo**
2. **载入游戏** → `roms/HACK/36 [HACK] 名将 战神版 [FBNeo IPS]/captcomm.zip`
3. 先正常进入原版名将
4. 呼出快捷菜单（热键或 L+R+Start+Select）→ **选项（核心选项）**
5. 找到 **IPS Patch** 分组 → 选中想要的改版（如「整体修改/梦魇 - 20240705」）→ 设为启用
6. 快捷菜单 → **重新启动** → 改版生效

### 注意事项

- **一次只能启用一个改版**（它们修改同样的 ROM 文件，同时启用会冲突）
- 切换改版：进核心选项把当前补丁关掉、开新的，再重新启动游戏
- 「允许修补集组 (Allow patched romsets)」选项必须保持开启（它同时是 IPS 的总开关，默认开）
- 补丁来源：[taoenwen/FBNeo_IPS](https://github.com/taoenwen/FBNeo_IPS) 仓库的 `CPS/captcomm/` 目录

### 可用的 17 个改版

**整体修改类**：战神、梦魇、无双、CR7 精英、二爷、打手、精英大赛、变身、大赛会、无限子弹、征途、1V4、重新调整 v1.0

**版本切换类**（原版换区）：日版 911202、日版 910928、美版 910928、世界版 911014

---

## 三、启动速度优化记录

### 优化前 → 优化后：35 秒 → 约 10 秒

| # | 措施 | 操作方式 | 效果 |
|---|---|---|---|
| 1 | **禁用 12.5MB OSD 大字体** | `retroarch.cfg` 中 `video_font_enable = "false"` | 35s → 14s（主因：老配置把 XMB 菜单的巨型字体设成了 OSD 字体）|
| 2 | **界面切英文** | `user_language = "0"`（或 Settings → User → Language）| 跳过 4.5MB 中文菜单字体解析 |
| 3 | **关动态壁纸** | `menu_dynamic_wallpaper_enable = "false"` | 跳过壁纸图片解码和内置图片核心加载 |
| 4 | **精简核心** | 把 101 个不用的 `.self` 从 `app0:/` 移到 `ux0:/core_backup/` | 124 → 23 个核心，省 412MB |
| 5 | **精简核心信息文件** | 把 267 个孤儿 `.info` 移到 `ux0:/core_backup/info/` | 292 → 24 个（启动时逐个解析，量大很慢）|
| 6 | 关播放列表子标签 | `playlist_show_sublabels = "false"` | 翻列表更快 |

### 剩余约 10 秒说明

Vita（2011 年硬件）跑 RetroArch 1.22 的固有开销：前端初始化 + GPU 初始化 + 内置占位核心（dummy core）初始化。**这基本是这台机器跑最新版的地板了**，无法再明显压缩。

### 如需回退

- 想要中文界面：Settings → User → Language → 简体中文（代价：启动慢几秒，游戏列表里的中文游戏名不受影响，只是菜单文字变中文）
- 想找回某个核心：从 `ux0:/core_backup/` 把对应 `.self` 移回 `ux0:/app/RETROVITA/` 即可

---

## 四、其他要点

### 原来"FB Alpha"核心的游戏怎么办

安装官方 1.22.1 时旧应用目录被清空，而 FBAlpha 核心 2019 年起就不在官方包里。**受影响的街机游戏改用 FB Alpha 2012 或 FinalBurn Neo 核心打开**即可（ROM 集基本兼容）。

### 目录速查

| 路径 | 内容 |
|---|---|
| `ux0:/roms/HACK/` | 改版游戏目录（含其他 MAME2003+ 格式 hack）|
| `ux0:/data/retroarch/system/fbneo/ips/` | IPS 补丁根目录 |
| `ux0:/core_backup/` | 被精简掉的核心（含 MOVED_CORES.txt 清单）|
| `ux0:/leonxing/` | 安装包存放处（RetroArch_1.22.1.vpk 已可删除）|

### 想加其他游戏的改版

去 [taoenwen/FBNeo_IPS](https://github.com/taoenwen/FBNeo_IPS) 仓库，按平台（CPS/NeoGeo/PGM 等）找到对应游戏的文件夹，把 `.dat` 文件和同名子文件夹放进 `ux0:/data/retroarch/system/fbneo/ips/<游戏ROM名>/` 即可（注意 relay 依赖文件夹也要一并下载）。

### 常见问题

- **IPS Patch 选项不出现**：确认游戏是用 FinalBurn Neo 核心加载的原版 `captcomm.zip`；检查系统目录设置指向 `ux0:/data/retroarch/system`
- **改版启用后花屏/崩溃**：确认只启用了一个补丁；确认 relay 文件夹（captcommr1/）完整
- **核心加载报 C2-12828-1**：核心与前端版本不匹配或核心过大内存不足——本文方案已用配套版本解决，勿单独替换核心文件
