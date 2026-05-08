---
name: code-doc-gen
description: Generate code-backed documentation for Spring Boot APIs from existing implementations. Use when the user asks in Chinese or English for endpoint documentation, interface call flow, API tracing, controller-to-service tracing, downstream dependency analysis, or Mermaid sequence diagrams. 基于现有 Spring Boot 实现生成有代码证据支撑的接口文档。适用于用户用中文或英文提出接口文档生成、接口调用链分析、API 调用流程梳理、Controller 到 Service 调用追踪、下游依赖分析或 Mermaid 时序图生成等需求。
---

# Code Documentation Generation for Spring Boot APIs

## Overview / 概述

Generate stable, code-backed documentation for one Spring Boot endpoint at a time. The documentation should explain how the endpoint works from entrypoint to downstream calls, and produce two linked outputs. 一次只针对一个 Spring Boot 接口生成稳定、基于代码证据的文档，说明该接口从入口到下游调用的执行方式，并产出两份相互对应的结果：

1. A stable Markdown call-flow document
2. A Mermaid `sequenceDiagram` for the primary success path

This skill is designed for code documentation generation, not freeform summarization. It is evidence-first. Every claim must be marked `Confirmed`, `Inferred`, or `Unknown`.
该 skill 面向基于代码的文档生成，而不是自由发挥式摘要。它以证据为先。每一条结论都必须标记为 `Confirmed`、`Inferred` 或 `Unknown`。
Every non-table bullet or numbered claim in the Markdown output must carry one of those labels.
Markdown 输出中，所有非表格的项目符号和编号结论都必须带上其中一个标签。
Use `Inferred` for conclusions supported by confirmed code evidence but not shown directly.
如果结论由已确认代码证据支持、但并未直接写明，应标记为 `Inferred`。

## When to Use / 何时使用

Use this skill when the user wants any of the following, in English or Chinese:
当用户用英文或中文表达以下意图时，使用这个 skill：

- Code documentation generated from backend code
- API documentation generated from existing Spring Boot implementations
- Developer-facing documentation for one endpoint's processing flow
- A Spring Boot interface call flow
- Controller-to-service tracing
- Repository, Feign, MQ, or event dependency tracing
- Stable endpoint documentation
- A timing or sequence view for one endpoint
- 根据后端代码生成文档
- 根据现有 Spring Boot 实现生成接口文档
- 生成面向开发者的单接口处理流程说明
- Spring Boot 接口调用链分析
- Controller 到 Service 的调用追踪
- Repository、Feign、MQ 或事件依赖梳理
- 稳定的单接口文档输出
- 单个接口的时序图或执行流程视图

Example Chinese triggers / 中文触发示例：

- 分析这个 Spring Boot 接口的调用链
- 帮我梳理这个接口从 Controller 到 Service 再到 Repository 的流程
- 看一下这个 API 有没有调用 Feign 或发 MQ
- 生成这个接口的调用时序图和文档
- 根据这段 Spring Boot 代码生成接口说明文档
- 帮我把这个后端接口整理成给开发看的技术文档

Do not use this for whole-system architecture mapping in a single pass.
不要把它用于一次性梳理整个系统的完整架构。

## Workflow / 工作流

1. Identify the endpoint entry from Spring MVC mapping annotations.
2. Trace controller request binding and the first delegation.
3. Follow the main service orchestration path.
4. Continue into repository, mapper, DAO, or JDBC code when present.
5. Check for Feign, `RestTemplate`, `WebClient`, MQ, event publication, and async boundaries.
6. Record transaction boundaries, major branches, and exception paths.
7. Write the Markdown output first.
8. Derive the Mermaid sequence diagram from the same main flow.

工作步骤对应如下：

1. 从 Spring MVC 映射注解识别接口入口。
2. 追踪 controller 的请求绑定和首次委派。
3. 顺着主 service 编排路径继续分析。
4. 如存在 repository、mapper、DAO 或 JDBC，则继续深入。
5. 检查 Feign、`RestTemplate`、`WebClient`、MQ、事件发布和异步边界。
6. 记录事务边界、主要分支和异常路径。
7. 先写 Markdown 输出。
8. 再基于同一主路径生成 Mermaid 时序图。

## Hard Constraints / 硬性约束

1. Analyze one endpoint at a time unless the user explicitly asks for more.
2. Do not present guesses as facts.
3. Do not invent table names, SQL, consumers, or downstream behavior.
4. Do not invent controller, service, repository, handler, or participant names that are not evidenced.
5. If a name is not provided, keep the role generic instead of fabricating a symbol.
6. If evidence is missing, mark the item `Unknown`.
7. Keep the sequence diagram aligned with the documented main flow.
8. Do not impose an execution order in the sequence diagram when the relative order of success-path steps is not evidenced.

中文说明：

1. 除非用户明确要求，否则一次只分析一个接口。
2. 不要把猜测写成事实。
3. 不要虚构表名、SQL、消费者或下游行为。
4. 不要编造 controller、service、repository、handler 或参与者名称。
5. 如果证据没有提供具体名称，就使用通用角色名称。
6. 证据缺失时标记为 `Unknown`。
7. 时序图必须和文档中的主流程保持一致。
8. 如果成功路径中各步骤的先后关系没有证据支撑，就不要在时序图中强行排序。

## Coverage Checklist / 覆盖检查清单

Always check for:

- Controller entrypoint
- Request DTO or parameter binding
- Service orchestration
- Repository or mapper persistence path
- External HTTP or RPC clients
- MQ or event publication
- `@Transactional`
- `@Async`
- Exception or validation branches
- Missing consumers or missing persistence details

中文检查项：

- Controller 入口
- 请求 DTO 或参数绑定
- Service 编排流程
- Repository 或 Mapper 持久化路径
- 外部 HTTP 或 RPC 客户端
- MQ 或事件发布
- `@Transactional`
- `@Async`
- 异常或校验分支
- 缺失的消费者或持久化细节

## Output Rules / 输出规则

1. Use `templates.md` as the source of truth for section order and output-shaping rules.
2. Keep the main flow concise and evidence-backed.
3. Put uncertainty in `Unknowns`, not in speculative prose.
4. Keep failure paths and branches in Markdown even when the diagram only shows the success path.
5. `Confirmed` claims must include file paths and line references when available.
6. Every non-table bullet and numbered item must end with exactly one `Confirmed`, `Inferred`, or `Unknown` label.
7. When component names are missing from evidence, use generic role labels such as `controller handler` or `service method` instead of invented method names.
8. If the relative order between success-path steps is unknown, keep that uncertainty in Markdown and omit the ambiguous ordering from the Mermaid diagram.
9. Prefer documentation wording that helps another engineer understand and maintain the endpoint, while staying strictly evidence-backed.

中文规则：

1. 以 `templates.md` 作为章节顺序和输出格式的唯一准则。
2. 主流程要简洁，并且有证据支撑。
3. 不确定项放进 `Unknowns`，不要写成猜测性 prose。
4. 即使时序图只展示成功路径，Markdown 里也要保留失败路径和分支。
5. `Confirmed` 结论在可能时必须附带文件路径和行号。
6. 所有非表格的项目符号和编号项必须以且仅以一个 `Confirmed`、`Inferred` 或 `Unknown` 结尾。
7. 如果证据里没有组件名，使用通用角色标签，不要编造方法名。
8. 如果成功路径步骤之间的先后关系不明确，就在 Markdown 中保留这种不确定性，并在 Mermaid 中省略有歧义的顺序。
9. 优先使用便于其他工程师理解和维护接口的文档表达方式，但必须严格受代码证据约束。

## Common Mistakes / 常见错误

- Stopping at the controller or service and missing repository or downstream calls
- Treating class names or method names as proof
- Forgetting transaction or async boundaries
- Drawing unsupported consumers in the sequence diagram
- Omitting evidence labels
- Letting the Markdown structure drift between runs

中文常见错误：

- 只看到 controller 或 service 就停下，漏掉 repository 或下游调用
- 把类名或方法名当成事实证据
- 忘记事务边界或异步边界
- 在时序图里画出没有证据支持的消费者
- 漏掉证据标签
- 多次运行后 Markdown 结构发生漂移

## Output Template / 输出模板

Use `templates.md` as the source of truth for:
以 `templates.md` 作为以下内容的唯一准则：

1. The exact Markdown section order
2. The Mermaid `sequenceDiagram` structure
3. Behavioral output-shaping rules, including keeping in-process calls in the call chain and external systems in `External Dependencies`
4. Evidence-label examples

1. Markdown 的精确章节顺序
2. Mermaid `sequenceDiagram` 的结构
3. 输出整形规则，包括在调用链中保留进程内调用，并把外部系统放进 `External Dependencies`
4. 证据标签示例
