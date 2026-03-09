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
- All secrets are stored in the **Chezmoi** vault in 1Password. See README.md for naming conventions.

### Choosing encryption method

| Condition | Method | Example |
|---|---|---|
| File has a mix of plain config and secret values | **Template** (`onepasswordRead`) | `.wakatime.cfg` — only `api_key` is secret |
| Entire file is sensitive | **Age encryption** (`chezmoi add --encrypt`) | SSH private keys, credential bundles |

### Detecting secrets

When adding a new file with `chezmoi add`, **always inspect the file contents first**. If any value looks like a secret, **do not add it as a plain file**. Instead:

1. **Ask the user** whether the value is a secret that needs protection.
2. If yes, determine the correct method (template vs encrypt) using the table above.
3. Create or confirm the 1Password item in the Chezmoi vault before proceeding.
4. Add the file as a `.tmpl` (template) or with `--encrypt` (age) accordingly.

Values that should be treated as potential secrets:
- API keys, tokens, passwords, passphrases
- Private keys, certificates
- Webhook URLs containing tokens
- Any string that looks like `sk-`, `waka_`, `ghp_`, `xoxb-`, or similar prefixed credentials
