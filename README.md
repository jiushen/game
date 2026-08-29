# PSV 街机改版游戏备份

破解 PSV（RetroArch + FBNeo 核心）玩街机 IPS 改版游戏的完整配置备份。

## 目录结构

```
psv/
├── PSV名将改版与模拟器优化指南.md   ← 主文档：环境、玩法、优化、恢复全流程
├── ips/          300 个 IPS 改版的 .dat 描述文件（23 个游戏，详见 docs/ips改版清单.json）
│                  ⚠️ .ips 补丁本体不进 git，恢复时从 taoenwen/FBNeo_IPS 下载
├── playlists/    RetroArch 播放列表（10 个改版列表 + 街机改版，点条目直接进改版）
├── patched/      patched 目录机制用的改版 ROM（快打旋风 命运无双 2016）
├── config/       FBNeo 核心选项（FinalBurn Neo.opt）
├── docs/         FBNeo-PLUS 使用说明、IPS 改版清单
└── roms/         ffightae.zip（快打旋风 三十周年版 ROM）
```

## 恢复速查

1. PSV 安装 RetroArch（stable ≥1.19，含 FBNeo 核心）——详见主文档
2. `ips/` 恢复：从 [taoenwen/FBNeo_IPS](https://github.com/taoenwen/FBNeo_IPS) 下载各游戏文件夹（CPS/NeoGeo/PGM），与本地 .dat 合并后放入 `ux0:/data/retroarch/system/fbneo/ips/`
3. `playlists/` → `ux0:/data/retroarch/playlists/`
4. `patched/` → `ux0:/data/retroarch/system/fbneo/patched/`（另在 arcade/ 目录建同名空壳 zip 触发加载）
5. 原版 ROM：见主文档"本机备份"章节的下载源

详细说明、原理、故障排查见 `psv/PSV名将改版与模拟器优化指南.md`。
