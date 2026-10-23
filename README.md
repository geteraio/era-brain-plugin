# Era Brain for Claude Code and Codex

[Docs](https://docs.getera.io/en/brain) · [Claude Code guide](https://docs.getera.io/en/brain/claude-code) · [Codex guide](https://docs.getera.io/en/brain/codex) · MIT

Long-term memory for your coding agent. With Era Brain, Claude Code **recalls** what matters before it answers — how you like things done, decisions and their reasons, the people, customers and projects you work with — and **saves** what it learns, in your [Era](https://getera.io) workspace.

- **One sign-in, no keys.** The plugin connects to Era's MCP server over OAuth: your browser opens Era's consent card, you pick the workspace, *Era Brain · memory* is ticked, and you can make it read-only.
- **Your permissions, always.** The agent sees exactly what you can see in Era — your workspace, your private memories, modules you can read — re-checked on every call. Disconnect any time in **Settings → API keys → Connected apps**.
- **A memory skill that knows when.** It searches before answering questions about people, preferences and past decisions, saves one self-contained fact at a time, never saves secrets, and corrects or forgets when you say something changed.

## Claude Code

```bash
claude plugin marketplace add geteraio/era-brain-plugin
claude plugin install era-brain@era
```

Restart Claude Code, then run `/mcp`, choose **plugin:era-brain:era-brain** and **Authenticate**. Approve Era's consent card in the browser. That's it — try:

```text
Remember that our staging database is only reset on Sundays.
```

```text
When is staging reset?
```

The plugin also adds two commands: `/era-brain:remember <fact>` and `/era-brain:recall <question>`.

Prefer no plugin? The same server, without the skill:

```bash
claude mcp add --transport http --scope user era-brain "https://app.getera.io/api/mcp?tools=brain"
```

## Codex

```bash
codex mcp add era-brain --url "https://app.getera.io/api/mcp?tools=brain"
codex mcp login era-brain
```

Then add [`codex/AGENTS.md`](codex/AGENTS.md) to `~/.codex/AGENTS.md` (or your project's `AGENTS.md`) so Codex knows when to recall and when to save. The config-file version is in [`codex/config.toml`](codex/config.toml). With the Era CLI, `era brain setup codex --apply` does all three.

## Cursor and other MCP clients

Add a remote (streamable HTTP) MCP server named `era-brain` with the URL `https://app.getera.io/api/mcp?tools=brain`. The client opens Era's sign-in the first time it connects.

## Let your agent set itself up

Paste [`SETUP-FOR-AGENT.md`](SETUP-FOR-AGENT.md) into any agent — it installs Era Brain into itself and asks you only to approve the sign-in.

## Tools

| Tool | What it does | Needs |
| --- | --- | --- |
| `search_memory` | Hybrid recall (keyword + semantic, newest value first); `graph: true` adds relationship paths | `brain:read` |
| `add_memory` | Saves text or ready-made facts; `visibility: "private"` keeps it to you | `brain:write` |
| `graph_paths` | Typed paths between the people, companies and products a question names (or `from` → `to`) | `brain:read` |
| `forget_memory` | Stops Era using a memory, by id or described in words | `brain:write` |

A read-only connection gets only `search_memory` and `graph_paths`.

## Your own Era

If your company runs its own Era, set the address once:

```bash
echo '{"era_url":"https://era.example.com"}' | claude plugin configure era-brain@era --values-stdin
```

## Troubleshooting

| What you see | What to do |
| --- | --- |
| `era-brain` shows **Needs authentication** | `/mcp` → plugin:era-brain:era-brain → Authenticate. |
| The tools answer *not available to this connection* | The connection was approved without Era Brain, or read-only. Disconnect it in Era (Settings → API keys → Connected apps) and authenticate again. |
| An added memory says **waiting** | Free text is split into facts by your workspace's AI, and private notes are filed with your own AI. Connect your AI subscription in Era (Settings → AI). Brain also asks that AI when a new fact may replace an older one; until one is connected, the memory waits. |

Docs: [docs.getera.io/en/brain](https://docs.getera.io/en/brain) · SDK: [`@getera/brain`](https://www.npmjs.com/package/@getera/brain) · CLI: [`@getera/cli`](https://www.npmjs.com/package/@getera/cli)

## What is in this repository

```text
.claude-plugin/marketplace.json         the "era" marketplace (claude plugin marketplace add geteraio/era-brain-plugin)
plugins/era-brain/
  .claude-plugin/plugin.json            the plugin: name, version, the era_url option
  .mcp.json                             the era-brain MCP server (HTTP, OAuth)
  skills/era-memory/SKILL.md            when to recall, when to save, what never to save
  commands/remember.md, recall.md       /era-brain:remember and /era-brain:recall
codex/AGENTS.md, codex/config.toml      the same for Codex
SETUP-FOR-AGENT.md                      paste into any agent to let it install itself
```

You need an Era account ([sign up](https://app.getera.io/signup?start=brain)); the plugin holds no keys.

## License

[MIT](LICENSE) © Era
