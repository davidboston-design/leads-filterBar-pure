---
name: figma
description: Build a prototype from a Figma design URL. Use when the user says /figma, pastes a figma.com URL, or asks to build from a Figma design.
---

# Figma

Connect to the Figma MCP server, fetch design context from a provided URL, and build a working prototype adapted to this project's stack.

## Workflow

1. **Verify Figma MCP server is connected**
   Call the MCP tool `get_metadata` on server `user-Figma` with a minimal request to check connectivity.
   If the call fails or the server is not found, show the user this setup guide and stop:

   > **Figma MCP server is not connected.** Follow these steps to set it up:
   >
   > **Remote Server (recommended):**
   > 1. Open a Figma Design file in your browser
   > 2. Switch to Dev Mode (Shift + D)
   > 3. In the right inspect panel, click "Set up an MCP client"
   > 4. Select **Cursor** and click **Add**
   > 5. Verify the URL is `https://mcp.figma.com/mcp`
   > 6. Click **Install**, then **Connect** to authenticate
   >
   > **Desktop Server (alternative):**
   > 1. Open the Figma desktop app and a Design file
   > 2. Switch to Dev Mode and enable the MCP server
   > 3. In Cursor, open Settings → MCP → Add Custom MCP with URL `http://127.0.0.1:3845/mcp`
   >
   > **Requirements:** Full or Dev seat on Professional, Organization, or Enterprise plan.
   > See [Figma MCP docs](https://help.figma.com/hc/en-us/articles/32132100833559) for details.

2. **Get the Figma URL**
   If the user provided a URL in their message, use it. Otherwise, ask the user for a Figma URL.
   Supported formats:
   - `figma.com/design/:fileKey/:fileName?node-id=:nodeId`
   - `figma.com/design/:fileKey/branch/:branchKey/:fileName` (use branchKey as fileKey)
   - `figma.com/make/:makeFileKey/:makeFileName`
   - `figma.com/board/:fileKey/:fileName` (FigJam — use `get_figjam`)

3. **Parse the URL**
   Extract `fileKey` and `nodeId` from the URL:
   - Convert `-` to `:` in the `node-id` query parameter
   - For branch URLs, use the `branchKey` as the `fileKey`

4. **Fetch design context**
   Call `get_design_context` on server `user-Figma` with the extracted `fileKey` and `nodeId`.
   This returns generated code (React + Tailwind), a screenshot, and contextual hints.

5. **Analyze the project stack**
   Before writing code, check:
   - Existing components in `components/` — reuse them where possible
   - Design tokens and CSS variables in `app/globals.css`
   - Layout patterns and conventions in existing pages (`app/` directory)
   - The `blueprint-component-index.json` for mapped component definitions

6. **Build the prototype**
   Adapt the Figma output to this project:
   - **Code Connect snippets** → use the mapped codebase component directly
   - **Component docs/links** → follow them for usage guidelines
   - **Design annotations** → follow notes, constraints, or instructions from the designer
   - **Design tokens as CSS variables** → map to the project's token system in `globals.css`
   - **Raw hex colors / absolute positioning** → use the screenshot as visual reference and translate to Tailwind utilities
   - Reuse existing project components instead of generating from scratch
   - Match existing code conventions (TypeScript, Tailwind, Next.js App Router)

7. **Report**
   Show the user:
   - What components/pages were created or modified
   - A summary of design decisions made during adaptation
   - Suggest running `/start` to preview the result locally

## Important

- The Figma output is a **reference**, not final code. Always adapt to this project's patterns.
- Prefer editing existing files over creating new ones.
- If the design includes multiple frames/screens, ask the user which to build first unless they specified.
- If `get_design_context` returns partial or unclear data, use `get_screenshot` for visual reference.
