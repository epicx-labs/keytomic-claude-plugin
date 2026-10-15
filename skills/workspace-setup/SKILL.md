---
name: workspace-setup
description: Read and edit a Keytomic site's brand profile, Article Settings, indexing settings, author profiles, plan and Usage Limits. Use when the user asks what Keytomic knows about their brand, wants to change the brand voice, audience, guardrails, offer, tone, article length, authors, or post URL prefix, or asks what their plan allows.
---

# Workspace setup

The brand profile is the owner's. Content generation uses it only after the owner completes the brand profile review in Keytomic. Edits through these tools spend no credits.

## Steps

1. Find the site with `list_sites`. Never show a projectId.
2. Read the site before any advice or edit:
   - `get_project`: name and domain, the brand profile (metadata, audience, Brand Voice, Content Guardrails, Offer Positioning, competitors), whether the review is complete, the setup checklist and the Domain Rating.
   - `get_project_settings`: Article Settings, indexing settings, the automatic blog schedule and the author profiles with the default author.
   - `get_usage`: the plan, the subscription state, and each Usage Limit with what is used and what is left.
3. Report the state. Name what is missing from the setup checklist, whether the review is complete, and the Usage Limits that are near their cap.
4. For an edit, show the user the exact fields and the new values, then get a yes, then call the tool:

   | Edit | Tool | Note |
   | --- | --- | --- |
   | Brand profile fields | `update_brand_profile` | `brandVoice`, `audienceProfile` and `contentGuardrails` replace the whole section: send every field of the section with the current values from `get_project`. The other fields change one at a time. |
   | Article length, tone, voice, links, images, YouTube, author override | `update_article_settings` | Only the named fields change. |
   | URL prefix of the site's posts | `update_indexing_settings` | `null` clears it. |
   | Add an author | `create_author_profile` | The first author becomes the default. One call makes one author. |
   | Change an author | `update_author_profile` | Send every field with the current values from `get_project_settings`. |
   | Default author | `set_default_author_profile` | |
   | Remove an author | `delete_author_profile` | Ask the user first. The result says which author became the default. |

5. After the edit, read the site again and confirm the new values to the user.

## Limits

- The brand profile review, the automatic blog schedule, plan changes and purchases happen in Keytomic. Give the user the link from the tool: `profileUrl` or `billingUrl`.
- Competitors, the sitemap inventory, screenshots and the Domain Rating are not editable here.
- Put new content rules in Content Guardrails, Brand Voice or Offer Positioning. The legacy custom instructions field is read only.

## Errors

- `retryable` true: wait `retryAfterSeconds`, then call the tool again.
- `subscription_required`, `onboarding_required` or `permission_denied`: give the user the message and the link in the error, then stop.
