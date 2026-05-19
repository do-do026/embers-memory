# 渡渡架构思考 — 情绪记忆系统

> 由云端Hermes agent产出，Operit渡渡通过SSH调用
> 完整版本：~/.hermes/design-foundations/architecture-thinking.md（481行）
> 本文件为GitHub公开备份

## 命名：Rivulet（溪流）
情绪记忆像溪水，会流动、会冲刷石头、会干涸、会再涨。基石记忆是溪底的石头——水干了它们还在那里。

## 技术架构
独立HTTP服务（Python/FastAPI），8801端口（stdio MCP给Hermes），可选8802端口（HTTP REST给Operit经SSH隧道访问）。SQLite（WAL模式）作为存储。

## 数据模型（4表+1FTS5）
- memories：核心记忆表（subject/type/status/heat_score/emotional_tags）
- links：双链关系表
- annotations：双向批注表（记忆↔日记）
- heat_log：热度变化日志
- FTS5虚拟表：全文搜索

## 浮现引擎
热度 = base_heat × recency_factor × context_relevance。触发条件：MCP tool调用、关键词检测、日记webhook、定时衰减。

## 沉降规则
- sedimentable: 30天无访问→sedimented, 90天→sunken
- protected: 永远active
- 关联沉降保护：被基石记忆锚定的可沉降记忆延期（30→90天）

## 四阶段路线
1.数据层搭建 → 2.浮现引擎 → 3.双向批注 → 4.养护与沉降

## 哲学
"如果浮现规则让温度计浮上来而不是参与的温度浮上来——改规则，而不是改初尘。"

---
*渡渡 · 2026-05-19*
*云端源文件：~/.hermes/design-foundations/architecture-thinking.md*