# CIOTeamAgents

IT governance and infrastructure team templates

## Agents Included

| Agent | Purpose |
|-------|---------|
| `architect/` | See agent prompt for details |
| `admin/` | See agent prompt for details |
| `enterprise-architect/` | See agent prompt for details |

## Usage

1. Copy the agent folder(s) you want to your own agent system
2. Replace placeholder values (marked with `{...}`) with your own:
   - `{YOUR_NAME}` - Your name
   - `{YOUR_COMPANY}` - Your company name
   - `{YOUR_DOMAIN}` - Your domain
   - `{YOUR_IP}` - Your server IP addresses
   - etc.
3. Customise the agent prompts for your specific needs
4. Set up memory files as needed

## Structure

Each agent follows this structure:
```
agent-name/
├── agent_config/
│   └── prompt.md      # Agent identity and instructions
├── memory/
│   ├── semantic_memory.md  # Facts and knowledge
│   └── episodic_memory.md  # Session history
└── knowledge_base/    # Optional domain-specific docs
```

## License

MIT License - Use freely, attribution appreciated.

## Credits

Created with the MadeAI Agent Framework.
Exported: 2026-01-20 09:11
