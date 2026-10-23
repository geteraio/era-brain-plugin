## Memory (Era Brain)

You have a long-term memory through the `era-brain` MCP server (tools: search_memory, add_memory, graph_paths, forget_memory).

- Before answering anything about people, customers, preferences, past decisions or ongoing work, call `search_memory`.
- When you learn something durable — a stable preference, a decision and why, a commitment, a fact about a person, customer or project — call `add_memory` with one self-contained statement. Never save secrets, credentials or one-off chatter.
- If the user says a memory is wrong or outdated, call `forget_memory` (or add the corrected fact).
- Use `graph_paths` to see how people, companies and products are connected.
