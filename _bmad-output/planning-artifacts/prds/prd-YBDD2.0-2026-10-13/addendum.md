---
title: FR-022 首页金刚区与点击分析 · Downstream Addendum
status: final
created: 2026-10-13
updated: 2026-10-13
---

# Addendum

本文件保留 PRD 不应承载的实现约束，供架构、数据和 Epic 拆解使用。它不改变 PRD 的能力边界。

## 1. 推荐能力契约

### 创作者首页

在现有 `创作者首页` 能力中增加：

```text
quickEntries[] {
  entryId,
  configVersion,
  icon,
  title,
  subtitle,
  jumpType,
  jumpTarget,
  sort
}
```

BFF 只下发已启用、当前有效、通过跳转校验的入口。C 端不自行拼接入口，也不以客户端配置覆盖服务端有效性判断。

### 首页金刚区配置保存

调用方为运营后台，保存结果至少返回生效版本号或更新时间。保存操作需要原子提交，并触发 BFF 缓存失效或进入短 TTL 窗口。

### 首页金刚区点击上报

建议事件字段：

```text
eventId
entryId
configVersion
userId
source = home_jingang
position
clickTime
jumpType
jumpResult
```

`eventId` 是幂等键。上报失败不阻塞跳转，客户端可以异步重试；服务端或事件平台负责最终去重。

## 2. 数据模型建议

- `HomeQuickEntry`：当前有效配置及其版本信息；
- `HomeQuickEntryAudit`：保存、删除、恢复、启停等操作审计；
- `HomeQuickEntryClickEvent`：原始点击事件，`eventId` 唯一；
- `HomeQuickEntryDailyAgg`：可选的按入口、日期聚合投影，用于报表趋势和查询性能。

点击次数不直接写入配置表累加。点击人数由 `entryId + userId + 统计范围` 去重计算，避免配置写入和统计写入互相污染。

## 3. 报表与导出建议

报表至少分为三个区域：

1. 汇总区：累计或时间范围内的点击次数、点击人数；
2. 趋势区：按 Asia/Shanghai 自然日展示每日点击次数和点击人数；
3. 明细区：入口、时间、配置版本、跳转结果、脱敏用户标识。

导出请求应携带当前筛选条件、数据类型（汇总/明细）和报表口径版本。若数据量超过同步导出上限，转为异步任务并在下载中心提供结果。同步上限、格式和保留周期待架构阶段确认。

## 4. 兼容与风险

- FR-021 首页 BFF DTO 的新增字段应向后兼容：旧客户端忽略 `quickEntries` 时不影响原首页；
- 后台保存即生效提高了误配置风险，必须在保存前完成必填、路由、域名和时间范围校验；
- 删除或停用入口不应删除历史点击事件，历史报表仍按 `entryId` 保留；
- 入口标题或图标变化不应改变 `entryId`，否则累计数据会被切断；
- 用户明细仅用于有权限的运营场景，导出必须脱敏并审计。

