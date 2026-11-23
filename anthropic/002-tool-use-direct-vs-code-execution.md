# Direct vs Code Execution

[Official Reference - Programmatic Tool Calling](https://platform.claude.com/docs/en/agents-and-tools/tool-use/programmatic-tool-calling)

**Direct tool calling** is the standard loop. The model emits a `tool_use` block, your client runs the tool, and the result goes back to the model as a `tool_result`. Each call is a model round trip, and every result enters the model's context. The model sees all of the data, so a query returning 10,000 rows consumes that many tokens.

**Code execution (`code_execution_20260120`)** is a server-side sandbox. The model writes Python that runs in the sandbox, and that code can call your custom tools as functions. The `"allowed_callers": ["code_execution_20260120"]` field on `query_database` in your notes is what enables this: the tool can be invoked from inside the code rather than only directly by the model.

In your example, the model can write a script that calls `query_database` once per region, aggregates the results in Python, and returns only the answer (for example, which region has the highest revenue). The raw rows never reach the model's context. The benefits are:

- **Fewer tokens**: intermediate data stays out of the context window.
- **Fewer round trips**: loops and aggregation run in one execution instead of one model turn per call.
- **Less model reasoning over raw data**: arithmetic and filtering are done by code, not by the model.

The tradeoff is that the model's control flow is now code, so you lose per-call visibility and must trust the sandbox's execution of your tool calls.

```python
client = anthropic.Anthropic()

response = client.messages.create(
    model = "claude-opus-5.5",
    max_tokens = 4096,
    messages = [
        {
            "role": "user",
            "content": "Query sales data for the West, East... which has the highest revenue"
        }
    ],
    tools = [
        {
            "type": "code_execution_20260120", "name": "code_execution"
        },
        {
            "name": "query_database",
            "description": "Execute a SQL Query against the sales database. Return a list of rows as JSON objects.",
            "input_schema": {
                "type": "object",
                "properties": {
                    "sql": {"type": "string", "description": "SQL query to execute" }
                },
                "required": ["sql"]
            },
            "allowed_callers": ["code_execution_20260120"]
        }
    ]
)

print(response)
```

## How it works

You have the first part right. The second part is only half right: **you still run `query_database` yourself in both cases.** What changes is who calls it and where the result goes.

### `"direct"` (the default)
1. The model returns a `tool_use` block with `name: "query_database"` and `input: {"sql": "..."}`.
2. Your code runs the query.
3. You send the rows back in a `tool_result` block (not `tool_response`) in the next `user` message.
4. The model reads the rows. **They're now in its context.**

For 4 regions, that's 4 tool calls (in one turn if they're parallel, or several turns if not), and every row is read by the model.

### `"code_execution_20260120"`
1. The model writes Python, and that Python runs in **Anthropic's sandbox**, not on your machine. For example:
   ```python
   totals = {}
   for r in ["West", "East", "North", "South"]:
       rows = query_database(sql=f"SELECT SUM(revenue) ... WHERE region='{r}'")
       totals[r] = rows[0]["sum"]
   print(max(totals, key=totals.get))
   ```
2. When the script reaches `query_database(...)`, the sandbox pauses. The API returns a `tool_use` block to **you**, marked as called from code execution.
3. **Your code still runs the query** and sends back a `tool_result`, the same way as before.
4. The result goes **to the running Python script, not to the model**. The script resumes and continues its loop, making further calls the same way.
5. When the script finishes, the model sees only what it printed (for example, `"West"`).

### Summary

| | Who decides to call the tool | Who executes the tool | Who receives the result |
|---|---|---|---|
| `direct` | Model | You | Model (in its context) |
| `code_execution` | Model-written code in the sandbox | You | The code; the model sees only the final output |

The only thing that runs automatically on Anthropic's side is the Python itself (loops, maths, filtering). Your custom tools always run in your code. Code execution saves tokens and model turns, because the model doesn't have to read and reason over every intermediate result.

You can list both callers, `"allowed_callers": ["direct", "code_execution_20260120"]`, so the model can choose either. With only `code_execution_20260120`, as in your notes, the model can call the tool only from code.