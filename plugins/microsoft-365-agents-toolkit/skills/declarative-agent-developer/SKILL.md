---
name: declarative-agent-developer
description: >
  Develop Microsoft 365 Copilot declarative agents. Delegate Declarative Agent authoring, manifests,
  capabilities, API/MCP actions, authentication, localization, instruction review,
  validation, provisioning, publishing, and evaluation to the WIQD plugin's wiqd skill.
  Use bundled workflows for MCP server/widget implementation and existing widget-server
  Entra SSO (no OBO). Triggers: "create agent", "declarative agent", "add an MCP plugin",
  "deploy my agent", "review instructions", "evaluate my agent", "generate evals",
  "run my evals", "analyze eval failures", "MCP server for Copilot", "OpenAI Apps SDK",
  "Copilot widget", "structuredContent", "UI widget", "add SSO to my MCP server",
  "setup SSO for MCP Apps".
---

# Declarative Agent Developer

This is the Declarative Agent entry point. WIQD owns general Declarative Agent authoring, lifecycle, and evaluation.
Bundled widget and SSO workflows retain implementation capabilities not supplied by WIQD.

## Required Routing

Select the route before inspecting the workspace or running commands:

| Request | Route |
|---------|-------|
| Implement an MCP server or interactive widget for Copilot | Read [widget development](workflows/widgets/workflow.md) |
| Add Entra SSO to an existing widget MCP server | Read [widget SSO](workflows/setup-sso-ui-widget/workflow.md); do not restart widget creation |
| General Declarative Agent authoring, connecting an existing MCP endpoint, authentication configuration, lifecycle, or evaluation | Immediately delegate to WIQD as below |

For mixed requests, split work by these routes. Invoke WIQD directly for the Declarative Agent portion,
then continue the applicable local workflow. Do not re-enter this skill for a subtask:
references to `declarative-agent-developer` inside the bundled workflows mean its WIQD
delegation route, not restarting widget/SSO routing.

## WIQD Delegation

Invoke the WIQD plugin's `wiqd` skill with:

- the user's complete original request as context; for mixed requests, explicitly scope
  WIQD's task to the Declarative Agent portion and reserve widget/server implementation for the local workflow;
- relevant context already provided by the user; and
- any explicit constraints or desired outcomes.

For the WIQD route, do this before inspecting the workspace, answering implementation
questions, running commands, or editing files. Let `wiqd` own workflow selection, prerequisite
checks, project discovery, clarification, execution, and validation for its assigned scope.
For WIQD-only requests, it owns the final response. For mixed requests, resume the local
workflow afterward and combine both parts' verified results in the final response.

## Boundaries

- On the WIQD route, do not invoke CLIs directly, modify project files, or duplicate WIQD guidance.
- On widget/SSO routes, follow the bundled workflow's existing commands and scripts.
  This consolidation does not migrate their specialized integration steps to WIQD.
- Do not fall back to manual implementation when the `wiqd` skill is unavailable. Report that
  the WIQD plugin is a required dependency and stop.
- Keep code-based Teams apps, Custom Engine Agents, bots, tabs, and message extensions in
  `teams-app-developer`.
- Keep eval dataset generation, execution, result analysis, and regression comparison in
  WIQD's eval workflow. No separate evaluator is bundled.
