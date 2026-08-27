# Watchitude Shopify Theme — Working Rules

## What this repo is

Theme code for the Watchitude Shopify store, migrated from Salesforce Commerce Cloud.

- Store: `watchitude-2026.myshopify.com` (plan: Shopify, USD, US/Eastern)
- Repo: `github.com/Watchitude/Shopify` (private)
- Local path: `C:\dev\watchitude-theme`
- OS/shell: Windows, PowerShell. Use PowerShell syntax, not bash.
- Base theme: Dawn, pulled from the live theme 2026-08-27. No `blocks/` directory,
  so this is a pre-blocks Dawn release. TODO: record exact version from
  `shopify theme info`.

## Branches and themes

| Branch | Shopify theme | Role |
|---|---|---|
| `main` | not connected | Clean baseline / promotion target |
| `staging` | GitHub-connected theme | Where all work happens |

Other themes in the store, both unconnected to Git:

- `Dawn` — id `188019540013` — currently LIVE
- `Copy of Dawn` — id `188020424749` — unpublished, redundant

TODO: record the connected theme's name and numeric id here once confirmed.

## Hard rules

1. **`git pull` before editing anything.** Shopify commits admin edits to `staging`
   on its own, as `shopify[bot]`. The local copy goes stale without any action here.
2. **Never `shopify theme push --live`.** Never push to theme `188019540013`.
   Deploy by merging into the connected branch and letting Shopify sync.
3. **Never guess a schema, API shape, file format, or spec.** Fetch the authoritative
   source and verify. If verification is genuinely impossible, say so explicitly and
   label the assumption — do not ship it silently. Guessed Dawn and Shopify schemas
   have already caused avoidable import failures on this project.
4. **Settings go through the theme editor; code goes through this repo.**
   `config/settings_data.json` is generated. Prefer the editor for anything that is a
   theme setting. If it must be edited locally, validate before committing:
   `Get-Content config\settings_data.json -Raw | ConvertFrom-Json | Out-Null`
5. **Nothing private in this repo.** Shopify's GitHub app can read the whole
   repository and the sync cannot be disabled. No migration CSVs, price lists,
   customer data, exports, or credentials. Migration working files stay in
   `Dropbox\Claude\Watchitude.com 2026`, never here.
6. **Keep the seven theme folders at the repo root.** `assets`, `config`, `layout`,
   `locales`, `sections`, `snippets`, `templates`. Any nesting breaks the GitHub
   connection, and it fails silently.

## Everyday commands

```powershell
git pull                                                  # always first
shopify theme dev --store watchitude-2026.myshopify.com    # preview, 127.0.0.1:9292
shopify theme check                                        # lint before pushing
git add -A; git commit -m "..."; git push                  # deploys to staging theme
```

`shopify theme list --store watchitude-2026.myshopify.com` shows theme ids and roles.
The dev theme created by `theme dev` is deleted on `shopify auth logout`.

## Known gotchas

- **The code editor overwrites GitHub with no conflict warning.** Shopify's admin code
  editor gives no conflict alerts; its version of a file wins. Avoid it — use VS Code.
- **A disconnected branch cannot be reconnected.** Reconnecting creates a whole new
  theme. Don't disconnect casually.
- **Commit attribution is wrong by design.** Bot commits read
  "Theme last edited by: Albert Hakim" because the store owner record carries that
  name against David's email. Edits attributed to Albert may well be David's.
- **Batched commits.** Admin edits saved within ~10 seconds are collapsed into one
  commit, so the named editor is whoever saved last, not a per-file record.

## Migration context

The store is pre-launch. The SFCC catalog was pruned to 249 KEEP / 489 DROP products,
58 of them slap watches. 42 products still need copy and photography before launch
(9 rescued orphans plus 22 new styles). Product import and starting inventory are
handled outside this repo — this repo is storefront code only.

7. **The Shopify admin MCP writes to the live store with no preview.** Do not use it
   for product, price, inventory, or collection changes during theme work. Theme work
   uses the repo and Shopify CLI only. Any store data mutation gets confirmed with
   David first, in chat, before the call is made.