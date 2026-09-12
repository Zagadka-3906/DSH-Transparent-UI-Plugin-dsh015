# Aqua 玻璃主题插件（DSH 0.1.5 移植版）

移植自 [WYH66666666/DSH-Transparent-UI-Plugin](https://github.com/WYH66666666/DSH-Transparent-UI-Plugin) v1.3.1，适配 DSH **0.1.5-rc.1**（npm `latest`）客户端 API，已实测通过。上游最后发布于 2026-08-17，面向旧版 API，在新版 DSH 上会导致 web profile 无法启动。

## 安装

一行命令，自动安装并注册到 web profile（装完重启 DSH 生效）：

```sh
dsh plugin --profile web add https://github.com/Zagadka-3906/DSH-Transparent-UI-Plugin-dsh015.git
```

`dsh` 不在 PATH 时，把命令开头换成 `npx @deepseek-ai/dsh plugin --profile web add …` 即可。

## 使用

总开关在 **设置 → 通用设置 → 玻璃主题**，关闭即完整恢复原生 UI。云母/兼容模式、模糊度、磨砂度、流体色调、背景亮度、壁纸、粒子鲸鱼、悬停效果等全部调节项都在总开关下方。

## 相对上游 v1.3.1 的改动

本仓库发布构建产物（`lib/`），改动共三处：

1. **store 模块迁移**（`lib/client.js`）：`require("@deepseek-ai/dsh-client-runtime/client")` → `require("@deepseek-ai/dsh-client-store")`。旧包自 DSH 0.1.2 起移除，其唯一被用到的导出 `defineStore` 已原样迁入新包（两版函数体逐字一致），由前端核心注册为浏览器模块。
2. **清单声明**（`package.json`）：`dsh.client.inject` 从 6 个旧 roster 包改为现存的 `ui-theme` / `locale` / `ui-settings` 三个，`peerDependencies` 同步。
3. **总开关迁址**（`lib/client.js`）：0.1.5 的「插件配置」页改为按 Host 设置命名空间分发的键控槽位，第三方无 `key` 卡片不再渲染，而那是主题唯一总开关。故将开关卡片（`id: "aqua-master"`，`order: 9`，独立 store）额外注册进通用设置，与外观旋钮行双向同步；原插件页注册保留，若日后恢复旧机制可自动复活。

如需从源码重建（上游 npm 包不含 `src/`），按上述三处在 `src/client/index.ts`、`src/client/settings-store.ts` 与 `package.json` 复现同样改动即可。

## 兼容性

| DSH 版本 | 状态 |
| --- | --- |
| 0.1.5-rc.1 | ✅ 实测通过 |
| 0.1.5-rc.2 | 理论可用（未实测） |
| ≤ 0.1.1-rc.2 | 请用上游原版 |

## 许可

AGPL-3.0（遵循上游）。
