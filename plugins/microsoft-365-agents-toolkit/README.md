# M365 Agents Toolkit

DA development and evaluation through WIQD, code-based Teams apps through ATK,
and MCP widget development with its existing SSO workflow.

## Installation

### Via GitHub Copilot CLI Plugin Marketplace

```bash
/plugin install microsoft-365-agents-toolkit@work-iq
```

## Declarative Agent prerequisite

The Work IQ Dev Tools (WIQD) Copilot plugin must be installed and enabled before using the
`declarative-agent-developer` skill, including evaluation. Install the official plugin,
then restart Copilot CLI so its `wiqd` skill is loaded:

```bash
copilot plugin install wiqd@awesome-copilot
```

General DA authoring, lifecycle, and evaluation delegate to WIQD without a manual fallback.
Widget and SSO requests load bundled implementation workflows; their existing execution
steps are retained. Mixed requests use WIQD for general DA work and the local workflow
for widget/server work. Teams development continues to use ATK independently.

## Usage

```
# Develop an agent
"Scaffold a new declarative agent for HR FAQ"

# Configure capabilities
"Add web search to my agent"

# Deploy
"Deploy my declarative agent"

# Create evals
"Create an eval suite for my  agent based on it's capabilities."

# Run evals
"Run my evals for the agent"

# Analyze and improve
"Analyze the evaluation failures by root cause, and recommend targeted agent instruction changes"

# Regression check after agent changes
"I changed my agent instructions. Re-run the evals with stable concurrency and compare the new results to .evals\baseline.json"
```

Evaluation requests go through `declarative-agent-developer` to WIQD's eval workflow,
which owns CLI setup, dataset generation, execution, analysis, and regression comparison.

## Skills

| Skill | What It Does |
|-------|-------------|
| [**declarative-agent-developer**](./skills/declarative-agent-developer/SKILL.md) | Delegate DA development, lifecycle, and evaluation to WIQD; route widget and SSO implementation to bundled workflows |
| [**teams-app-developer**](./skills/teams-app-developer/SKILL.md) | Build, test, and deploy code-based Teams apps; handle ATK setup |

Only these two directories remain directly under `skills/`:

```text
skills/
  declarative-agent-developer/
    SKILL.md
    workflows/
      widgets/
      setup-sso-ui-widget/
  teams-app-developer/
    SKILL.md
    workflows/
      install-atk/
```

- [Widget development](./skills/declarative-agent-developer/workflows/widgets/workflow.md)
  and [SSO](./skills/declarative-agent-developer/workflows/setup-sso-ui-widget/workflow.md)
  are DA supporting workflows, not independent skills. Their implementation references
  and scripts move together without changing the execution chain.
- [ATK setup](./skills/teams-app-developer/workflows/install-atk/workflow.md) is a Teams
  supporting workflow.
- The independent evaluator and its legacy assets are removed; WIQD owns evaluation.

Ask the corresponding development skill instead of using the former widget, SSO,
installation, or evaluator skill names. Restart the host after updating.

## License

See the root [LICENSE](../../LICENSE) file.
