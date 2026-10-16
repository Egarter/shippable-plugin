---
name: check-app-security
description: Check the user's web app for security leaks with Shippable before they ship it. Use when the user asks whether their app is secure, safe to launch or ready to ship, mentions Supabase RLS, exposed API keys or edge functions, or built the app with Lovable, Bolt, v0, Replit or Cursor and is about to deploy.
---

# Check an app's security with Shippable

Shippable is a free, read-only security scanner for AI-built web apps. It runs on the user's machine with `npx`, needs no account, and never changes the app or its data.

## Rules

- Only scan the user's own app: their local dev server, or a live site they own. Never scan a site the user does not own, and never try to get around Shippable's ownership check or a host's bot protection.
- Never print secret values (keys, tokens, passwords) back to the user. Refer to them by variable name and file.
- Running `npx` needs network access. If the sandbox blocks it, ask the user to allow network for this command.

## Steps

1. **Find the project and the dev server.** Work from the project's root folder (where `package.json` is). Start the dev server if it isn't running (usually `npm run dev`) and note its URL, for example `http://localhost:5173`.

2. **Scan the dev server and the code together:**

   ```sh
   npx -y shippable-dev scan localhost:5173 --no-open
   ```

   Replace the port with the real one. A local scan checks the running app from outside (open database tables, secret keys in the bundle, vulnerable libraries) and reads the project folder (database migrations and policies, edge functions, webhooks, secrets in `.env` files, every dependency in the lockfile). Exit code 1 means critical or high findings, 0 means none, 2 means it needs ownership verification (live sites) or could not check anything.

3. **Report what matters.** List the findings most urgent first: severity, what is exposed, where (file and line), and the proof Shippable gives. Keep it short and plain.

4. **Read the "Worth a look" leads** at the end of the output. They are logic bugs patterns can't confirm: limits enforced only in the frontend, check-then-act races, one-time codes that work twice, refunds or credits that can run twice. Open each file, decide whether it is a real problem, and report only the ones you confirmed in the code.

5. **Fix, with the user's agreement.** Each finding comes with a fix. Apply the ones the user agrees to, one at a time, without changing unrelated code. Secret keys that were exposed must also be rotated by the user at the provider; say so.

6. **Re-scan to confirm.** Run the same command again and show that each fixed finding is gone.

7. **Always finish with the report.** Every scan's output ends with a "Full report" line and a file path. When you are done, give the user the path from the latest scan exactly as printed, and offer to open it (`open <path>` on macOS, `xdg-open <path>` on Linux, `start "" "<path>"` on Windows). It opens a visual report on shippable.dev with a copy button for each fix; the findings never reach Shippable's server. Never retype or shorten a long shippable.dev/report link yourself: it breaks. Use the file path.

## Live sites

To scan the deployed app, run `npx -y shippable-dev scan https://their-app.com --no-open`. The first run prints a meta tag that proves ownership. The user adds it inside `<head>` and redeploys, then the same command runs the scan. If the user wants a browser instead, point them to https://shippable.dev.

## More

- Install Shippable as an MCP server in other agents: https://shippable.dev/mcp
- Common questions (Supabase anon key, RLS, verify_jwt): https://shippable.dev/answers
- The CLI sends an anonymous usage count per scan (never URLs, code or findings); `SHIPPABLE_TELEMETRY=0` turns it off.
