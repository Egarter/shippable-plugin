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

Only scan apps you own. Live sites need an ownership tag only the owner can deploy. More at [shippable.dev/mcp](https://shippable.dev/mcp).

## License

Apache-2.0. The license covers the code only; it doesn't grant use of the Shippable name or the Patchy mascot.
