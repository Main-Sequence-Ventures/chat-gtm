# MSV GTM Marketplace

A Claude plugin marketplace of go-to-market tools that Main Sequence shares with
its portfolio companies. Add the marketplace once, then install and update
plugins from it in one click.

## Plugins in this marketplace

| Plugin | What it does |
|--------|--------------|
| `chatgtm` | High-volume, high-customization outbound sales engine. Leadership-owned segment lock, verifiable-signal gating, non-AI-sounding drafts, CRM write-back, daily tempo report, and a status dashboard. Tool-agnostic — you connect your own CRM, mailbox, and enrichment provider during setup. |

---

## Before you start — what you need

ChatGTM is tool-agnostic and connects to your own accounts. A full pull needs
**three connectors**, not just enrichment:

- **Enrichment** — Apollo (or Clay, FullEnrich, …)
- **A read-only CRM** — HubSpot, Salesforce, Attio, …
- **A mailbox** — Gmail or Outlook

Setup will stall at the verify + test-pull steps if only one is connected, so
confirm all three are addable in your Claude plan before you begin.

> **You never paste API keys.** Apollo and the other tools connect as **Claude
> Connectors** via **Settings → Connectors** (OAuth). The skills explicitly tell
> you never to paste a token or key into chat — there is nowhere to put one, and
> you shouldn't.

## For portfolio companies — how to install

In Claude Cowork (or Claude Code):

1. **Add the marketplace** (one time):
   ```
   /plugin marketplace add Main-Sequence-Ventures/chat-gtm
   ```
   This is the GitHub `owner/repo`. (or paste the repo URL if you were given one)

2. **Install ChatGTM**:
   ```
   /plugin install chatgtm@msv-gtm-marketplace
   ```
   Note: `chatgtm@msv-gtm-marketplace` uses the **marketplace name** (from
   `marketplace.json`), which stays `msv-gtm-marketplace` regardless of the repo name.

3. **Set it up**: say **"set up ChatGTM"**. The wizard connects your own tools
   as Claude Connectors — a read-only CRM, Gmail or Outlook, and an enrichment
   provider such as Apollo, Clay, or FullEnrich — captures your segment, and
   runs a small test pull. Nothing is ever sent automatically.

When MSV ships an update, you get it by running:
```
/plugin marketplace update msv-gtm-marketplace
```

---

## For MSV — how to publish this to GitHub

This folder is a complete marketplace repo, published at
[`Main-Sequence-Ventures/chat-gtm`](https://github.com/Main-Sequence-Ventures/chat-gtm).
To publish it (or a copy) under the Main Sequence org:

1. **Create the repo** in the Main Sequence GitHub org (the live one is
   `Main-Sequence-Ventures/chat-gtm`). The repo name is what portfolio companies
   pass to `/plugin marketplace add`, so it must match exactly.

2. **Push these files** to the repo root (the `.claude-plugin/marketplace.json`
   must sit at the repository root):
   ```bash
   cd msv-gtm-marketplace
   git init
   git add .
   git commit -m "ChatGTM marketplace v1.1.0"
   git branch -M main
   git remote add origin git@github.com:Main-Sequence-Ventures/chat-gtm.git
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
