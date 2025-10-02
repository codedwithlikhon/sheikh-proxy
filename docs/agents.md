# Sheikh Agent & Tool Integration Spec

Sheikh Proxy can evolve into a hybrid LLM + agent system, enabling tool execution as part of responses.

## Available Tools

1. **BashTool**  
   Execute shell commands on server environment.  
   E.g. `ls`, `grep`, `pwd`.

2. **CodeExecTool**  
   Run code snippets (Python, JS, etc.) in sandboxed environment.  
   Return output or error.

3. **EditorTool**  
   Read / write files in your project repository.  
   Useful for code generation, updates.

4. **WebFetchTool**  
   HTTP GET / POST API calls, fetch content / JSON / HTML.

5. **WebSearchTool**  
   Perform search queries (e.g. via Google, Bing) and return search results.

6. **MemoryTool**  
   Persist facts, past interactions, user preferences, context.  
   Retrieve memory in future prompts.

## Agent Workflow

When a user prompt is given:

1. Agent analyzes whether tools are needed (e.g. “Fetch this URL”, “Generate Dockerfile”).
2. In an internal `<think>` block, picks the tool(s) to use.
3. Calls tool(s) with appropriate arguments.
4. Receives tool outputs.
5. Wraps the output into a prompt or combines with user request, then sends to LLM.
6. Optionally stores new facts in memory.
7. Returns final answer to user.

## Prompt Strategy for Agent

- Always include a system / instruction portion describing your role and tool policy.
- Use XML-like `<tool>` tags inside prompt to indicate tool calls.
- Use chain-of-thought reasoning *internally*, but only output a short reasoning summary / final answer.
- Use delimiter structure (###, triple quotes) to separate context, instructions, tool usage, input, output spec.

## Example Prompt Flow

```

<system>
You are SheikhAgent, an AI with tools. Use them when needed. Be safe, precise.
</system>

<user>
Generate a Dockerfile for Sheikh Proxy, then verify it with `docker build`.
</user>
```

Internally, agent might do:

````xml
<think>
Need to generate Dockerfile → use EditorTool to create file
Then use BashTool to run `docker build .`
</think>

<tool name="EditorTool">
path: Dockerfile
content: "FROM python:3.11-slim …"
</tool>

<tool name="BashTool">
command: "docker build -t sheikh-proxy ."
</tool>

<answer>
Dockerfile created. Build succeeded with output: “…”  
Here’s the Dockerfile:
```dockerfile
…
```

</answer>
````

## Success Criteria

* Tools work correctly (writes files, runs commands, fetches web)
* Agent does not leak internal reasoning or secrets
* Outputs are valid, safe, and match schema / user request
* Memory is consistent and recallable across sessions

---
