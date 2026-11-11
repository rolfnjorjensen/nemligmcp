# Running this fork locally

Use Node 22 or newer. Install the locked dependencies without lifecycle scripts,
then build and test:

```sh
npm ci --ignore-scripts
npm run build
npm test
```

Create `.env.local` with the following content. This file is ignored by Git and
Docker; keep its permissions at `600`. Leave credentials empty for anonymous
product searches, or enter your own Nemlig email and password privately.

```dotenv
NEMLIG_USER=
NEMLIG_PASS=
MCP_TRANSPORT=
```

`npm run start:local` starts the stdio MCP server and reads `.env.local`.
Configure your MCP client's command as the absolute path reported by
`command -v node`, with arguments:

```json
["--env-file=/Users/rolf/projects/nemligmcp/.env.local", "/Users/rolf/projects/nemligmcp/dist/index.js"]
```

For a local HTTP server, run:

```sh
npm run start:local -- --http
```

It binds to `127.0.0.1:3000`; the MCP endpoint is `http://127.0.0.1:3000/mcp`.
Do not change the bind address for ordinary local use. Avoid the upstream npm
package or Docker `latest` when you intend to run these patches.

This fork validates outbound API origins and product paths, blocks redirects,
sets a 15-second API timeout, upgrades the MCP SDK and Zod, limits HTTP request
bodies to 64 KiB, rejects JSON-RPC batches, and caps tool-input element counts.
Authenticated tools can still change your basket and reserve delivery slots.
This personal fork is marked private for npm publishing. Upstream release tooling
and automated publishing have been removed; GitHub Actions runs checks only.
