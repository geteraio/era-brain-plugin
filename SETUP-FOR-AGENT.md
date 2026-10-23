Set up Era Brain as your long-term memory. Era Brain is a hosted memory service (https://getera.io/brain): you search it before answering and save durable facts to it. It connects over MCP with an OAuth sign-in in the user's browser — there is no key to paste.

Do these steps, then tell me what you did:

1. Work out which assistant you are.
   - Claude Code: run
     `claude plugin marketplace add geteraio/era-brain-plugin`
     `claude plugin install era-brain@era`
     If plugins are not available, run `claude mcp add --transport http --scope user era-brain "https://app.getera.io/api/mcp?tools=brain"` instead.
   - Codex: run
     `codex mcp add era-brain --url "https://app.getera.io/api/mcp?tools=brain"`
     `codex mcp login era-brain`
     Then append the "Memory (Era Brain)" section below to `~/.codex/AGENTS.md` (create the file if needed; do not duplicate the section).
   - Cursor or another MCP client: add a remote (streamable HTTP) MCP server named `era-brain` with the URL `https://app.getera.io/api/mcp?tools=brain`.
   - If the user runs their own Era, replace `https://app.getera.io` with their Era address.

2. Sign-in. The first connection opens Era's consent card in the browser. Ask me to approve it: I choose the workspace, "Era Brain · memory" is ticked, and I can make it read-only. In Claude Code the sign-in starts from `/mcp` → era-brain → Authenticate (restart Claude Code first if you just installed the plugin). Do not try to complete the sign-in yourself.

3. Check it works: call `search_memory` with the query "test". An empty result is fine; an "unauthorized" error means the sign-in is not done yet.

4. From now on, follow these rules (for assistants without the plugin, keep them in your standing instructions):

## Memory (Era Brain)

You have a long-term memory through the `era-brain` MCP server (tools: search_memory, add_memory, graph_paths, forget_memory).

- Before answering anything about people, customers, preferences, past decisions or ongoing work, call `search_memory`.
- When you learn something durable — a stable preference, a decision and why, a commitment, a fact about a person, customer or project — call `add_memory` with one self-contained statement. Never save secrets, credentials or one-off chatter.
- If the user says a memory is wrong or outdated, call `forget_memory` (or add the corrected fact).
- Use `graph_paths` to see how people, companies and products are connected.
