# DSH Transparent UI Plugin — DSH 0.1.5 移植版

Aqua 玻璃主题插件（`dsh-client-ui-aqua`）针对 **DeepSeek Harness (DSH) 0.1.5-rc.1** 客户端 API 的非官方移植版。

移植自 [WYH66666666/DSH-Transparent-UI-Plugin](https://github.com/WYH66666666/DSH-Transparent-UI-Plugin) **v1.3.1**。上游最后一次发布是 2026-08-17，针对 DSH 0.1.0-rc.6 时代的客户端 API；DSH 从 0.1.2 起重构了客户端包结构，原版插件在新版上会把整个 web profile 搞挂。本仓库将其移植到新版 API，并在 DSH 0.1.5-rc.1（npm `latest`）上完成实测。

## 相对上游 v1.3.1 的改动

本仓库发布的是构建产物（`lib/`），与上游 npm 包结构一致。实际改动只有三处：

1. **store 模块迁移**（`lib/client.js`）
   `require("@deepseek-ai/dsh-client-runtime/client")` → `require("@deepseek-ai/dsh-client-store")`。
   `dsh-client-runtime` 包自 DSH 0.1.2 起移除，其唯一被本插件用到的导出 `defineStore` 已原样迁入 `@deepseek-ai/dsh-client-store`（两版函数体逐字一致），由 DSH 前端核心注册为浏览器模块。

2. **profile 清单声明**（`package.json`）
   `dsh.client.inject` 从 6 个旧 roster 包（`dsh-client-runtime`、`dsh-client-ui-slots`、`dsh-client-ui-primitives` 等已不存在于新版包集合）改为 3 个现存的 roster 包：`@deepseek-ai/dsh-client-ui-theme`、`@deepseek-ai/dsh-client-locale`、`@deepseek-ai/dsh-client-ui-settings`；`peerDependencies` 同步更新。

3. **总开关迁址**（`lib/client.js`）
   DSH 0.1.5 的「设置 → 插件 → 插件配置」页改为按 Host 设置命名空间分发的**键控槽位**（卡片必须带 `key` 且该 key 被 Host 服务），旧式无 `key` 的第三方插件卡片不再渲染——而那张卡是主题唯一的总开关。因此将总开关卡片（`AquaPluginCard`）额外注册进**设置 → 通用设置**（`settings.general.item`，`id: "aqua-master"`，`order: 9`，独立 store，与外观旋钮行双向同步）。原插件页注册保留（当前为死代码，若上游恢复旧分发机制可自动复活）。

> 注意：上游 TS 源码（`src/`）未包含在原 npm 包中，本移植直接修改构建产物。如需从源码重建，请按上述三处在 `src/client/index.ts`、`src/client/settings-store.ts` 与 `package.json` 复现同样改动。

## 安装（手动方式，已在 Windows 实测）

1. 克隆本仓库，将内容放入（或软链接到）`$DSH_HOME/profiles/web/node_modules/dsh-client-ui-aqua`；
2. 编辑 `$DSH_HOME/profiles/web/package.json`：
   - `dsh.profile.bundles` 数组加入 `"dsh-client-ui-aqua"`；
   - `dependencies` 加入 `"dsh-client-ui-aqua": "^1.3.1-dsh015.1"`；
3. 重启 DSH。

也可尝试 `dsh plugin --profile web add <本仓库路径>`（该命令内部转发给 pnpm，未实测）。

## 使用

- 总开关：**设置 → 通用设置 → 玻璃主题**。关闭后所有玻璃层收起，原生 UI 完整恢复；
- 全部调节项（云母/兼容模式、模糊度、磨砂度、流体色调、背景亮度、壁纸、粒子鲸鱼、悬停效果等）都在总开关下方的通用设置里。

## 兼容性

| DSH 版本 | 状态 |
| --- | --- |
| 0.1.5-rc.1（npm `latest`） | ✅ 实测通过 |
| 0.1.5-rc.2 | 理论可用（未实测） |
| ≤ 0.1.1-rc.2 | 用上游原版，不要用本移植 |

## 许可

AGPL-3.0（遵循上游）。非官方移植，与上游作者及 DeepSeek 无关联。
