# Joy API and MCP server for ChatGPT and Claude

Connect ChatGPT, Claude and other AI agents to Joy through Cotlet.dev for wedding guests, RSVPs, schedules, registry gifts and messages. The live Cotlet.dev page describes 15 hosted Joy operations. Joy has no public API or MCP server according to that page, checked 24 October 2026.

## What you can ask

> Show guests who have not responded to the RSVP.

> Count meal choices for the caterer.

> Prepare an RSVP reminder for my approval.

> Read the wedding schedule.

> Review received gifts and prepare thank-you notes.

## Connect Joy

Start at https://cotlet.dev/signup?app=joy, sign in to Joy in its Cloud Browser and enable it for agents. For editors and scripts, create your API key in the Cotlet.dev console and export it as `COTLET_API_KEY`.

### ChatGPT

Add `https://cotlet.dev/mcp/v1/connect` as a connector and sign in with Cotlet.dev.

### Claude

Add `https://cotlet.dev/mcp/v1/connect` in Settings > Connectors and sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Local browser adapter (stdio)

This repo contains a local MCP adapter for the hosted Cotlet.dev Cloud Browser. The seven local browser helpers below are distinct from the 15 Joy operations documented on the integration page. It speaks MCP over stdio, lists its Joy tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the adapter directly from this repository:

```bash
export COTLET_API_KEY="your-key"
python3 joy_agent.py serve
```

Or with Docker:

```bash
docker build -t joy-mcp .
docker run -i --rm -e COTLET_API_KEY joy-mcp
```

Tools: `joy_guide`, `joy_status`, `joy_open`, `joy_read`, `joy_click`, `joy_type`, `joy_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, using the standard library. From this repository:

```bash
export COTLET_API_KEY="your-key"
python3 joy_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 joy_agent.py status    # is Joy ready in your Cotlet.dev account
python3 joy_agent.py guide     # Joy screens and sign-in link
python3 joy_agent.py read      # open Joy, read the page, close the browser
python3 joy_agent.py tools     # list Cotlet.dev tools
python3 joy_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from Joy users

**Does Joy have a public API or MCP server?**

The Cotlet.dev integration page reports no public Joy API or MCP server, checked 24 October 2026. Cotlet.dev provides a hosted connection.

**What can I do with Joy through Cotlet.dev?**

Read guests, RSVP summaries, schedules and received gifts; update guests, tags, schedule events and registry gifts; prepare reminders, messages and thank-you notes. Review recipients and message contents before sending.

**Is this an official Joy integration?**

No. Cotlet.dev is independent and is not affiliated with Joy.

## Source

https://cotlet.dev/integrations/joy-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=joy

Start here: https://cotlet.dev/signup?app=joy
