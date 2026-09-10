# Agent Note: Hero composer 增量 dock

Status: implemented

[English](2026-09-09-hero-composer-additive-dock.md) | 中文

## 问题

常驻 composer 在活跃会话的卡片下方提供一个增量列表，但空白会话使用同一个 InputBar 的 Hero variant。复用活跃 dock 会让活跃会话条目出现在 Hero 中，并改变现有扩展点的含义。替换 composer 或把内容定位到其上方，还会让增量功能控制常驻输入行为或几何信息。

## 决策

`conversation.hero.composer.dock` 是由 `conversation.composer.bar` 声明的会话级列表。仅当 InputBar 的 variant 为 `hero`、输入状态存在且具有真实会话 id 时，常驻 InputBar 才会在其卡片之后的普通文档流中立即渲染该列表。卡片保持挂载，列表不会收到 placement 或 geometry props。

活跃的 `composer` variant 仍只渲染 `conversation.composer.dock`。缺少会话时，两个会话级 dock 都不会渲染。列表排序仍由 slot 注册表按 `order` 升序处理，值相同时按注册顺序处理。

## 备选方案

**复用 `conversation.composer.dock`。** 不予采纳，因为现有 occupant 面向活跃 composer，会在没有主动选择该产品语境的情况下出现在 Hero 中。

**为现有 dock 添加 placement 选项。** 不予采纳，因为 placement 会把两个生命周期语境混入同一个 slot，并要求每个 occupant 理解宿主布局。

**公开 DOM anchor 或 composer geometry。** 不予采纳，因为增量条目只需要普通文档流组合；几何信息会把插件与常驻 composer 的实现耦合。

**替换 Hero composer。** 不予采纳，因为扩展内容不拥有输入行为，也不得移除常驻 InputBar。

## 影响

Hero 专属功能获得一个有序增量位置，同时不改变活跃 composer 语义、输入行为或 Session phase 逻辑。有意同时支持 Hero 与活跃语境的贡献方必须分别注册。该 slot 仅在 composer bar 声明存在时可用，并且在没有真实 Session 时不渲染任何内容。
