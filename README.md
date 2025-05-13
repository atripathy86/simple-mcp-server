# simple-mcp-server

This project is a simple example of an MCP server

## Build Server
-`docker compose up --build`
- This will expose MCP server at http://localhost:8384/sse

## Client 
- UV: 
    ```
    curl -LsSf https://astral.sh/uv/install.sh | sh.
    uv lock
    uv venv simple-mcp
    source simple-mcp/bin/activate 
    uv pip install -e . 
    ```
- PIP: 
    ```
    pip install pip-tools
    pip-compile pyproject.toml 
    pip install -r requirements.txt
    ```
- python mcp-client.py 

## Connecting to MCP Server from a MCP Client 
- VSCode: If using workspace mode. Create .vscode/mcp.json as below 
- More info here: https://code.visualstudio.com/docs/copilot/chat/mcp-servers
- Switch to Agent Mode in Github Copilot Chat and click refresh so that the available tools are queried from MCP server and updated
- Currently of the three primitives (tools, prompts, resources), servers can only provide tools to Copilot's agent mode.

```
// Example .vscode/mcp.json
{
    "servers": {
      "simple-mcp-server": {
        "type": "sse",
        "url": "http://localhost:8384/sse",
        "headers": { "VERSION": "1.2" }
      }
    }
}
```

## Operations
- simple-mcp-server
[simple-mcp-server](img/simple-mcp-server.jpg)
- python mcp-client.py
    ```
    $ python mcp-client.py
    meta=None nextCursor=None tools=[Tool(name='add', description='Add two numbers', inputSchema={'properties': {'a': {'title': 'A', 'type': 'integer'}, 'b': {'title': 'B', 'type': 'integer'}}, 'required': ['a', 'b'], 'title': 'addArguments', 'type': 'object'}, annotations=None)]
    resources meta=None nextCursor=None resources=[Resource(uri=AnyUrl('resource://some_static_resource'), name='resource://some_static_resource', description=None, mimeType='text/plain', size=None, annotations=None)]
    content Any static data can be returned
    content Hello, yash!
    prompts meta=None nextCursor=None prompts=[Prompt(name='review_code', description='', arguments=[PromptArgument(name='code', description=None, required=True)]), Prompt(name='debug_error', description='', arguments=[PromptArgument(name='error', description=None, required=True)])]
    prompt meta=None description=None messages=[PromptMessage(role='user', content=TextContent(type='text', text='Please review this code:\n\nprint("Hello world")', annotations=None))]
    prompt meta=None description=None messages=[PromptMessage(role='user', content=TextContent(type='text', text='[\n  "user",\n  "I\'m seeing this error:"\n]', annotations=None)), PromptMessage(role='user', content=TextContent(type='text', text='[\n  "user",\n  "SyntaxError: invalid syntax"\n]', annotations=None)), PromptMessage(role='user', content=TextContent(type='text', text='[\n  "assistant",\n  "I\'ll help debug that. What have you tried so far?"\n]', annotations=None))]
    ```
