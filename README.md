# Shippable for Claude Code

Find the security leaks in your AI-built app before you ship it: database tables anyone can read, secret keys in the browser, edge functions that never check who is calling, unsigned payment webhooks. Read-only, free, runs on your machine.

## Install

```
/plugin marketplace add Egarter/shippable-plugin
/plugin install shippable@shippable
```

Then ask Claude: "Check my app for security leaks before I ship it."

## What you get

- **A skill** that scans your dev server and project with [`shippable-dev`](https://www.npmjs.com/package/shippable-dev), reports what matters, applies the fixes you agree to and re-scans to confirm.
- **The Shippable MCP server**, with `shippable_scan` (checks the running app from outside) and `shippable_review_code` (checks the code).

## What it runs and what it sends

The MCP server is the [`shippable-dev`](https://www.npmjs.com/package/shippable-dev) npm package (Apache-2.0), run with `npx` at a pinned version. It runs on your machine and only reads; it never writes to your app or database. A scan contacts:

- **The app you scan**, and the Supabase or Firebase backend its code uses. To prove a table is open it reads at most a few rows, and shows them only to you.
- **The OSV vulnerability database** (`api.osv.dev`): the names and versions of the packages in your lockfile. Never your code.
- **Certificate logs** (`crt.sh`, Cert Spotter), on live scans only: your domain name, to find your subdomains.
- **shippable.dev**: one anonymous usage count per scan (version, which command, how many findings at each severity, whether it ran in CI, a random install id). Never URLs, file names, code or findings. Turn it off with `SHIPPABLE_TELEMETRY=0`.

The visual report opens on shippable.dev, but the findings travel after the `#` in the link, which browsers never send to a server.

Only scan apps you own. Live sites need an ownership tag only the owner can deploy. More at [shippable.dev/mcp](https://shippable.dev/mcp).

## License

Apache-2.0. The license covers the code only; it doesn't grant use of the Shippable name or the Patchy mascot.
