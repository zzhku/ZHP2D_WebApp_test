# ZHP2D Web 控制台 — AI 项目说明（AGENTS.md）

本仓库是 ZHP2D 移动电源的 **Web Bluetooth 控制台**（单文件 `index.html`；DATA 折线图依赖 ECharts CDN）。

## 设置同步契约（改设置前必读）

设置项契约以**固件** `../ZHP2D/main/web/web_cfg_table.c` 为唯一权威，
完整规则见 **[`../ZHP2D/WEBAPP_SYNC_SPEC.md`](../ZHP2D/WEBAPP_SYNC_SPEC.md)**。要点：

- `SETTING_TREE`（`index.html` 约 1110 行起）叶子 `key` 必须等于固件 DEF 名
- ui 类型映射：DEF_UINT→spinbox/slider、DEF_RNG→range、DEF_BOOL→switch、DEF_ENUM→roller(options 与固件 E_XXX 顺序一致)、只读→label
- min/max/step/unit 与固件完全一致；固件改范围必须同步
- `hidden:{key,eq}` 语义同 scr2 `hidden_condition`；固件菜单分组重组后本树同步重组
- 动作类菜单项(库伦重置/重启等)不进本树；只读状态用 label
- 后台自动键(如 sync_time 自动对时)不进树: 声明进 `NO_UI_KEYS`, 由自动流程直接 set config
- 改完运行 WEBAPP_SYNC_SPEC.md 第 7 节的键名对比脚本，两侧差值必须为空

## 本仓库约束

- 单文件静态站点（无构建系统/框架）；DATA 折线图依赖 ECharts（经 CDN 引入 `echarts.min.js`，断网时该图表不可用，其余功能仍可离线）；改动只在 `index.html`
- BLE 交互协议跟随固件 `web_dispatch.c` 的 JSON 格式，键名不得自行发明
- 中文 commit message；与固件改动相关联的提交互相标注
