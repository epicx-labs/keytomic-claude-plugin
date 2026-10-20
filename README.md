# Keytomic for Claude

Keytomic is an SEO platform. This plugin connects Claude to one Keytomic workspace through the Keytomic MCP server. It adds four skills that run the common SEO workflows with its tools: Website Audits, Search Console reviews, brand profile and settings, and the content calendar.

## What the plugin does

- **Website Audit**: start an audit of a site, follow it to the end, and read the findings by category, with the affected pages and the recommended fix.
- **Search Console review**: find the top, new, improving and declining queries and pages, the low-hanging fruits, keyword cannibalization, zombie pages, question queries, and the raw Search Analytics data.
- **Workspace setup**: read a site's brand profile, settings, author profiles, Usage Limits and integrations. Edit the brand profile, the Article Settings, the indexing settings and the author profiles after your yes.
- **Content calendar**: research keywords, read and edit the planned topics, add topics, start a calendar run and follow it, write, read and edit articles, and generate their hero images. Each action that uses credits happens only after your yes.

## Requirements

- A Keytomic workspace on a paid plan or in a trial. A workspace with no active plan, or one still in Onboarding, gets an error with a link to Keytomic instead of data.
- A Google Search Console property connected to the site, for the Search Console tools. The other tools do not need it.
- Claude Code 2.1.292 or later for the one-line install below.

## Install

From the Claude directory: add **Keytomic** on claude.ai. It then loads in Claude, Cowork and Claude Code.

From this repository, in a terminal:

```bash
claude plugin install keytomic --marketplace epicx-labs/keytomic-claude-plugin
```

Or inside a Claude Code session:

```text
/plugin install keytomic --marketplace epicx-labs/keytomic-claude-plugin
```

For one session only, from a clone of this repository:

```bash
claude --plugin-dir ./keytomic-claude-plugin
```

## Sign in

The first tool call asks you to sign in to Keytomic in your browser and to choose a workspace. In Claude Code, run `/mcp`, select **keytomic**, and complete the sign-in. The connection is a Connected App. Keytomic asks for these MCP Scopes: workspace read and write, audit read and run, Search Console read and write, and content read and write. On the consent page, you can clear a scope that you do not want to give. You can disconnect the Connected App at any time on the Keytomic MCP page in Keytomic, under Connected apps.

If the Connected App does not have a scope, the server refuses the tools of that scope with an HTTP 403 `insufficient_scope` response. This occurs when you cleared the scope on the consent page, or when you connected Keytomic before the content scopes were available. Claude shows an authorization or insufficient scope error for them, sometimes with the text "Keytomic needs approval". This error is not the `permission_denied` tool error, and a role change does not correct it. The other tools continue to operate. First, sign in to Keytomic again and approve all the scopes. In Claude Code, run `/mcp`, select **keytomic**, and sign in again. If the error continues, open the Keytomic MCP page in Keytomic and click **Disconnect** on the app's row under Connected apps. Then connect Keytomic again and approve all the scopes.

## What the plugin sends and reads

The plugin has no code of its own. It declares one remote MCP server, `https://app.keytomic.com/mcp`, and its skills are text files. Claude sends each tool call to that server with your sign-in token. The server reads and writes only the data of the workspace you chose. The plugin sends data to no other destination.

Tool calls that spend credits or start work, such as a new audit, a keyword search, a calendar run, an article or a hero image, happen only after you say yes. Edits to the brand profile and the settings spend no credits.

## Skills

| Skill | Use it for |
| --- | --- |
| `website-audit` | Run or read a Website Audit and get a report of the findings |
| `search-console-review` | Answer questions about Google Search performance with the insight tools |
| `workspace-setup` | Read and edit the brand profile, the settings and the author profiles, and read the integrations |
| `content-calendar` | Research keywords, plan and edit the calendar topics, start a calendar run, write and edit articles, and generate hero images |

## Support

Write to hi@keytomic.com. The privacy policy is at [keytomic.com/privacy-policy](https://keytomic.com/privacy-policy).

## License

MIT. See `LICENSE`.
