# Publishing & updating the MSV GTM Marketplace (maintainers)

This is the runbook for whoever at Main Sequence has GitHub org access. It
covers the one-time publish and the repeatable update flow. Portfolio companies
never see this file — it is for MSV.

Set your org handle once and reuse it below:

```bash
ORG=MainSequenceVentures          # <-- replace with the real GitHub org handle
REPO=msv-gtm-marketplace
```

If the org handle differs from the placeholder used in the docs, stamp it across
the repo before pushing:

```bash
grep -rl "MainSequenceVentures" . | xargs sed -i '' "s#MainSequenceVentures#$ORG#g"   # macOS
# (Linux: drop the '' after -i)
```

## One-time publish

```bash
cd msv-gtm-marketplace
git init
git add .
git commit -m "ChatGTM marketplace v1.1.0 (plugin v0.3.0)"
git branch -M main
git remote add origin git@github.com:$ORG/$REPO.git
git push -u origin main
```

Then make it reachable by portfolio companies — pick one:

- **Public repo** — simplest. Anyone with the name can add it. Safe: the plugin
  holds no secrets; each company connects their own tools.
- **Private repo + collaborators** — invite each company's GitHub team read-only.
  More control, more admin.

## Distribution message to send a portfolio company

> In Claude (Cowork or Claude Code), run:
>
>     /plugin marketplace add MainSequenceVentures/msv-gtm-marketplace
>     /plugin install chatgtm@msv-gtm-marketplace
>
> Then say "set up ChatGTM" and follow the wizard. (Attach the branded install
> one-pager.)

## Shipping an update (the important part)

Updates are pull-based: you push, companies pull. To release a change:

1. Edit files under `plugins/chatgtm/`.
2. Bump the version in **both**:
   - `plugins/chatgtm/.claude-plugin/plugin.json` (`"version"`)
   - `.claude-plugin/marketplace.json` (the chatgtm entry's `"version"`)
   Keep them identical. Use semver (0.3.0 → 0.3.1 patch, → 0.4.0 feature).
3. Add a dated entry to `plugins/chatgtm/CHANGELOG.md`.
4. Commit and push:
   ```bash
   git add . && git commit -m "chatgtm v0.4.0: <summary>" && git push
   ```
5. Companies receive it when they run:
   ```bash
   /plugin marketplace update msv-gtm-marketplace
   ```

There is no forced push to installed users — they pull on demand. If an update
is important, tell them to run the update command (a Slack/email nudge is enough).

## Adding a second plugin later

Drop the new plugin folder under `plugins/`, add an entry to the `plugins` array
in `.claude-plugin/marketplace.json`, bump the marketplace `metadata.version`,
and push. It appears for everyone who has added the marketplace.

## Sanity check before pushing

```bash
python3 -c "import json;json.load(open('.claude-plugin/marketplace.json'));print('marketplace.json OK')"
claude plugin validate plugins/chatgtm/.claude-plugin/plugin.json
```
