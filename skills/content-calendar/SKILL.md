---
name: content-calendar
description: Read a Keytomic site's content calendar, follow a calendar run, and research keywords. Use when the user asks what articles are planned, what is scheduled for a day or a month, whether an article was written or published, how a calendar run is going, how many keyword searches are left, or wants keyword ideas with search volume.
---

# Content calendar

The content calendar holds a site's planned topics. Each topic has a scheduled day, a primary keyword with its search volume, difficulty and demand trend, and the planning fields: intent, funnel stage, audience, angles and author override. The daily automation writes the article on the scheduled day and, with automatic publishing on, publishes it.

These tools need the content MCP Scopes. A Connected App made before these scopes existed does not have them. Then the server refuses the call before the tool runs: the call fails with an authorization, insufficient scope or HTTP 403 error, sometimes with the text "Keytomic needs approval". This is not the `permission_denied` tool error. When a content tool call fails this way, tell the user to remove the Connected App on the Connected apps page in Keytomic, connect Keytomic again, and approve all the scopes. Stop until the user connects again.

## Steps

1. Find the site with `list_sites`. Never show a projectId or a topicId. Name a topic by its title and its scheduled day.
2. Pick the tool for the question:

   | The user asks about | Tool |
   | --- | --- |
   | What is planned for a day, a week or a month | `list_calendar_topics` with `from` and `to` (UTC, both included) |
   | Every topic, with the unscheduled ones | `list_calendar_topics` without dates. Page with `startRow` and `nextStartRow` |
   | One topic in full | `get_calendar_topic` |
   | How a calendar run is going | `get_content_calendar_progress`. Without a `runId`, it reads the run in progress |
   | How many keyword searches are left | `get_keyword_search_usage` |
   | Keyword ideas with volume, difficulty, competition, CPC and trend | `search_keywords` |

3. Before `search_keywords`, read `get_keyword_search_usage`. One call spends 1 of the site's monthly keyword searches, which all members of the site share. Tell the user what a search costs and what is left, then get a yes. Give the seed keyword and the related keywords with their numbers. Keywords with no search volume are for reference only.
4. When the workspace can open only some topics for now, `list_calendar_topics` returns only those and a `lockedTopicCount`. Say how many topics are locked for now. Do not offer a plan change or a billing link, also when the result has an upgrade text or an `upgradeUrl`.

## The report

- List the topics by scheduled day. For each: the title, the primary keyword with its volume, the review status, and whether the article is written or published.
- For a calendar run: the status, each stage with its summary, the number of topics it plans, and the message Keytomic shows when it stopped.

## Errors

- `retryable` true: wait `retryAfterSeconds`, then call the tool again.
- `topic_not_in_plan` or `upgrade_required` from `get_calendar_topic`: the topic is locked for now. Tell the user so. Do not offer a plan change or a billing link, also when the error has an upgrade text or an `upgradeUrl`.
- `subscription_required`, `onboarding_required` or `permission_denied`: give the user the message and the link in the error, then stop.
