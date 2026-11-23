# Pushary for OpenHands

OpenHands stops when it needs you. You're not at your desk, so it sits there.

Pushary sends that moment to your phone. Before OpenHands runs a terminal command or edits a file, your Pushary rules decide. The risky ones wait for your yes or no. Tap yes and it keeps going.

- **Your phone.** The step shows up as a push. Approve or deny in one tap.
- **A call if you miss it.** No answer after a minute? Pushary calls you and reads the step out. Say yes, no, or tell the agent what to do instead.
- **Your team.** On Team, a rule can send risky steps to your lead instead of you.
- **Questions and updates.** The agent can ask you a question and send a push when a long task ends.

Nobody answers? The step does not run. A missing answer is never a yes.

| Plan | Price | Call minutes a month |
| --- | --- | --- |
| Agent | $9.99 a month | 10 |
| Agent Pro | $19.99 a month | 20 |
| Team | $59 a month for 5 people | 60 |

Every plan starts with 3 days free at [pushary.com](https://pushary.com). You also need the Pushary app on your phone, or the Mac app, signed in to the same account.

## Install

1. Create a key under [Settings, API keys](https://pushary.com/dashboard/agent/settings).
2. Start Agent Canvas with the key in its environment:

   ```bash
   PUSHARY_API_KEY=pk_... agent-canvas
   ```

3. Open **Plugins** and install from source `github:Pushary/openhands-plugin`. Or clone it where Canvas looks for local plugins:

   ```bash
   git clone https://github.com/Pushary/openhands-plugin ~/.openhands/plugins/pushary
   ```

4. Start a new conversation and ask OpenHands to run `touch pushary-check.txt`. Your phone gets the question. Tap **Deny** and the file is not created.

Running Agent Canvas on your own computer? `npx -y pushary@latest setup --agents openhands` does the approvals part without this plugin, and saves the key for you. Use one or the other, not both, or you get asked twice.

## What's inside

| File | What it does |
| --- | --- |
| `hooks/hooks.json` | Runs the Pushary hook before `terminal`, `file_editor`, `planning_file_editor`, `apply_patch`, `write_file` and `edit`, and reports finished steps in the background. Generated from the Pushary CLI, never edited by hand. |
| `.mcp.json` | Connects the Pushary MCP server at `https://pushary.com/api/mcp/mcp` with `Bearer ${PUSHARY_API_KEY}`. |
| `skills/pushary/SKILL.md` | Tells the agent when to reach you. |

The hooks run `npx -y -p @pushary/agent-hooks@latest`, so the backend needs Node.js 20.17+, 22.13+ or 23.5+.

## How it behaves

- OpenHands hooks answer allow or deny, never "ask". When nobody answers in time, Pushary denies the step and tells the agent why.
- Pushary runs before OpenHands' own confirmation step. If Pushary has no opinion, for example when it's offline, OpenHands carries on as if there were no hook.
- In OpenHands Cloud, the MCP server works with your key saved as a secret. Approvals before each command in Cloud aren't tested yet.
- OpenHands' experimental Decision cards add their own `ask_user` tool. With Pushary as your only MCP server, OpenHands won't start a conversation (`Duplicate tool names found: {'ask_user'}`). Turn Decision cards off, or add a second MCP server, which makes OpenHands prefix the names.

Full guide: [pushary.com/docs/agents/guides/openhands](https://pushary.com/docs/agents/guides/openhands)

## License

MIT
