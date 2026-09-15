# E2E PoC repo — Sanitizer bypass in anthropics/claude-code-action (H1 anthropic, Core asset)

Purpose: end-to-end proof that entity-encoded attributes and unterminated HTML comments survive `sanitizeContent()` in claude-code-action and reach the model prompt.

## Contents
- `.github/workflows/claude.yml` — workflow triggering `anthropics/claude-code-action@v1` on issues/issue_comment containing `@claude`.
- `issue-body.md` — the raw issue body to submit (contains `@claude` + 2 payload variants: terminated & unterminated HTML comments).
- `AGENTS.md` — a trap file to prove which memory files the CLI actually loads (defense-in-depth signal only; not a finding).

## How to run
1. Set `ANTHROPIC_API_KEY` as an Actions secret on this repo.
2. Open an issue with the content of `issue-body.md`.
3. Observe whether Claude replies with `BANANA-CONFIRM-42`.
4. Check the Actions log for the `stripHiddenAttributes` ordering bug proof: entity-encoded `alt&#61;` attributes survive sanitization.

## Expected outcome
- **Bypass works**: Claude replies with `BANAYA...`/`BANANA-CONFIRM- complies with the injected instruction → sanitization did NOT strip the hidden content.
- **Bypass fails**: Claude ignores the hidden instructions and answers normally.

## Safety note
All payloads are self-reporting (marker string) — non-destructive. The repo is private, run on my own GitHub account.