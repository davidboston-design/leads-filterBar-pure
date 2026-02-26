---
name: deploy
description: Commit all changes and push to the remote repository. Use when the user says /deploy, "deploy", "push changes", or asks to commit and push in one step.
---

# Deploy

Stage all changes, auto-generate a commit message from the diff, commit, and push to the remote.

## Workflow

1. **Verify git is installed**
   Run `git --version`. If the command fails or is not found, inform the user that git is not installed and provide a link to https://git-scm.com/downloads. Stop here.

2. **Check for changes**
   Run `git status` and `git diff --staged` and `git diff` to see what changed.
   If there are no changes (no untracked files, no modifications), inform the user there is nothing to deploy and stop.

3. **Stage everything**
   Run `git add -A` to stage all changes.

4. **Generate a commit message**
   Run `git diff --cached --stat` and `git diff --cached` to analyze staged changes, then write a commit message:
   - First line: concise summary of the change (imperative mood, max 72 chars)
   - Blank line, then a short body (2-4 bullet points) if multiple files or logical changes are involved
   - Use conventional-style prefixes when appropriate: `feat:`, `fix:`, `refactor:`, `style:`, `docs:`, `chore:`
   - Always pass the message via HEREDOC:
     ```
     git commit -m "$(cat <<'EOF'
     feat: add dark mode toggle

     - Add state management for theme preference
     - Update global styles with dark variants
     EOF
     )"
     ```

5. **Push**
   Run `git push`. If rejected (non-fast-forward), run `git pull --rebase` then `git push` again.

6. **Report**
   Show the user:
   - Commit hash and message
   - Files changed summary
   - Push result (remote URL and branch)

## Important

- Do NOT use `git push --force` unless the user explicitly asks for it.
- Do NOT amend previous commits.
- If there are merge conflicts after pull --rebase, stop and ask the user for guidance.
- Warn (but don't block) if `.env`, credentials, or secret files are about to be committed.
