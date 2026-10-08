# Funraise Taiwan Real Estate — Claude plugin

Ask Claude about Taiwan real estate and get answers from Funraise data. Type `/funraise` followed by your question, and Claude answers with the Funraise connector: actual transaction prices for sales, rentals and presales, office buildings, cadastral parcels and zoning, urban renewal areas, industrial parks, land readjustment zones, and the company registry. You can ask in Traditional Chinese or English.

在 Claude 裡輸入 `/funraise` 加上問題，Claude 就會用 Funraise 的資料回答：實價登錄（買賣／租賃／預售）、商辦大樓、地籍與使用分區、都市更新、產業園區、市地重劃、公司登記等。

## What's inside

| File | Purpose |
|---|---|
| `skills/funraise/SKILL.md` | The `/funraise` skill: how Claude picks data sources, reads tool schemas, and interprets responses |
| `skills/funraise/references/data-sources.md` | Data source reference that Claude reads when it needs to choose one |
| `skills/list-recipes/SKILL.md` | The `/list-recipes` skill: lists your personal, team and official recipes in one place, lets you pick one, and runs it with new inputs |
| `skills/save-recipe/SKILL.md` | The `/save-recipe` skill: saves the workflow from this conversation as a Funraise recipe, to your personal recipes or a team's, as a new recipe or a new version of an existing one |
| `.mcp.json` | The Funraise connector, `https://connector.mcp.funraise.ai/c/default/mcp` |

## Install and use

1. Install the plugin from **Customize > Plugins** in Claude.
2. **Connect the connector — installing the plugin does not do this for you.** The plugin ships the connector, but it starts as *Not added*: open the plugin's **Connectors** tab and press **Connect** on `funraise`. Sign in with Google or Microsoft. New accounts complete a short profile on first sign-in (currently in Traditional Chinese); you are returned to the authorization page automatically. A personal account includes a free trial of 300 tool executions per month.
3. In a chat, type `/`, choose `funraise`, and write your question. For example:
   `/funraise What was the average unit price of office transactions in Xinyi District in 2025?`

If Claude says it cannot find any Funraise tools, step 2 has not been completed in that conversation.

Requirements: a Pro, Max, Team, or Enterprise plan, with **Code execution and file creation** turned on under **Settings > Capabilities** (skills run in Claude's sandbox).

In Claude Code the same skill is `/funraise:funraise`, and the connector can also be added from the terminal:

```bash
claude mcp add funraise https://connector.mcp.funraise.ai/c/default/mcp
```

### Save a workflow as a recipe

After finishing a piece of work with Funraise data, type `/save-recipe` (in Claude Code, `/funraise:save-recipe`), or just say 「把這次的做法存起來」. Claude asks where to save it — your personal recipes or one of your teams' — and whether to add a new recipe or update an existing one, shows you a summary to confirm, then saves it.

### Run a saved recipe

Type `/list-recipes` (in Claude Code, `/funraise:list-recipes`), or say 「列出我的食譜」. Claude gathers your personal recipes, your teams' recipes and the official recipes from every Funraise connector you've connected, lists them in one place, and lets you pick one. It then asks for this time's inputs before running the recipe.

Recipes are saved through the connector you choose: a personal connector saves personal recipes, a team connector saves that team's recipes. The recipe feature is currently open on some connections only; if yours doesn't have it yet, the skill says so instead of saving.

Before the directory listing is live, you can add this repository as a marketplace from **Customize > Plugins > Add > Add marketplace**.

## Data and privacy

The plugin itself contains only instructions and the connector address. It runs no code and stores nothing.

When Claude uses the connector, your question's search parameters (such as city, district, parcel number, or company name) are sent to `connector.mcp.funraise.ai`, operated by Funraise, which queries Funraise's data and returns the results. Signing in to the connector is handled by Funraise's OAuth server at `app.mcp.funraise.ai`. Some optional data sources (property transcripts) can incur charges; the skill always asks for your explicit confirmation before running one.

Privacy policy: https://mcp.funraise.tw/privacy/

## Support

https://mcp.funraise.tw/support/

## License

Apache-2.0. See [LICENSE](LICENSE).
