# CIO Team AI Agents

AI agent templates for IT governance and infrastructure management. These agents form a complete IT leadership team for managing technology strategy, architecture, and operations.

## Agents Included

| Agent | Role | Expertise |
|-------|------|-----------|
| `architect/` | System Architect (Archie) | Governance, standards enforcement, git management, system auditing |
| `admin/` | Infrastructure Builder (Bob) | Sysadmin, Docker, Linux, networking, home servers, cybersecurity |
| `enterprise-architect/` | Enterprise Architect | TOGAF, solution design, technology strategy, infrastructure planning |

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/aaroneisler/CIOTeamAgents.git
cd CIOTeamAgents
```

### 2. Choose Your Agent

**For governance and standards:** Use `architect/`
**For hands-on infrastructure:** Use `admin/`
**For strategic planning:** Use `enterprise-architect/`

### 3. Customise the Agent

Open `agent_config/prompt.md` and replace placeholders:

| Placeholder | Replace With |
|-------------|--------------|
| `{YOUR_NAME}` | Your name |
| `{YOUR_INFRASTRUCTURE}` | Your infrastructure name |
| `{YOUR_IP}` | Your server IP addresses |
| `{YOUR_NAS}` | Your NAS system (e.g., "Synology DS920+") |
| `{YOUR_ROUTER}` | Your router (e.g., "UniFi UDM Pro") |

### 4. Use with Your AI Platform

Copy the contents of `prompt.md` into your preferred AI platform:

- **Claude Projects:** Add as project instructions
- **Claude Code:** Place in your working directory
- **ChatGPT:** Use as Custom Instructions
- **Other LLMs:** Use as system prompt

## Agent Details

### System Architect (Archie)

**Purpose:** Governance and standards enforcement

- Enforces folder structure and naming conventions
- Manages git workflows and version control
- Validates changes against system rules
- Audits for compliance issues

**Best for:** Teams needing consistent standards across projects

### Infrastructure Builder (Bob)

**Purpose:** Hands-on system administration

- Docker container management
- Linux server administration
- Network configuration
- Backup systems
- Media servers (Plex, etc.)
- Document management (Paperless-ngx)

**Best for:** Home lab enthusiasts, small business IT

### Enterprise Architect

**Purpose:** Strategic technology planning

- TOGAF-based architecture
- Solution design
- Technology roadmaps
- Migration planning
- ADR (Architecture Decision Records)

**Includes:** TOGAF reference materials and ADR templates

## Example Usage

### Using Bob for Infrastructure

```
You: "My Plex server keeps buffering. Can you help troubleshoot?"

Bob: "Let's diagnose this systematically:
     1. First, check container resource usage: docker stats plex
     2. Verify transcoding settings...
     What's your current hardware specs?"
```

### Using Archie for Governance

```
You: "I want to add a new Python script to the project"

Archie: "According to our standards, Python scripts should be placed in
        scripts/tools/. What's the purpose of this script? I'll help you
        determine the correct location."
```

### Using Enterprise Architect for Planning

```
You: "We need to migrate from on-prem to cloud"

EA: "I'll help design this migration. Let me first understand:
    1. Current infrastructure inventory
    2. Business drivers for the move
    3. Compliance requirements
    Then I'll propose a phased migration plan using TOGAF ADM."
```

## Team Workflow

These agents work together:

```
Enterprise Architect (designs) → Bob (builds) → Archie (governs)
```

- **EA** designs the architecture and creates ADRs
- **Bob** implements the infrastructure
- **Archie** ensures standards compliance

## Included Knowledge Base

The `enterprise-architect/knowledge_base/` contains:

- **TOGAF Reference:** Enterprise architecture framework guides
- **ADRs:** Architecture Decision Record templates
- **Design Documents:** Example architecture designs

## Customisation Tips

### Adding Your Infrastructure

Update Bob's prompt with your actual systems:

```markdown
## Infrastructure You Manage

| System | Purpose | Key Info |
|--------|---------|----------|
| [Your NAS] | Storage, Backups | [IP address] |
| [Your Router] | Network | [Model] |
| [Your Servers] | Compute | [Specs] |
```

### Creating Governance Rules

Archie can enforce custom rules. Add to the `<instructions>` section:

```markdown
## Custom Rules
- All code must have tests
- PRs require two approvals
- No commits to main without review
```

## Requirements

- Any LLM that supports system prompts (Claude, GPT-4, Gemini, etc.)
- No code installation required
- No API keys needed (beyond your AI platform access)

## File Structure

```
CIOTeamAgents/
├── README.md
├── LICENSE
├── architect/
│   ├── agent_config/prompt.md
│   └── memory/
├── admin/
│   ├── agent_config/prompt.md
│   └── memory/
└── enterprise-architect/
    ├── agent_config/prompt.md
    ├── memory/
    └── knowledge_base/
        ├── TOGAF_Reference/
        └── adrs/
```

## License

Free Use, No Resale - See [LICENSE](LICENSE) file.

You can use these agents freely in personal or commercial settings. You cannot sell the templates themselves.

## Contributing

Found an improvement? Contributions welcome:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## Related Repositories

- [C-SuiteAgents](https://github.com/aaroneisler/C-SuiteAgents) - Business executive team
- [PersonalAssistantAgents](https://github.com/aaroneisler/PersonalAssistantAgents) - Personal productivity

## Credits

Created with the MadeAI Agent Framework.
