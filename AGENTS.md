# AGENTS.md

Rules for AI agents working on this repository.

## Commit rules

- **No conventional commit prefixes.** Do not use `feat:`, `fix:`, `chore:`, etc.
- **Start with uppercase.** e.g. `Add global gitignore`, `Update tmux config`
- **No emoji** in commit messages.
- **Never commit without explicit user confirmation.** Always show staged changes and ask before committing.
- **Never push without explicit user confirmation.** Always show local commits and ask before pushing.

## Adding a managed file

When adding a new dotfile to this repo, **all three steps are required**:

1. Register with chezmoi: `chezmoi add <file>`
2. Update the **"What's managed"** table in `README.md` with the file path and description.
3. Stage, confirm with user, then commit and push.

## Modifying existing files

- **Do not alter the user's existing config content** unless explicitly asked.
- If the file already exists on the system, add it as-is first. Propose changes separately.

## README

- All prose in **English**.
- Keep it concise. No filler text.

## Secrets

- **Never commit plaintext secrets.** Use chezmoi templates (`onepasswordRead`) or age encryption.
- See the Secrets section in README.md for the two supported patterns.
