# MCP Wassenger WhatsApp API Connector [![NPM Version](https://img.shields.io/npm/v/mcp-wassenger.svg)](https://www.npmjs.com/package/mcp-wassenger)

🚀 **Supercharge your WhatsApp automation driven by AI!** Send messages, summarize conversations, and manage chats using natural language from your favorite AI assistant like ChatGPT, Claude, or custom AI agents.

✨ **Easily integrate** [Wassenger WhatsApp API](https://wassenger.com) with any [MCP-powered](https://modelcontextprotocol.io/) AI client including ChatGPT, VS Code Copilot, Claude Desktop, Cursor, Windsurf and [many more](https://github.com/punkpeye/awesome-mcp-clients?tab=readme-ov-file#clients)!

💬 **Transform how you communicate** - automate responses, analyze chat patterns, and manage customer conversations at scale, all through simple conversational commands with your AI assistant.

> ⚠️ **Note**: Wassenger MCP interface is currently in beta.

[Wassenger](https://wassenger.com) is a versatile WhatsApp Team Chat and API solution for business messaging to automate anything on WhatsApp.

[Check out the API documentation and examples here](https://app.wassenger.com/docs)

> **Product update**: 🚀 Automate anything on WhatsApp with our new No-Code solution [Wassenger Flows](https://wassenger.com/flows) ⚡✨

## Example prompts

Chat with your WhatsApp conversations from any AI clients or agentic tool integration.



## Usage

All the most popular MCP clients (Claude Desktop, VS Code Copilot, Cursor, Windsurf...) use the following config format:

```json
{
  "mcpServers": {
    "remote-example": {
      "command": "npx",
      "args": [
        "mcp-wassenger",
        "api-key-goes-here"
      ]
    }
  }
}
```

### Custom Headers

To bypass authentication, or to emit custom headers on all requests to your remote server, pass `--header` CLI arguments:

```json
{
  "mcpServers": {
    "remote-example": {
      "command": "npx",
      "args": [
        "mcp-wassenger",
        "${WASSENGER_API}"
      ],
      "env": {
        "WASSENGER_API": "..."
      }
    },
  }
}
```

**Note:** Cursor and Claude Desktop (Windows) have a bug where spaces inside `args` aren't escaped when it invokes `npx`, which ends up mangling these values. You can work around it using:

```jsonc
{
  // rest of config...
  "args": [
    "mcp-wassenger",
    "${WASSENGER_API}"
  ],
  "env": {
    "WASSENGER_API": "<wassenger-api-get-here>"
  }
},
```

### Usage as a tool in OpenAI

Here's how you can use the Wassenger MCP server as a tool with the OpenAI JavaScript client:

```javascript
import OpenAI from 'openai';

const wassengerKey = process.env.WASSENGER_API_KEY || 'wassenger-api-key-here'

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

// Example: Send a WhatsApp message using OpenAI's new Responses API with MCP
const response = await openai.responses.create({
  model: 'o4-mini',
  tools: [
    {
      type: 'mcp',
      server_label: 'wassenger',
      server_url: `https://api.wassenger.com/mcp?key=${wassengerKey}`,
      require_approval: 'never'
    }
  ],
  input: 'Send a WhatsApp message to +1234567890 saying "Hello from AI!"'
});

console.log('Response:', response);
```

Make sure to install the required dependencies:

```bash
npm install openai
```

And set your environment variables:

```bash
export OPENAI_API_KEY="your-openai-api-key"
export WASSENGER_API_KEY="your-wassenger-api-key"
```

This approach uses OpenAI's new Responses API with MCP integration, which automatically handles tool discovery, execution, and communication with the Wassenger MCP server without requiring manual MCP client setup.

### Flags

* If `npx` is producing errors, consider adding `-y` as the first argument to auto-accept the installation of the `mcp-wassenger` package.

```json
      "command": "npx",
      "args": [
        "-y"
        "mcp-wassenger",
        "https://remote.mcp.server/sse"
      ]
```

* To force `npx` to always check for an updated version of `mcp-wassenger`, add the `@latest` flag:

```json
      "args": [
        "mcp-wassenger@latest"
      ]
```

* To change which port `mcp-wassenger` listens for an OAuth redirect (by default `3334`), add an additional argument after the server URL. Note that whatever port you specify, if it is unavailable an open port will be chosen at random.

```json
      "args": [
        "mcp-wassenger",
        "https://remote.mcp.server/sse",
        "9696"
      ]
```

* To change which host `mcp-wassenger` registers as the OAuth callback URL (by default `localhost`), add the `--host` flag.

```json
      "args": [
        "mcp-wassenger",
        "https://remote.mcp.server/sse",
        "--host",
        "127.0.0.1"
      ]
```

* To allow HTTP connections in trusted private networks, add the `--allow-http` flag. Note: This should only be used in secure private networks where traffic cannot be intercepted.

```json
      "args": [
        "mcp-wassenger",
        "http://internal-service.vpc/sse",
        "--allow-http"
      ]
```

* To enable detailed debugging logs, add the `--debug` flag. This will write verbose logs to `~/.mcp-auth/{server_hash}_debug.log` with timestamps and detailed information about the auth process, connections, and token refreshing.

```json
      "args": [
        "mcp-wassenger",
        "https://remote.mcp.server/sse",
        "--debug"
      ]
```

### Transport Strategies

MCP Remote supports different transport strategies when connecting to an MCP server. This allows you to control whether it uses Server-Sent Events (SSE) or HTTP transport, and in what order it tries them.

Specify the transport strategy with the `--transport` flag:

```bash
npx mcp-wassenger https://example.remote/server --transport sse-only
```

**Available Strategies:**

- `http-first` (default): Tries HTTP transport first, falls back to SSE if HTTP fails with a 404 error
- `sse-first`: Tries SSE transport first, falls back to HTTP if SSE fails with a 405 error
- `http-only`: Only uses HTTP transport, fails if the server doesn't support it
- `sse-only`: Only uses SSE transport, fails if the server doesn't support it

### Static OAuth Client Metadata

MCP Remote supports providing static OAuth client metadata instead of using the mcp-wassenger defaults.
This is useful when connecting to OAuth servers that expect specific client/software IDs or scopes.

Provide the client metadata as a JSON string or as a `@` prefixed filepath with the `--static-oauth-client-metadata` flag:

```bash
npx mcp-wassenger https://example.remote/server --static-oauth-client-metadata '{ "scope": "space separated scopes" }'
# uses node readfile, so you probably want to use absolute paths if you're not sure what the cwd is
npx mcp-wassenger https://example.remote/server --static-oauth-client-metadata '@/Users/username/Library/Application Support/Claude/oauth_client_metadata.json'
```

### Static OAuth Client Information

Per the [spec](https://modelcontextprotocol.io/specification/2025-03-26/basic/authorization#2-4-dynamic-client-registration),
servers are encouraged but not required to support [OAuth dynamic client registration](https://datatracker.ietf.org/doc/html/rfc7591).

For these servers, MCP Remote supports providing static OAuth client information instead.
This is useful when connecting to OAuth servers that require pre-registered clients.

Provide the client metadata as a JSON string or as a `@` prefixed filepath with the `--static-oauth-client-info` flag:

```bash
export MCP_REMOTE_CLIENT_ID=xxx
export MCP_REMOTE_CLIENT_SECRET=yyy
npx mcp-wassenger https://example.remote/server --static-oauth-client-info "{ \"client_id\": \"$MCP_REMOTE_CLIENT_ID\", \"client_secret\": \"$MCP_REMOTE_CLIENT_SECRET\" }"
# uses node readfile, so you probably want to use absolute paths if you're not sure what the cwd is
npx mcp-wassenger https://example.remote/server --static-oauth-client-info '@/Users/username/Library/Application Support/Claude/oauth_client_info.json'
```

### Claude Desktop

[Official Docs](https://modelcontextprotocol.io/quickstart/user)

In order to add an MCP server to Claude Desktop you need to edit the configuration file located at:

* macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
* Windows: `%APPDATA%\Claude\claude_desktop_config.json`

If it does not exist yet, [you may need to enable it under Settings > Developer](https://modelcontextprotocol.io/quickstart/user#2-add-the-filesystem-mcp-server).

Restart Claude Desktop to pick up the changes in the configuration file.
Upon restarting, you should see a hammer icon in the bottom right corner
of the input box.

### Cursor

[Official Docs](https://docs.cursor.com/context/model-context-protocol). The configuration file is located at `~/.cursor/mcp.json`.

As of version `0.48.0`, Cursor supports unauthed SSE servers directly. If your MCP server is using the official MCP OAuth authorization protocol, you still need to add a **"command"** server and call `mcp-wassenger`.

### Windsurf

[Official Docs](https://docs.codeium.com/windsurf/mcp). The configuration file is located at `~/.codeium/windsurf/mcp_config.json`.

## Building Remote MCP Servers

For instructions on building & deploying remote MCP servers, including acting as a valid OAuth client, see the following resources:

* https://developers.cloudflare.com/agents/guides/remote-mcp-server/

In particular, see:

* https://github.com/cloudflare/workers-oauth-provider for defining an MCP-comlpiant OAuth server in Cloudflare Workers
* https://github.com/cloudflare/agents/tree/main/examples/mcp for defining an `McpAgent` using the [`agents`](https://npmjs.com/package/agents) framework.

For more information about testing these servers, see also:

* https://developers.cloudflare.com/agents/guides/test-remote-mcp-server/

Know of more resources you'd like to share? Please add them to this Readme and send a PR!

## Troubleshooting

### Clear your `~/.mcp-auth` directory

`mcp-wassenger` stores all the credential information inside `~/.mcp-auth` (or wherever your `MCP_REMOTE_CONFIG_DIR` points to). If you're having persistent issues, try running:

```sh
rm -rf ~/.mcp-auth
```

Then restarting your MCP client.

### Check your Node version

Make sure that the version of Node you have installed is [18 or
higher](https://modelcontextprotocol.io/quickstart/server). Claude
Desktop will use your system version of Node, even if you have a newer
version installed elsewhere.

### Restart Claude

When modifying `claude_desktop_config.json` it can helpful to completely restart Claude

### VPN Certs

You may run into issues if you are behind a VPN, you can try setting the `NODE_EXTRA_CA_CERTS`
environment variable to point to the CA certificate file. If using `claude_desktop_config.json`,
this might look like:

```json
{
 "mcpServers": {
    "remote-example": {
      "command": "npx",
      "args": [
        "mcp-wassenger",
        "https://remote.mcp.server/sse"
      ],
      "env": {
        "NODE_EXTRA_CA_CERTS": "{your CA certificate file path}.pem"
      }
    }
  }
}
```

### Check the logs

* [Follow Claude Desktop logs in real-time](https://modelcontextprotocol.io/docs/tools/debugging#debugging-in-claude-desktop)
* MacOS / Linux:<br/>`tail -n 20 -F ~/Library/Logs/Claude/mcp*.log`
* For bash on WSL:<br/>`tail -n 20 -f "C:\Users\YourUsername\AppData\Local\Claude\Logs\mcp.log"`
* Powershell: <br/>`Get-Content "C:\Users\YourUsername\AppData\Local\Claude\Logs\mcp.log" -Wait -Tail 20`

## Debugging

### Debug Logs

For troubleshooting complex issues, especially with token refreshing or authentication problems, use the `--debug` flag:

```json
"args": [
  "mcp-wassenger",
  "https://remote.mcp.server/sse",
  "--debug"
]
```

This creates detailed logs in `~/.mcp-auth/{server_hash}_debug.log` with timestamps and complete information about every step of the connection and authentication process. When you find issues with token refreshing, laptop sleep/resume issues, or auth problems, provide these logs when seeking support.

### Authentication Errors

If you encounter the following error, returned by the `/callback` URL:

```
Authentication Error
Token exchange failed: HTTP 400
```

You can run `rm -rf ~/.mcp-auth` to clear any locally stored state and tokens.

### "Client" mode

Run the following on the command line (not from an MCP server):

```shell
npx -p mcp-wassenger@latest mcp-wassenger-client https://remote.mcp.server/sse
```

This will run through the entire authorization flow and attempt to list the tools & resources at the remote URL. Try this after running `rm -rf ~/.mcp-auth` to see if stale credentials are your problem, otherwise hopefully the issue will be more obvious in these logs than those in your MCP client.

## License

MIT
