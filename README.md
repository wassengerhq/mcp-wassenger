# MCP Wassenger WhatsApp API Connector [![NPM Version](https://img.shields.io/npm/v/mcp-wassenger.svg)](https://www.npmjs.com/package/mcp-wassenger)

🚀 **Supercharge your WhatsApp automation driven by AI!** Send messages, summarize conversations, and manage chats using natural language from your favorite AI assistant like ChatGPT, Claude, or custom AI agents.

✨ **Easily integrate** [Wassenger WhatsApp API](https://wassenger.com) with any [MCP-powered](https://modelcontextprotocol.io/) AI client including ChatGPT, VS Code Copilot, Claude Desktop, Cursor, Windsurf and [many more](https://github.com/punkpeye/awesome-mcp-clients?tab=readme-ov-file#clients)!

💬 **Transform how you communicate** - automate responses, analyze chat patterns, and manage customer conversations at scale, manage WhatsApp chats and groups, everything through simple conversational text or voice commands with your AI assistant.

> ⚠️ **Note**: You only need to use this package if your MCP client does not [support HTTP streaming](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#streamable-http) (previously known as SSE connection). Otherwise follow [these instructions](#http-streaming-usage).

## Contents

- [About](#about)
- [Example prompts](#example-prompts)
  - [📱 Basic Messaging](#-basic-messaging)
  - [📊 Conversation Analysis](#-conversation-analysis)
  - [👥 Group Management](#-group-management)
  - [⏰ Message Scheduling](#-message-scheduling)
  - [🔍 Contact Validation & Management](#-contact-validation--management)
  - [📈 Analytics & Insights](#-analytics--insights)
  - [🔔 Status & Monitoring](#-status--monitoring)
  - [🔄 Bulk Operations](#-bulk-operations)
  - [🔐 Account Management](#-account-management)
  - [🎯 Smart Automation](#-smart-automation)
- [MCP streaming usage](#mcp-streaming-usage)
  - [Supported Clients](#supported-clients)
  - [Claude Desktop Configuration](#claude-desktop-configuration)
  - [VS Code Copilot Configuration](#vs-code-copilot-configuration)
  - [Benefits of HTTP Streaming](#benefits-of-http-streaming)
  - [Getting Your API Key](#getting-your-api-key)
- [Usage](#usage)
  - [Custom Headers](#custom-headers)
  - [Usage as a tool in OpenAI](#usage-as-a-tool-in-openai)
  - [Flags](#flags)
  - [Claude Desktop](#claude-desktop)
  - [VS Code Extension / Copilot](#vs-code-extension--copilot)
  - [Cursor](#cursor)
  - [Windsurf](#windsurf)
  - [Cline](#cline)
  - [Continue.dev](#continuedev)
  - [Zed Editor](#zed-editor)
  - [Jan AI](#jan-ai)
  - [Open WebUI](#open-webui)
  - [Sourcegraph Cody](#sourcegraph-cody)
  - [OpenAI Responses API](#openai-responses-api)
- [Troubleshooting](#troubleshooting)
  - [Clear your `~/.mcp-auth` directory](#clear-your-mcp-auth-directory)
  - [Check your Node version](#check-your-node-version)
  - [Restart Claude](#restart-claude)
  - [VPN Certs](#vpn-certs)
  - [Check the logs](#check-the-logs)
- [Debugging](#debugging)
  - [Debug Logs](#debug-logs)
  - [Authentication Errors](#authentication-errors)
  - ["Client" mode](#client-mode)
- [License](#license)

## About

[Wassenger](https://wassenger.com) is a versatile WhatsApp Team Chat and API solution for business messaging to automate anything on WhatsApp.

[Check out the API documentation and examples here](https://app.wassenger.com/docs)

> **Product update**: 🚀 Automate anything on WhatsApp with our new No-Code solution [Wassenger Flows](https://wassenger.com/flows) ⚡✨

## Example prompts

Chat with your WhatsApp conversations from any AI clients or agentic tool integration.

Here are various prompts you can use with any AI assistant to interact with WhatsApp through the Wassenger MCP connector:

### 📱 Basic Messaging
- "Send a WhatsApp message to +1234567890 saying 'Hello! How are you today?'"
- "Send a message to the contact named 'John Smith' with the text 'Meeting confirmed for 3 PM'"
- "Send an urgent message to +44123456789: 'Please call me back ASAP'"

### 📊 Conversation Analysis
- "Summarize my last 10 WhatsApp messages with +1555123456"
- "Analyze the conversation tone in my chat with the Marketing Team group"
- "Show me the key topics discussed in my conversation with Sarah over the past week"
- "Count how many messages I've received today from all contacts"

### 👥 Group Management
- "How many participants are in the 'Project Team Alpha' WhatsApp group?"
- "List all members of my 'Family Chat' group"
- "Show me the most active participants in the 'Sales Team' group this week"
- "Get the admin list for the 'Customer Support' group"

### ⏰ Message Scheduling
- "Schedule a message to +1234567890 saying 'Happy Birthday!' to be sent tomorrow at 9 AM"
- "Set up a reminder message for the team group about the meeting next Friday at 2 PM"
- "Schedule a follow-up message to my client in 3 days asking about project status"

### 🔍 Contact Validation & Management
- "Check if the phone number +1555987654 is a valid WhatsApp number"
- "Verify if +44987654321 has WhatsApp installed"
- "Get the profile information for contact +1234567890"
- "Show me all my recent contacts from the past month"

### 📈 Analytics & Insights
- "Generate a report of my most frequent WhatsApp contacts this month"
- "Show me my busiest WhatsApp conversation days this week"
- "Analyze which groups have the highest message volume"
- "Count unread messages across all my chats"

### 🔔 Status & Monitoring
- "Check the delivery status of my last message to +1234567890"
- "Show me all failed message deliveries from today"
- "Monitor if my contact +1555123456 has read my recent messages"
- "Get the online status of my important business contacts"

### 🔄 Bulk Operations
- "Send the same announcement to all members of my 'Team Updates' group individually"
- "Broadcast a holiday greeting to my top 10 most contacted numbers"
- "Send a survey link to all participants in the 'Product Feedback' group"

### 🔐 Account Management
- "Show me my current Wassenger account usage and limits"
- "List all my connected WhatsApp devices and their status"
- "Get my account's message quota for this month"

### 🎯 Smart Automation
- "Set up an auto-reply for messages received outside business hours (9 AM - 5 PM)"
- "Create a workflow: when someone messages 'INFO', automatically send our company brochure"
- "Analyze sentiment in customer support conversations and flag negative ones"

## MCP streaming usage

If your MCP client supports [**HTTP streaming**](https://modelcontextprotocol.io/specification/2025-03-26/basic/transports#streamable-http) (previously known as Server-Sent Events or SSE transport), you can connect directly to the Wassenger MCP server without installing this package. This is the preferred method as it's faster and requires no local setup.

### Supported Clients

Most modern MCP clients support HTTP streaming, including:

- **Claude Desktop** (latest versions)
- **VS Code Copilot** with MCP extension
- **Cursor** (v0.48.0+)
- **Windsurf**
- **OpenAI Responses API**
- **ChatGPT** (Pro users have access, soon more users)

### Claude Desktop Configuration

Add this to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "wassenger": {
      "type": "http",
      "url": "https://api.wassenger.com/mcp?key=YOUR_WASSENGER_API_KEY"
    }
  }
}
```

Or using environment variables:

```json
{
  "mcpServers": {
    "wassenger": {
      "type": "http",
      "url": "https://api.wassenger.com/mcp?key=${WASSENGER_API_KEY}",
      "env": {
        "WASSENGER_API_KEY": "your-api-key-here"
      }
    }
  }
}
```

### VS Code Copilot Configuration

In VS Code settings (JSON format):

```json
{
  "mcp.servers": {
    "wassenger": {
      "url": "https://api.wassenger.com/mcp?key=YOUR_WASSENGER_API_KEY",
      "transport": "http-streaming"
    }
  }
}
```

### Benefits of HTTP Streaming

- ✅ **No local installation** required
- ✅ **Faster connection** times
- ✅ **Automatic updates** - always uses the latest server version
- ✅ **Better reliability** - no Node.js dependency issues
- ✅ **Simpler configuration** - just a URL

### Getting Your API Key

1. Sign up at [Wassenger.com](https://wassenger.com)
2. Go to your [API settings](https://app.wassenger.com/api)
3. Copy your API key
4. Replace `YOUR_WASSENGER_API_KEY` in the configuration above

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
        "API_KEY_GOES_HERE"
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
        "API_KEY_GOES_HERE"
      ]
```

* To enable detailed debugging logs, add the `--debug` flag. This will write verbose logs to `~/.mcp-auth/{server_hash}_debug.log` with timestamps and detailed information about the auth process, connections, and token refreshing.

```json
      "args": [
        "mcp-wassenger",
        "API_KEY_GOES_HERE",
        "--debug"
      ]
```

<!-- ### Transport Strategies

MCP Remote supports different transport strategies when connecting to an MCP server. This allows you to control whether it uses Server-Sent Events (SSE) or HTTP transport, and in what order it tries them.

Specify the transport strategy with the `--transport` flag:

```bash
npx mcp-wassenger $API_KEY --transport sse-only
```

**Available Strategies:**

- `http-first` (default): Tries HTTP transport first, falls back to SSE if HTTP fails with a 404 error
- `sse-first`: Tries SSE transport first, falls back to HTTP if SSE fails with a 405 error
- `http-only`: Only uses HTTP transport, fails if the server doesn't support it
- `sse-only`: Only uses SSE transport, fails if the server doesn't support it -->
<!--
### Static OAuth Client Metadata

MCP Remote supports providing static OAuth client metadata instead of using the mcp-wassenger defaults.
This is useful when connecting to OAuth servers that expect specific client/software IDs or scopes.

Provide the client metadata as a JSON string or as a `@` prefixed filepath with the `--static-oauth-client-metadata` flag:

```bash
npx mcp-wassenger $API_KEY --static-oauth-client-metadata '{ "scope": "space separated scopes" }'
# uses node readfile, so you probably want to use absolute paths if you're not sure what the cwd is
npx mcp-wassenger $API_KEY --static-oauth-client-metadata '@/Users/username/Library/Application Support/Claude/oauth_client_metadata.json'
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
npx mcp-wassenger $API_KEY --static-oauth-client-info "{ \"client_id\": \"$MCP_REMOTE_CLIENT_ID\", \"client_secret\": \"$MCP_REMOTE_CLIENT_SECRET\" }"
# uses node readfile, so you probably want to use absolute paths if you're not sure what the cwd is
npx mcp-wassenger $API_KEY --static-oauth-client-info '@/Users/username/Library/Application Support/Claude/oauth_client_info.json'
``` -->

### Claude Desktop

[Official Docs](https://modelcontextprotocol.io/quickstart/user)

In order to add an MCP server to Claude Desktop you need to edit the configuration file located at:

* macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
* Windows: `%APPDATA%\Claude\claude_desktop_config.json`

If it does not exist yet, [you may need to enable it under Settings > Developer](https://modelcontextprotocol.io/quickstart/user#2-add-the-filesystem-mcp-server).

Restart Claude Desktop to pick up the changes in the configuration file.
Upon restarting, you should see a hammer icon in the bottom right corner
of the input box.

### VS Code Extension / Copilot

[Official Docs](https://code.visualstudio.com/docs/copilot/copilot-extensibility-overview#_model-context-protocol-mcp). Add MCP servers through VS Code settings or via the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`) by searching for "MCP". Configuration is managed through VS Code's settings interface.

### Cursor

[Official Docs](https://docs.cursor.com/context/model-context-protocol). The configuration file is located at `~/.cursor/mcp.json`.

As of version `0.48.0`, Cursor supports unauthed SSE servers directly. If your MCP server is using the official MCP OAuth authorization protocol, you still need to add a **"command"** server and call `mcp-wassenger`.

### Windsurf

[Official Docs](https://docs.codeium.com/windsurf/mcp). The configuration file is located at `~/.codeium/windsurf/mcp_config.json`.

### Cline

[Official Docs](https://cline.bot/docs/mcp). The configuration file is located at `~/.cline/mcp_servers.json`.

### Continue.dev

[Official Docs](https://docs.continue.dev/customize/model-context-protocol). The configuration file is located at `~/.continue/config.json`. Continue.dev supports MCP servers for enhanced code context and AI-powered development workflows.

### Zed Editor

[Official Docs](https://zed.dev/docs/assistant/model-context-protocol). Configure MCP servers in Zed's settings to extend the AI assistant capabilities. Zed provides native MCP integration for seamless development experience.

### Jan AI

[Official Docs](https://jan.ai/docs/extensions/mcp). Jan AI supports MCP servers through its extension system, allowing you to integrate external tools and data sources with local AI models.

### Open WebUI

[Official MCP Integration](https://docs.openwebui.com/features/mcp). Open WebUI provides MCP support for connecting external tools and services to your self-hosted AI interface, enabling powerful workflow automation.

### Sourcegraph Cody

[Official Docs](https://sourcegraph.com/docs/cody/core-concepts/context). Cody supports MCP integration for enhanced code understanding and AI-assisted development across your entire codebase.

### OpenAI Responses API

You can use the Wassenger MCP server as a tool with OpenAI's new Responses API, which provides native MCP integration without requiring manual client setup.

#### JavaScript Example

```javascript
import OpenAI from 'openai';

const wassengerKey = process.env.WASSENGER_API_KEY || 'your-wassenger-api-key';

const openai = new OpenAI({
  apiKey: process.env.OPENAI_API_KEY,
});

// Send a WhatsApp message using OpenAI's Responses API with MCP
const response = await openai.responses.create({
  model: 'o1-mini',
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

#### Python Example

```python
import os
from openai import OpenAI

wassenger_key = os.getenv('WASSENGER_API_KEY', 'your-wassenger-api-key')

client = OpenAI(
    api_key=os.getenv('OPENAI_API_KEY')
)

# Send a WhatsApp message using OpenAI's Responses API with MCP
response = client.responses.create(
    model='o1-mini',
    tools=[
        {
            'type': 'mcp',
            'server_label': 'wassenger',
            'server_url': f'https://api.wassenger.com/mcp?key={wassenger_key}',
            'require_approval': 'never'
        }
    ],
    input='Send a WhatsApp message to +1234567890 saying "Hello from AI!"'
)

print('Response:', response)
```

#### Installation

Make sure to install the required dependencies:

**JavaScript:**
```bash
npm install openai
```

**Python:**
```bash
pip install openai
```

#### Environment Variables

Set your API keys as environment variables:

```bash
export OPENAI_API_KEY="your-openai-api-key"
export WASSENGER_API_KEY="your-wassenger-api-key"
```

This approach uses OpenAI's native MCP integration, which automatically handles tool discovery, execution, and communication with the Wassenger MCP server without requiring manual MCP client configuration.

<!--
## Building Remote MCP Servers

For instructions on building & deploying remote MCP servers, including acting as a valid OAuth client, see the following resources:

* https://developers.cloudflare.com/agents/guides/remote-mcp-server/

In particular, see:

* https://github.com/cloudflare/workers-oauth-provider for defining an MCP-comlpiant OAuth server in Cloudflare Workers
* https://github.com/cloudflare/agents/tree/main/examples/mcp for defining an `McpAgent` using the [`agents`](https://npmjs.com/package/agents) framework.

For more information about testing these servers, see also:

* https://developers.cloudflare.com/agents/guides/test-remote-mcp-server/

Know of more resources you'd like to share? Please add them to this Readme and send a PR! -->

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
        "API_KEY_GOES_HERE"
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
  "API_KEY_GOES_HERE",
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
npx -p mcp-wassenger@latest mcp-wassenger-client API_KEY_GOES_HERE
```

This will run through the entire authorization flow and attempt to list the tools & resources at the remote URL. Try this after running `rm -rf ~/.mcp-auth` to see if stale credentials are your problem, otherwise hopefully the issue will be more obvious in these logs than those in your MCP client.

## License

MIT
