# Teamhood MCP server

Connect Claude, ChatGPT, Copilot, Cursor, VS Code or any other assistant that supports the [Model Context Protocol](https://modelcontextprotocol.io) to [Teamhood](https://teamhood.com), the project management tool for matrix organizations. The assistant reads and updates your work as you, with your role's permissions.

The server is hosted by Teamhood: there is nothing to install or run. This repository holds its listing for the [MCP Registry](https://registry.modelcontextprotocol.io) and this guide. Full documentation is at [help.teamhood.com](https://help.teamhood.com/integrations/mcp/).

## Addresses

| Address | Offers |
|---|---|
| `https://api.teamhood.com/public/v1/mcp` | Every tool |
| `https://api.teamhood.com/public/v1/mcp/readonly` | Only the tools that read |

The address is the same for every organization: signing in decides which organization the assistant works in. Both use the Streamable HTTP transport.

## Connect an assistant

| Assistant | How |
|---|---|
| Claude, ChatGPT and others | Add a custom connector, or a remote MCP server, with the address |
| Claude Code | `claude mcp add --transport http teamhood https://api.teamhood.com/public/v1/mcp`, then `claude mcp login teamhood` |
| Cursor | In `.cursor/mcp.json`: `{"mcpServers": {"teamhood": {"url": "https://api.teamhood.com/public/v1/mcp"}}}` |
| VS Code | In `.vscode/mcp.json`: `{"servers": {"teamhood": {"type": "http", "url": "https://api.teamhood.com/public/v1/mcp"}}}` |

The assistant then asks you to sign in to Teamhood and approve it. Step by step: [Connect an assistant with MCP](https://help.teamhood.com/integrations/connect-mcp/).

### Signing in

- **OAuth.** The assistant opens Teamhood's sign-in page, and you approve it for one organization. Disconnect it any time under **Settings → Integrations → MCP → Connected apps**.
- **Personal token.** For an assistant that takes a request header instead, create a token under **Settings → Personal tokens** and send it in the `X-Api-Key` header or as `Authorization: Bearer <token>`.

## What an assistant can do

- Read the item types, their fields and the values each field allows
- Find, read, create, change and delete items, and link items that depend on each other
- Read, post, edit and delete comments; read, log, correct and delete time
- Read an item's history, and the images and text files attached to it
- Read working hours, time off and planned hours, and list the people and teams
- Change item types, fields, teams and invitations, through a connection allowed to
- Search Teamhood's help pages

Every tool and its limits: [MCP server reference](https://help.teamhood.com/integrations/mcp/).

## Safety

- An assistant acts as the person who connected it, and can do only what that person's role allows.
- The read-only address never offers a tool that changes anything.
- Changes to the organization's setup need a connection you approved for them, or a personal token.
- A deleted item goes to the trash, where it can be restored for 30 days.
- Changes to an item's title, description and fields show in its activity under your name with the **AI** label. Comments it posts carry the same label.

More: [How MCP works in Teamhood](https://help.teamhood.com/how-it-works/mcp/).

## Teamhood skill

Assistants that load skills can add the [Teamhood skill](https://help.teamhood.com/skills/teamhood/SKILL.md), which gives them the rules for working in Teamhood. See [Add the Teamhood skill to an assistant](https://help.teamhood.com/integrations/teamhood-skill/).

## Support

Choose **Contact us** at the bottom of the Help panel in Teamhood ([how](https://help.teamhood.com/get-started/contact-teamhood/)). Privacy policy: [teamhood.com/privacy-policy](https://teamhood.com/privacy-policy/).

## Publishing the registry listing

The listing is `server.json`, published as `com.teamhood/teamhood`. The registry proves ownership of `teamhood.com` from a TXT record on the domain's apex: `v=MCPv1; k=ed25519; p=<public key>`. The matching private key is the repository secret `MCP_PRIVATE_KEY`, as 64 hex characters.

To change the listing, edit `server.json`, raise its `version` (a published version cannot be replaced), and run the **Publish to MCP Registry** workflow.
