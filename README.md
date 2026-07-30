# MSV GTM Marketplace

A Claude plugin marketplace of go-to-market tools that Main Sequence shares with
its portfolio companies. Add the marketplace once, then install and update
plugins from it in one click.

## Plugins in this marketplace

| Plugin | What it does |
|--------|--------------|
| `chatgtm` | High-volume, high-customization outbound sales engine. Leadership-owned segment lock, verifiable-signal gating, non-AI-sounding drafts, CRM write-back, daily tempo report, and a status dashboard. Tool-agnostic — you connect your own CRM, mailbox, and enrichment provider during setup. |

---

## For portfolio companies — how to install

In Claude Cowork (or Claude Code):

1. **Add the marketplace** (one time):
   ```
   /plugin marketplace add MainSequenceVentures/msv-gtm-marketplace
   ```
   (or paste the repo URL if you were given one)

2. **Install ChatGTM**:
   ```
   /plugin install chatgtm@msv-gtm-marketplace
   ```

3. **Set it up**: say **"set up ChatGTM"**. The wizard connects your own tools
   (CRM, Gmail or Outlook, and an enrichment provider such as Apollo, Clay, or
   FullEnrich), captures your segment, and runs a small test pull. Nothing is
   ever sent automatically.

When MSV ships an update, you get it by running:
```
/plugin marketplace update msv-gtm-marketplace
```

---

## For MSV — how to publish this to GitHub

This folder is a complete marketplace repo. To publish it under the Main
Sequence org and share it with portfolio companies:

1. **Create the repo** in the Main Sequence GitHub org, e.g.
   `MainSequenceVentures/msv-gtm-marketplace`.

2. **Push these files** to the repo root (the `.claude-plugin/marketplace.json`
   must sit at the repository root):
   ```bash
   cd msv-gtm-marketplace
   git init
   git add .
   git commit -m "ChatGTM marketplace v1.0.0"
   git branch -M main
   git remote add origin git@github.com:MainSequenceVentures/msv-gtm-marketplace.git
   git push -u origin main
   ```

3. **Make it reachable by portfolio companies.** Pick one:
   - **Public repo** — simplest. Anyone with the name/URL can add it. Fine because
     the plugin holds no secrets (companies connect their own tools).
   - **Private repo + collaborators** — invite each portfolio company's GitHub
     user/team as read-only collaborators. More control, more admin.

4. **Tell companies to add it** with the two commands in the section above.

## How to ship updates

1. Edit the plugin under `plugins/chatgtm/`.
2. Bump `version` in **both** `plugins/chatgtm/.claude-plugin/plugin.json` and the
   matching entry in `.claude-plugin/marketplace.json`.
3. Commit and push. Companies pull it with `/plugin marketplace update`.

## Repo layout

```
msv-gtm-marketplace/
├── .claude-plugin/
│   └── marketplace.json         # marketplace manifest (lists plugins)
├── plugins/
│   └── chatgtm/                 # the plugin itself
│       ├── .claude-plugin/plugin.json
│       ├── skills/
│       ├── CONNECTORS.md
│       └── README.md
└── README.md
```

## Adding more plugins later

Drop the new plugin folder under `plugins/`, add an entry to the `plugins` array
in `marketplace.json`, and push. It appears for everyone who has added the
marketplace.
