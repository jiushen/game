# Switch RetroArch 部署备忘

## 已解决问题

### 1. 播放列表部分中文显示方块（如「战纪」）

- 原因：Ozone 菜单默认字体缺简体字形（界面自带文字正常，列表自定义文字方块）
- 修复：完整中文字体 `wqy-microhei.ttc`（文泉驿微米黑，本目录 `ozone/` 下有备份）
  放到 TF 卡 `/retroarch/assets/ozone/wqy-microhei.ttc`，retroarch.cfg 设置：
  ```
  ozone_font = "/retroarch/assets/ozone/wqy-microhei.ttc"
  ```

### 2. 游戏画面叠加触屏软按键（A/B/X/Y/START 虚拟按键）

- 原因：Switch 触屏 overlay 默认开启
- 修复：retroarch.cfg 设置 `input_overlay_enable = "false"`
  （实体手柄游玩不需要触屏按键；若将来要用触屏改回 true 即可）

### 3. TF 卡空间不足

- ROM 目录与 arcade 双份重复问题：旧列表路径已改指 arcade，ROM 目录街机副本已删
- 128GB 卡部署后剩余约 2.8GB

## 关键路径

- RA 1.21.0 NRO：`/switch/retroarch_switch.nro`（从 hbmenu 启动；桌面旧图标是 1.19 勿用）
- ROM：`/retroarch/system/fbneo/arcade/`（397 个）
- 列表：`/retroarch/playlists/`
- 桌面直达图标：hbmenu 运行 nsp-forwarder → NRO Forwarder → 目标 retroarch_switch.nro
