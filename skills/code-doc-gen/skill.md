---
name: tracing-spring-boot-api-flows
description: Use when analyzing a Spring Boot project's API entrypoints, request handling path, service orchestration, persistence layer, or downstream integrations, especially when the user asks for interface call flow, controller-to-service tracing, Feign or MQ dependencies, or stable endpoint documentation.
---

# Tracing Spring Boot API Flows

## Overview

Trace one Spring Boot endpoint at a time and produce two linked outputs:

1. A stable Markdown call-flow document
2. A Mermaid `sequenceDiagram` for the primary success path

This skill is evidence-first. Every claim must be marked `Confirmed`, `Inferred`, or `Unknown`.
Every non-table bullet or numbered claim in the Markdown output must carry one of those labels.
Use `Inferred` for conclusions supported by confirmed code evidence but not shown directly.

## When to Use

Use this skill when the user wants:

- A Spring Boot interface call flow
- Controller-to-service tracing
- Repository, Feign, MQ, or event dependency tracing
- Stable endpoint documentation
- A timing or sequence view for one endpoint

Do not use this for whole-system architecture mapping in a single pass.

## Workflow

1. Identify the endpoint entry from Spring MVC mapping annotations.
2. Trace controller request binding and the first delegation.
3. Follow the main service orchestration path.
4. Continue into repository, mapper, DAO, or JDBC code when present.
5. Check for Feign, `RestTemplate`, `WebClient`, MQ, event publication, and async boundaries.
6. Record transaction boundaries, major branches, and exception paths.
7. Write the Markdown output first.
8. Derive the Mermaid sequence diagram from the same main flow.

## Hard Constraints

1. Analyze one endpoint at a time unless the user explicitly asks for more.
2. Do not present guesses as facts.
3. Do not invent table names, SQL, consumers, or downstream behavior.
4. Do not invent controller, service, repository, handler, or participant names that are not evidenced.
5. If a name is not provided, keep the role generic instead of fabricating a symbol.
6. If evidence is missing, mark the item `Unknown`.
7. Keep the sequence diagram aligned with the documented main flow.
8. Do not impose an execution order in the sequence diagram when the relative order of success-path steps is not evidenced.

## Coverage Checklist

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

## Output Rules

1. Use `templates.md` as the source of truth for section order and output-shaping rules.
2. Keep the main flow concise and evidence-backed.
3. Put uncertainty in `Unknowns`, not in speculative prose.
4. Keep failure paths and branches in Markdown even when the diagram only shows the success path.
5. `Confirmed` claims must include file paths and line references when available.
6. Every non-table bullet and numbered item must end with exactly one `Confirmed`, `Inferred`, or `Unknown` label.
7. When component names are missing from evidence, use generic role labels such as `controller handler` or `service method` instead of invented method names.
8. If the relative order between success-path steps is unknown, keep that uncertainty in Markdown and omit the ambiguous ordering from the Mermaid diagram.

## Common Mistakes

- Stopping at the controller or service and missing repository or downstream calls
- Treating class names or method names as proof
- Forgetting transaction or async boundaries
- Drawing unsupported consumers in the sequence diagram
- Omitting evidence labels
- Letting the Markdown structure drift between runs

## Output Template

Use `templates.md` as the source of truth for:

1. The exact Markdown section order
2. The Mermaid `sequenceDiagram` structure
3. Behavioral output-shaping rules, including keeping in-process calls in the call chain and external systems in `External Dependencies`
4. Evidence-label examples
