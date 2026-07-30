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

## How to install

Pick the method that matches your situation. They're ordered by least user effort.
The GitHub repo is `Main-Sequence-Ventures/chat-gtm`; once added, it registers as
the marketplace **`msv-gtm-marketplace`** (that name comes from `marketplace.json`
and never changes, regardless of the repo name).

### 🥇 Method 1 — Org-managed (zero effort for users) — *recommended within MSV*

An MSV admin enables it once for the whole workspace; it then **appears
automatically for every member on every platform (terminal, desktop, and Cowork
web)** with no action from users.

1. Go to **[claude.ai/admin-settings/claude-code](https://claude.ai/admin-settings/claude-code)**
   → **Managed settings** (requires an Owner role on a Teams/Enterprise plan).
2. Add:
   ```json
   {
     "extraKnownMarketplaces": {
       "msv-gtm-marketplace": {
         "source": { "source": "github", "repo": "Main-Sequence-Ventures/chat-gtm" },
         "autoUpdate": true
       }
     },
     "enabledPlugins": { "chatgtm@msv-gtm-marketplace": true }
   }
   ```

Users see one security-approval dialog the first time, then it's silent forever.

> **Note:** managed settings only reach people **inside MSV's own workspace**.
> Portfolio companies in their own separate orgs won't get it this way — use
> Method 2 for them.

### 🥈 Method 2 — Repo-committed config (external companies, one prompt)

Do the JSON once *for* them so they never touch it. Commit a `.claude/settings.json`
to a repo they open:

```json
{
  "extraKnownMarketplaces": {
    "msv-gtm-marketplace": {
      "source": { "source": "github", "repo": "Main-Sequence-Ventures/chat-gtm" }
    }
  },
  "enabledPlugins": { "chatgtm@msv-gtm-marketplace": true }
}
```

When they open (and trust) the repo, Claude prompts *"Install marketplace
msv-gtm-marketplace?"* — they click **Install** once, and ChatGTM loads. Works on
terminal, desktop, and Cowork.

### 🥉 Method 3 — Terminal one-liner (for CLI users)

```bash
claude plugin marketplace add Main-Sequence-Ventures/chat-gtm && \
claude plugin install chatgtm@msv-gtm-marketplace --scope project
```

Terminal CLI only (not Cowork). The equivalent interactive commands are
`/plugin marketplace add Main-Sequence-Ventures/chat-gtm` then
`/plugin install chatgtm@msv-gtm-marketplace`.

### Method 4 — Desktop app plugin browser (GUI, desktop only)

In the Claude **desktop app**: **Code** tab → **+** → **Plugins** → **Add
marketplace** → paste `Main-Sequence-Ventures/chat-gtm` → find **chatgtm** →
**Install**. Note: desktop-installed plugins do **not** carry over to Cowork/web
sessions.

### Method 5 — Manual settings.json (last resort)

If none of the above fit, a user can hand-edit `.claude/settings.json` (project) or
`~/.claude/settings.json` (everywhere) with the JSON from Method 2. Avoid this for
non-technical users — it's error-prone.

### Then set it up

Say **"set up ChatGTM"**. The wizard connects your own tools as Claude Connectors
— a read-only CRM, Gmail or Outlook, and an enrichment provider such as Apollo,
Clay, or FullEnrich — captures your segment, and runs a small test pull. Nothing
is ever sent automatically.

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
