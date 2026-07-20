# Online Session 与 WORLD 模式边界说明

更新日期：2026-07-20  
性质：正式仓库边界索引，不构成 WORLD 规则、数值或 Online Session 实现批准

## 两类对象

### Online Session 联机技术架构

Web 中的参考：

```text
docs/research/online-mode-architecture-reference.md
```

该文档说明如何让既有规则通过服务器权威裁决、房间、WebSocket、隐藏信息视图、确定性随机、断线重连和 Replay 在线运行。

它回答：

> 一套既有游戏规则如何通过网络让多人参与？

### WORLD 独立游戏

WORLD 已建立独立仓库：

```text
realmruckus/world
```

WORLD 是异步共享世界怪物养成与领地争夺 RPG。其根目录是产品网站根目录，同时维护：

- WORLD 游戏应用与专属 JavaScript Engine；
- WORLD 产品专属规则与数据；
- World Tick、Event Composer、持续车怪物卡、地点、驻军和异步领地结算；
- WORLD 设计文档网页；
- WORLD 版本、迭代和长期产品状态网页；
- WORLD 专属部署与后续数据库适配。

WORLD 网站与设计入口：

```text
realmruckus/world/index.html
realmruckus/world/docs/index.html
```

## 关系

```text
通用共享输入
└─ Core

独立产品
├─ Classic 桌游主题与实体产品
├─ Web 官方展示网站
└─ WORLD 独立网页游戏

通用联网参与方式参考
└─ Online Session
```

因此：

- `CLASSIC + ONLINE SESSION` 表示经典规则的联网对局；
- WORLD 自己维护其异步在线游戏应用，不由 `realmruckus/web` 承载；
- Online Session 参考不替代 WORLD；
- WORLD 也不替代 Web 的官方展示网站和既有联机技术参考。

## 仓库边界

- Core 维护跨产品通用的 Mechanic、Effect、数学模型、产品元数据、AI、Simulation、Tests 和项目基线；
- WORLD 维护 WORLD 产品专属应用、网站、规则适配、数据、JS Engine、事件内容、UI、版本与部署；
- Web 维护 Realm Ruckus 官方网站、在线规则展示、SEO 与官方部署；
- Web 不复制 WORLD 产品文档、专属规则或游戏应用；
- WORLD 和 Web 消费 Core 内容时，均使用 `dependencies.lock.json` 固定明确 Commit。

## 使用限制

本说明不代表：

- WORLD 已完成 MVP；
- 已批准 WORLD 正式数值、卡表或资产；
- 已实现正式账号、数据库、异步玩家投影或服务端权威结算；
- Online Session 已完成实现；
- WORLD 已形成 Release、Freeze 或 Baseline。
