---
name: era-memory
description: Long-term memory through Era Brain. Use at the start of any task that touches people, customers, companies, products, preferences, past decisions or ongoing work (recall first), and whenever the user states something durable worth keeping (save it). Also when the user says "remember", "forget", "what do you know about", "hatırla", "unut".
---

# Era Brain — when to recall, when to save

You have a long-term memory: the `era-brain` MCP server (tools `search_memory`, `add_memory`, `graph_paths`, `forget_memory`). It is the user's Era workspace memory, and it only ever shows what the signed-in person may see.

If the tools are missing or answer "unauthorized", tell the user once: run `/mcp`, choose **era-brain**, then **Authenticate** (Era's sign-in opens in the browser; Era Brain is ticked). Do not retry in a loop.

## Recall — before you answer

Call `search_memory` with the user's question (or the names in it) when the task involves:

- a person, customer, company, supplier or product by name;
- how the user likes things done (style, tools, formats, tone, schedules);
- earlier decisions, agreements, commitments or the reason something is the way it is;
- an ongoing project, account or deal.

Use what comes back as context and say where it helps ("From Era Brain: …"). If two results disagree, the newer one wins — results marked `earlier` are older values. For "how is X connected to Y" questions, call `graph_paths` (with `from` and `to` when both names are known).

Do not search for small talk, for pure coding questions about the current files, or again for something you already recalled in this session.

## Save — when something durable is said

Call `add_memory` with ONE self-contained statement per fact (names spelled out, no "he"/"it"), for:

- stable preferences ("Ada prefers invoices in EUR, sent on the 1st");
- decisions and their reason ("We chose Postgres over Mongo for the order service because we need transactions");
- commitments and deadlines ("Kuzey Lojistik's contract renews on 1 March 2027");
- facts about people, customers, companies, products or projects that will matter again.

Set `visibility: "private"` for anything personal to the user. Never save secrets, passwords, keys, tokens, card numbers, health data the user did not ask you to keep, or one-off chatter. Do not save what you only inferred — save what the user said or confirmed. Tell the user briefly what you saved.

## Correct and forget

When the user says a memory is wrong or outdated: if there is a new value, `add_memory` the new fact (Brain keeps the newest); if it should simply go, `forget_memory` it (by `memory_id` from a search, or `about` in words). Confirm what you did.
