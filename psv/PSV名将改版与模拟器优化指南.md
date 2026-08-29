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

**方式一：播放列表直接进改版（推荐，2026-08-29 新增）**

全部 300 个 IPS 改版已做成**独立播放列表条目**，点击即进改版游戏，无需手动勾选补丁：

1. RetroArch 主界面 → **收藏**（播放列表）→ 打开对应的改版列表：

   | 播放列表 | 条目数 | 内容 |
   |---|---|---|
   | 名将改版 | 17 | 战神、梦魇、无双、CR7精英、1V4、日版/美版切换… |
   | 恐龙快打改版 | 33 | 世纪、魔神乱舞、无双、三叠纪、游聚十周年… |
   | 三国志2改版 | 12 | 三美、马战、战狼、达人… |
   | 三国战纪改版 | 45 | 三国战纪/2/Plus 的风云再起系改版 |
   | 西游释厄传改版 | 44 | 群魔乱舞、大圣归来、噩梦求生… |
   | 拳皇改版 | 139 | 拳皇94~2003 全系（风云再起、屠蛇、终极之战…）|
   | 快打旋风/圆桌骑士/惩罚者/街霸2改版 | 10 | 各 1~4 条 |

2. 点任意条目（如「名将 战神 - 20240622」）→ 直接以 FBNeo 核心启动改版游戏

原理：列表条目直接指向 IPS 的 `.dat` 描述文件（`system/fbneo/ips/<游戏>/<改版>.dat`），FBNeo 自动加载原版 ROM 并打补丁。所需原版 ROM 已集中放在 `ux0:/data/retroarch/system/fbneo/arcade/`（22 个，约 895MB），`neogeo.zip`、`pgm.zip` BIOS 已在 `system/fbneo/`。

**方式二：先加载原版再选补丁（通用）**

**方式二：先加载原版再选补丁（通用）**

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

### 可用的改版（共 12 个游戏，140+ 个改版）

IPS 补丁位于 `ux0:/data/retroarch/system/fbneo/ips/<游戏名>/`，用法统一：**FBNeo 核心加载原版 ROM → 核心选项 → IPS Patch → 选改版 → 重新启动**。一次只启用一个。

| 游戏 | 原版ROM | 改版数 | 亮点 |
|---|---|---|---|
| 名将 captcomm | captcomm.zip | 17 | 战神、梦魇、无双、CR7精英、1V4… |
| 三国志2 wof | wof.zip | 12 | 三美、马战、战狼、达人、无双加强… |
| 惩罚者 punisher | punisher.zip | 3 | 1v2、框架版2020 |
| 快打旋风 ffight | ffight.zip | 1 | 框架版2020 |
| 恐龙快打 dino | dino.zip | 33 | 世纪、魔神乱舞、三叠纪、游聚十周年… |
| 圆桌骑士 kod | kod.zip | 2 | 狼牙困难 |
| 街霸2CE sf2ce | sf2ce.zip | 4 | 三问版、训练模式、混合 |
| 三国战纪 kov/kov2/kovplus | IGS目录对应zip | 45 | 风云再起系（2012无双/群雄乱舞/真·吞食天地/群英新传…）|
| 西游释厄传 orlegend/olds/oldsplus | IGS目录对应zip | 44 | 群魔乱舞、大圣归来、噩梦求生、缘聚无双… |
| 拳皇94~2003 | NeoGeo/拳皇系列对应zip | 139 | 风云再起系列（多代）、屠蛇、天国神族、终极之战、连技版、练习模式、Boss版… |

NeoGeo 改版（kof94-2003、garou、mslug 等）在仓库里也有，需要时从 [taoenwen/FBNeo_IPS](https://github.com/taoenwen/FBNeo_IPS) 下载对应游戏文件夹放入 ips 目录即可。

---

## 三、启动速度优化记录

### 优化前 → 优化后：35 秒 → 约 10 秒

| # | 措施 | 操作方式 | 效果 |
|---|---|---|---|
| 1 | **禁用 12.5MB OSD 大字体** | `retroarch.cfg` 中 `video_font_enable = "false"` | 35s → 14s（主因：老配置把 XMB 菜单的巨型字体设成了 OSD 字体）|
| 2 | **界面语言** | rgui 下中英文都行：rgui 用**外置点阵字库**渲染中文，开销极小（4.5MB TTF 字体只有 ozone/xmb 菜单才会解析）| 中文界面可直接用 |
| 3 | **关动态壁纸** | `menu_dynamic_wallpaper_enable = "false"` | 跳过壁纸图片解码和内置图片核心加载 |
| 4 | **精简核心** | 把 101 个不用的 `.self` 从 `app0:/` 移到 `ux0:/core_backup/` | 124 → 23 个核心，省 412MB |
| 5 | **精简核心信息文件** | 把 267 个孤儿 `.info` 移到 `ux0:/core_backup/info/` | 292 → 24 个（启动时逐个解析，量大很慢）|
| 6 | 关播放列表子标签 | `playlist_show_sublabels = "false"` | 翻列表更快 |

### 剩余约 10 秒说明

Vita（2011 年硬件）跑 RetroArch 1.22 的固有开销：前端初始化 + GPU 初始化 + 内置占位核心（dummy core）初始化。**这基本是这台机器跑最新版的地板了**，无法再明显压缩。

### ROM 加载速度说明

游戏加载耗时 = 核心加载解压 + ROM 解压 + 游戏初始化，其中**核心体积是大头**：

| 核心 | 体积 | 用途 |
|---|---|---|
| FB Alpha 2012 CPS-1/CPS-2/Neo Geo | 各约 2.9MB | 普通街机游戏首选，加载最快 |
| MAME 2003+ | 11.5MB | MAME 格式 ROM 和部分 hack 游戏 |
| FinalBurn Neo | 20.5MB | 名将改版（IPS）必需，加载最慢 |

已把 CPS1/NeoGeo 播放列表的默认核心改为专用小核心（原列表备份在 `playlists_backup/`）。**注意**：名将改版（战神/梦魇等）必须用 FBNeo，加载慢是 IPS 机制的固有代价。

其他技巧：
- 连续玩同一个核心的游戏时核心驻留内存，换游戏不重载，明显更快
- 核心选项里的 `fbneo-cyclone`（旋风）开启后 CPS 游戏运行更流畅（影响精度，按需开）

**实测结论（2026-08-29）**：试过把 ROM 包转成零压缩（STORED）格式跳过解压，三国志2 和 KOF98 加载时间**均无改善**——瓶颈不在解压，而在 Vita 的 CPU 图形解码（NeoGeo 大游戏要解码 90MB 级的图形数据）。ROM 加载时间属于硬件物理极限，无法通过配置继续优化。

### 如需调整

- **界面语言**：当前为简体中文（`user_language = "12"`）+ rgui 菜单。rgui 的中文字库是**外置 .bin 文件**，位于 `ux0:/data/retroarch/assets/rgui/font/`（bitmap10x10_chn.bin 等 7 个文件，从官方 [assets.zip](https://buildbot.libretro.com/assets/frontend/assets.zip) 中提取）——**缺了这些文件中文会显示不出来**（自动回退英文）。换机/重装后记得恢复该目录
- 想找回某个核心：从 `ux0:/core_backup/` 把对应 `.self` 移回 `ux0:/app/RETROVITA/` 即可
- 若以后改用 ozone/xmb 菜单（更华丽但明显更慢），中文界面会额外加载 4.5MB TTF 字体，Vita 上启动和操作都会变慢，**不推荐**

---

## 四、游戏卡顿优化（惩罚者改版、名将梦魇敌人多时）

### 已生效：FBNeo Cyclone 68K 加速

`fbneo-cyclone = "enabled"` 已全局开启（见 `config/FinalBurn Neo.opt`）。Cyclone 是 ARM 汇编版 68K 解释器，Vita（ARM 架构）跑 CPS1/CPS2/NeoGeo 的 68K 模拟提速明显。代价是精度略降，个别游戏若开启后出现异常（花屏/崩溃），告诉我为其单独关闭。

### 推荐安装：PSVShell 系统超频（CPU 444 → 500MHz）

插件文件已放 `ux0:/leonxing/PSVshell.skprx`（v1.1 稳定版）。因 tai 配置在 ur0 分区（USB 无法访问），需在 VitaShell 手动安装：

1. VitaShell 中把 `ux0:/leonxing/PSVshell.skprx` 复制到 `ur0:tai/`
2. 编辑 `ur0:tai/config.txt`，在 `*KERNEL` 段（没有就新建）添加一行：`ur0:tai/PSVshell.skprx`
3. 重启（Enso 用户直接关机开机）

用法（实测 v1.1 在 3.72 固件可用，README 说仅 3.60/3.65 偏保守）：游戏内点按 **SELECT+↑ 或 SELECT+↓** 循环切换 关闭 → HUD → 完整菜单（注意 SELECT 是街机投币键，呼出时会顺便投币，无碍）。完整菜单操作：**↑/↓** 移动光标到 `CPU` 行 → **X** 切换成蓝色 Manual → **←/→** 调到 **500MHz**（ES4/BUS 保持 222，XBR 166 即可）→ 光标移到 `save profile` → **按住 L 再按 X** = 保存为全局默认（之后每个游戏自动应用）。菜单里还能实时看 FPS/内存，方便对比效果。

### 后备手段（卡顿仍明显时）

- **跳帧**：核心选项 `fbneo-frameskip-type` 设为 auto（丢帧保速度，流畅但动画不平滑）
- **降采样率**：`fbneo-samplerate` 从 48000 降到 22050
- 注意：`fbneo-cpu-speed-adjust` 是"游戏自身 CPU 超频"（让游戏内动作加速），**不是**提升模拟性能，卡顿时别动它

## 五、其他要点

### 原来"FB Alpha"核心的游戏怎么办

安装官方 1.22.1 时旧应用目录被清空，而 FBAlpha 核心 2019 年起就不在官方包里。**受影响的街机游戏改用 FB Alpha 2012 或 FinalBurn Neo 核心打开**即可（ROM 集基本兼容）。

### 目录速查

| 路径 | 内容 |
|---|---|
| `ux0:/roms/HACK/` | 改版游戏目录（含其他 MAME2003+ 格式 hack）|
| `ux0:/data/retroarch/system/fbneo/ips/` | IPS 补丁根目录 |
| `ux0:/core_backup/` | 被精简掉的核心（含 MOVED_CORES.txt 清单）|
| `ux0:/leonxing/` | 安装包存放处（RetroArch_1.22.1.vpk 已可删除）|

### 本机备份（D:\game\emulator\game\psv\）

PSV 出问题或换机时的完整恢复材料：

| 文件/目录 | 用途 |
|---|---|
| `PSV名将改版与模拟器优化指南.md` | 本文档 |
| `ips/captcomm/`（86 个文件）| 全部 17 个名将改版补丁，恢复时整体复制到 `ux0:/data/retroarch/system/fbneo/ips/captcomm/` |
| `roms/` | 原版 ROM 本地备份：captcomm（在 D:\game\emulator\名将战神版\roms）、ffightae（三十周年）、slapfigh、thndzone。恢复时放入 `system/fbneo/arcade/` |
| `playlists/收藏.lpl` | 收藏列表：Slap Fight（Toaplan 竖版射击 1986）、Thunder Zone（Data East 雷鸣战区 1990），ROM 在 arcade 目录 |

**RetroArch 安装包**（521MB，不本地保存，需要时下载）：

```
https://buildbot.libretro.com/stable/1.22.1/playstation/vita/RetroArch.vpk
```

> 也可用更新的稳定版（把 URL 里的 `1.22.1` 换成最新版本号），Vita 版各稳定版地址格式相同：
> `https://buildbot.libretro.com/stable/<版本号>/playstation/vita/RetroArch.vpk`

**恢复流程**：下载 vpk 并用 VitaShell 安装 → 复制 ips 目录 → 复制 ROM → 按本文档第二章操作即可。

### 想加其他游戏的改版

去 [taoenwen/FBNeo_IPS](https://github.com/taoenwen/FBNeo_IPS) 仓库，按平台（CPS/NeoGeo/PGM 等）找到对应游戏的文件夹，把 `.dat` 文件和同名子文件夹放进 `ux0:/data/retroarch/system/fbneo/ips/<游戏ROM名>/` 即可（注意 relay 依赖文件夹也要一并下载）。

### 常见问题

- **快打旋风"三十周年版"（ffightae.zip）玩起来像原版**：ROM 本身是正确的（文件 CRC 与 MAME 2003-Plus 驱动完全匹配，缺失的音频文件由父 set ffight.zip 自动补齐）。这个版本（originalgrego 的 Final Fight AE）主打**三人同时游戏**（PSV 单机玩不出区别）、选人界面调色板、无审查开场——单人玩视觉差异确实很小，属正常现象
- **快打旋风 命运无双 2016（patched 目录方式，2026-08-30 新增）**：完整改版 ROM 已放 `system/fbneo/patched/ffightj2.zip`，触发用的空壳 zip 在 `system/fbneo/arcade/ffightj2.zip`，列表入口在「快打旋风改版」。原理：改版冒用官方日版 set 名 ffightj2 且程序文件 CRC 不匹配（FBNeo 严格校验会拒绝），patched 机制按文件名+大小加载且不校验 CRC。以后其他"MAME set 名+改文件"式的游聚 ROM 都可用此法接入
- **IPS Patch 选项不出现**：确认游戏是用 FinalBurn Neo 核心加载的原版 `captcomm.zip`；检查系统目录设置指向 `ux0:/data/retroarch/system`
- **改版启用后花屏/崩溃**：确认只启用了一个补丁；确认 relay 文件夹（captcommr1/）完整
- **核心加载报 C2-12828-1**：核心与前端版本不匹配或核心过大内存不足——本文方案已用配套版本解决，勿单独替换核心文件
