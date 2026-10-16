# Keytomic for Claude

Keytomic is an SEO platform. This plugin connects Claude to one Keytomic workspace through the Keytomic MCP server. It adds four skills that run the common SEO workflows with its tools: Website Audits, Search Console reviews, brand profile and settings, and the content calendar.

## What the plugin does

- **Website Audit**: start an audit of a site, follow it to the end, and read the findings by category, with the affected pages and the recommended fix.
- **Search Console review**: find the top, new, improving and declining queries and pages, the low-hanging fruits, keyword cannibalization, zombie pages, question queries, and the raw Search Analytics data.
- **Workspace setup**: read a site's brand profile, settings, author profiles and Usage Limits, and edit them after your yes.
- **Content calendar**: read the planned topics by day, follow a calendar run, and research keywords.

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

The first tool call asks you to sign in to Keytomic in your browser and to choose a workspace. In Claude Code, run `/mcp`, select **keytomic**, and complete the sign-in. The connection is a Connected App with these MCP Scopes: workspace read and write, audit read and run, Search Console read and write, and content read and write. You can remove the Connected App at any time on the Connected apps page in Keytomic.

If you connected Keytomic before the content scopes were available, the server refuses the content calendar tools. Claude shows an authorization or permission error for them, sometimes with the text "Keytomic needs approval". The other tools continue to operate. Remove the Connected App on the Connected apps page in Keytomic, then connect Keytomic again and approve all the scopes.

## What the plugin sends and reads

The plugin has no code of its own. It declares one remote MCP server, `https://app.keytomic.com/mcp`, and its skills are text files. Claude sends each tool call to that server with your sign-in token. The server reads and writes only the data of the workspace you chose. The plugin sends data to no other destination.

Tool calls that spend credits or start work, such as a new audit or a keyword search, happen only after you say yes. Edits to the brand profile and the settings spend no credits.

## Skills

| Skill | Use it for |
| --- | --- |
| `website-audit` | Run or read a Website Audit and get a report of the findings |
| `search-console-review` | Answer questions about Google Search performance with the insight tools |
| `workspace-setup` | Read and edit the brand profile, the settings and the author profiles |
| `content-calendar` | Read the planned topics, follow a calendar run, and research keywords |

## Support

Write to hi@keytomic.com. The privacy policy is at [keytomic.com/privacy-policy](https://keytomic.com/privacy-policy).

## License

MIT. See `LICENSE`.
