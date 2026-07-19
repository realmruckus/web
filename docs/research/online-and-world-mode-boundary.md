# Online Session 与 WORLD 模式边界说明

归档日期：2026-07-19  
性质：参考索引与概念边界，不构成规则、架构、依赖或开发状态变更

## 两份参考均保留

Realm Ruckus 当前保留两类不同但都需要的参考：

### 1. Online Session 联机技术架构

文件：

```text
docs/research/online-mode-architecture-reference.md
```

该文档说明如何让一套既有 Core 规则通过服务器权威裁决、房间、WebSocket、隐藏信息视图、确定性随机、断线重连和 Replay 在线运行。

它回答的是：

> 一套游戏规则如何通过网络让多人参与？

该参考继续保留，不删除、不替换。

### 2. WORLD 持续多人卡牌 RPG 游戏模式

正式设计参考位于 Core 独立目录：

```text
realmruckus/core/docs/research/world/
```

当前包括：

```text
overview.md
asynchronous-territory-conflict.md
```

`overview.md` 描述持续角色、等级、金币、装备、个人卡组、行动力、专业成长、区域资源、Boss、公会与区域竞争等游戏机制。玩家通过狩猎、采集、探索、攻击、建设等卡牌行动与持续世界交互。

`asynchronous-territory-conflict.md` 记录按个人进度展开时的临时占领、时间线汇合、异步夺取与历史收益不回滚机制建议。

它回答的是：

> Realm Ruckus 可以增加怎样的持续多人在线游戏模式？

## 关系

```text
规则／游戏模式
├─ CLASSIC
└─ WORLD

参与与运行方式
├─ LOCAL
├─ AI
└─ ONLINE SESSION
```

因此：

- `CLASSIC + ONLINE SESSION`：经典规则的联网对局；
- `WORLD + ONLINE SESSION`：持续多人卡牌 RPG 世界通过网络运行；
- Online Session 不是 WORLD 的替代品；
- WORLD 也不替代既有联机技术架构；
- 两者未来可以组合，但由不同正式来源分别定义。

## 仓库边界

- Core 维护 CLASSIC、WORLD 等正式模式的通用规则、机制、数据和 Engine；
- Web 维护这些模式的在线展示、Session、服务器适配、UI 与部署；
- Web 不复制 WORLD 或 CLASSIC 的正式规则；
- Web 消费 Core 内容时必须通过 `dependencies.lock.json` 固定明确 Commit。

## 使用限制

本说明不代表：

- WORLD 已正式批准；
- Online Session 已实现；
- 已修改任何依赖锁；
- 已启动 WORLD 或联机功能开发；
- 已建立服务器、数据库、WebSocket 或对局 UI。