---
name: start
description: Start a local npm dev server. Use when the user says /start, "start server", "run dev", or asks to start the app locally.
---

# Start

Start the local npm development server.

## Workflow

1. **Verify Node.js is installed**
   Run `node --version`. If the command fails or is not found, inform the user that Node.js is not installed and provide a link to https://nodejs.org. Stop here.

2. **Check for package.json**
   Confirm `package.json` exists in the project root. If missing, inform the user this doesn't appear to be an npm project and stop.

3. **Check for node_modules**
   If the `node_modules` directory does not exist, run `npm install` first and wait for it to complete.

4. **Check for an already-running dev server**
   List active terminals to see if a dev server is already running. If so, inform the user and provide the URL instead of starting a duplicate.

5. **Clear the Next.js cache**
   Delete the `.next` directory if it exists (`Remove-Item -Recurse -Force .next` on Windows, `rm -rf .next` on macOS/Linux). This prevents stale module cache errors (`MODULE_NOT_FOUND`) that occur when new route files were created while a previous server was running.

6. **Start the server**
   Run `npm run dev` as a background process (set `block_until_ms: 0` so it doesn't block).

7. **Verify startup**
   Read the terminal output after a few seconds to confirm the server started successfully. Look for the local URL (e.g. `http://localhost:3000`).

8. **Report**
   Show the user:
   - The local URL to open in their browser
   - That the server is running in the background
   - How to stop it (e.g. kill the process or close the terminal)

## Important

- Always run the server as a background process so the agent remains responsive.
- If the `dev` script doesn't exist in `package.json`, check for `start` and use that instead.
- If the port is already in use, inform the user and suggest they stop the other process or use a different port.
