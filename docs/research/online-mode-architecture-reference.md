# Realm Ruckus Online 模式架构参考

归档日期：2026-07-19  
性质：未来实现参考，不构成已批准的规则、架构、依赖或开发状态变更

## 一、基本原则

Online 模式不是独立规则，而是同一套 Core 规则通过联网房间、服务器裁决、隐藏信息控制和实时同步运行。

```text
Core
规则、状态机、Engine、合法行动、随机种子、胜负判断
        ↓
Online Game Server
房间、玩家身份、计时、行动验证、状态同步、断线恢复
        ↓
Web
卡牌 UI、区域布局、操作交互、动画、聊天、观战
```

应区分：

- `format_id`：决定游戏规则，例如 `CLASSIC`、`ADVANCED`；
- `session_mode`：决定参与方式，例如 `LOCAL`、`ONLINE_PRIVATE`、`ONLINE_MATCHMAKING`、`AI`、`SPECTATOR`。

示例：

```json
{
  "format_id": "CLASSIC",
  "session_mode": "ONLINE_PRIVATE"
}
```

Online 不重新定义部署、攻击、区域回流、技能触发或胜利条件。

## 二、服务器权威裁决

客户端只能提交行动意图，不能决定结果。

```text
玩家操作
  ↓
客户端发送 Command
  ↓
服务器读取当前 GameState
  ↓
Core Engine 校验并执行
  ↓
产生 Events
  ↓
服务器保存新状态
  ↓
广播玩家视角更新
```

客户端提交示例：

```json
{
  "command_id": "CMD-1042",
  "game_id": "GAME-7281",
  "player_id": "P1",
  "type": "DEPLOY_UNIT",
  "payload": {
    "card_instance_id": "CARD-31",
    "region_instance_id": "REGION-07"
  },
  "expected_revision": 18
}
```

服务器必须重新校验回合归属、手牌、费用、区域容量、目标合法性、响应时机和胜负条件。

## 三、Command 与 Event

推荐使用确定性的 `Command → Event` 架构。

常见 Command：

- `DEPLOY_UNIT`
- `DECLARE_ATTACK`
- `PLAY_EFFECT`
- `EQUIP_UNIT`
- `BUILD_FACILITY`
- `PASS_ACTION`
- `RESPOND_COUNTER`

常见 Event：

- `CARD_DRAWN`
- `UNIT_DEPLOYED`
- `ATTACK_DECLARED`
- `DAMAGE_DEALT`
- `UNIT_DEFEATED`
- `REGION_CONTROL_CHANGED`
- `REGION_RETURNED`
- `REGION_REVEALED`
- `ABILITY_TRIGGERED`
- `GAME_ENDED`

Web 根据 Events 播放动画，不自行推导规则结果。该结构应支持在线同步、AI、Simulation、Replay、断线恢复和 Bug 重现。

## 四、隐藏信息与玩家视角

服务器保存完整状态，但只向每个连接返回其合法视角。

建议提供：

```text
getPlayerView(gameState, viewerId)
getSpectatorView(gameState)
```

玩家只能看到自己的手牌，对手只显示手牌数量；观战者默认不能查看任何玩家私有信息。不能把完整状态发到浏览器后仅通过 UI 隐藏。

## 五、确定性随机

洗牌、抽牌、随机目标、随机先手和区域回流后的重洗必须由服务器使用固定种子执行。

```json
{
  "game_id": "GAME-7281",
  "seed": "8ef2a91c",
  "rng_cursor": 37
}
```

随机事件应记录前后游标，支持确定性回放、作弊争议核查和线上 Bug 重现。

## 六、状态版本与幂等

每次有效行动后增加状态版本号。Command 应携带 `expected_revision` 和唯一 `command_id`。

若客户端版本落后，服务器返回 `STALE_STATE` 并要求重新同步。相同 Command 重试时不得重复执行。

## 七、通讯与重连

推荐：

- HTTP：登录、创建或加入房间、获取快照、查询历史；
- WebSocket：游戏行动、状态更新、响应窗口、倒计时、在线状态和聊天。

重连时客户端提交：

```text
game_id + player_token + last_revision
```

服务器返回最新玩家视角快照或缺失 Events。

## 八、反制与响应窗口

Online 不采用自由抢响应，应由服务器进入明确的响应状态：

```text
玩家使用效果
  ↓
RESPONSE_WINDOW
  ↓
按顺序询问有资格的玩家
  ↓
反制或放弃
  ↓
全部放弃后结算
```

建议首版限制：

- 只有明确具备反制时机的牌可响应；
- 每名玩家一次响应机会；
- 超时自动放弃；
- 暂不支持无限响应链；
- 最多一个反制层。

## 九、计时、断线与托管

建议房间可配置回合时间：

| 模式 | 建议时间 |
|---|---:|
| Casual | 90 秒 |
| Standard | 45 秒 |
| Ranked | 30 秒 |
| Async | 数小时至一天 |

超时优先自动 `PASS_ACTION`；响应窗口超时自动放弃。短暂断线保留席位并允许重连，较长断线进入自动 Pass。AI 接管仅在房间创建时明确允许，并必须通过 Core 合法行动接口运行。

## 十、房间模式

首阶段建议：

1. `Private Room`：邀请码、2–4 人、选择 Format、可选 AI 和计时；
2. `Quick Match`：固定人数、Format 和计时；
3. `Practice`：玩家对 AI，可重开，不计排名。

Ranked、Tournament、Spectator 和异步模式后置。

## 十一、Web 对局交互

建议页面结构：

```text
对手信息
公开区域与已控制区域
区域牌、驻军、设施和状态
自己的手牌、资源和行动按钮
```

部署流程：选择手牌单位 → 高亮合法区域 → 点击目标 → 确认 → 发送 Command。

攻击流程：选择己方区域 → 选择进攻单位 → 高亮合法目标 → 确认 → 发送 Command。

前端可预览合法目标，但服务器必须重新验证。

## 十二、仓库职责

### Core

负责：

- GameState；
- Command、Event；
- 合法行动生成；
- 随机模型；
- 隐藏信息视图规则；
- 确定性 Replay；
- AI、Simulation、Tests。

### Web

负责：

- 登录和大厅；
- 房间、匹配与 Session；
- WebSocket 客户端与网站专属服务器层；
- 对局 UI、动画、聊天和观战；
- 网站数据库与部署。

Web 不复制 Core 规则。使用 Core 时必须通过 `dependencies.lock.json` 固定明确 Commit，不得长期跟踪 `main`。

Production 不承担在线服务器或游戏运行逻辑。

## 十三、数据保存建议

建议保存：

- Game Session：Format、Core Commit、规则版本、Seed、状态和 Revision；
- Commands：玩家意图与输入版本；
- Events：公开和私有事件载荷；
- Snapshots：定期保存完整状态，缩短重连和回放恢复时间。

对局记录必须包含所用 Core Commit 和规则版本，保证未来仍可准确回放。

## 十四、MVP 范围

首版 Online 建议只支持：

- 2 人；
- Classic Format；
- 好友邀请码房间；
- 服务器权威；
- WebSocket 同步；
- 断线重连；
- 确定性随机；
- Replay 事件记录；
- 基础反制窗口。

暂不包含：

- 4 人自动匹配；
- Advanced；
- 装备和设施；
- 排名赛；
- 赛事系统；
- 复杂观战；
- 自定义卡组。

## 十五、推荐实施顺序

```text
1. Core Engine 可完整运行一局
2. 确定性 Command / Event / Replay
3. 玩家视角与隐藏信息过滤
4. 本地双客户端模拟
5. Online 房间服务器
6. WebSocket 同步
7. Web 对局 UI
8. 断线重连
9. AI 与匹配
10. Advanced Format
```

架构原则：

> Core 决定发生什么，服务器决定谁能在何时请求发生，Web 负责让玩家看见并操作。

## 十六、使用限制

本文件仅为未来 Online 实现参考，不代表：

- 已建立 Online 服务器；
- 已完成 WebSocket、房间、匹配或数据库；
- 已修改 Core Engine；
- 已批准新的规则 Format；
- 已更新任何依赖锁；
- 已进入开发排期或 Milestone。