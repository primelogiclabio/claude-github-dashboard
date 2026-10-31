# Plan: github-dashboard

Goal: a plugin that shows a morning dashboard of the installer's GitHub pull requests and issues (title, date, status, link) from their own account, and flags new or changed ones. Each installer configures it; nothing is tied to one person's setup.

## Current focus
- (none) — answer the Open questions, then start Phase 1

## Open questions
- [ ] Repo/org scope: just the installer's own repos, or anywhere they're a contributor? Answer: _
- [ ] PR scope: authored by them, awaiting their review, or both? Answer: _
- [ ] Issue scope: filed by them, assigned to them, or all open issues? Answer: _
- [ ] Default refresh schedule? Answer: _

## Next

### Phase 1: MVP on one setup
- [ ] Bundle GitHub's official MCP server in the plugin's `.mcp.json`, and sign in once
- [ ] Check that its search returns each PR's and issue's title, state, review state, dates and URL
- [ ] SKILL.md: one account and fixed PR and issue searches as constants at the top, plus the layout and sorting rules
- [ ] Two sections, Pull requests and Issues, each with title, date, status and link
- [ ] Render the dashboard as an artifact page whose rows live in its database
- [ ] Check whether a plugin can ship a scheduled task, or only a step that creates one
- [ ] Weekday-morning scheduled task that runs the skill and refreshes the rows
- [ ] Use it for a week on real PRs and issues, and note what to change before Phase 2

### Phase 2: configurable for any installer
- [ ] Turn every Phase 1 constant into a plugin option: repo/org scope, PR scope, issue scope, refresh time
- [ ] Defaults come from the Open questions answers
- [ ] Setup step (`/github-dashboard setup`): asks for unset options, signs in to GitHub, creates the scheduled task
- [ ] Remove every author-specific value; test from a fresh install on a different GitHub account
- [ ] README in the shared layout, marketplace entry, root and `plugins/` README rows; `claude plugin validate .`

### Phase 3: alerts and notifications
- [ ] Store each run's snapshot (number, state, review state, updated) in the artifact database
- [ ] Mark PRs and issues that are new or changed since the last run at the top
- [ ] Notify when something is new or changed, such as a review requested, where the host supports notifications; stay quiet otherwise
- [ ] Option to turn alerts off, or limit them to chosen repos

## Done (last 10)
- (none)

## Decisions
- One plugin bundles the GitHub MCP connector, a skill and a scheduled task, because one install should give the whole morning flow
- The skill holds the formatting and behaviour rules, because the scheduled run and an on-demand ask then produce the same dashboard
- The dashboard is an artifact page with its rows in the page's database, because a scheduled run can update rows without republishing (same as daily-organizer); proposed, confirm before Phase 1
- Phase 1 hardcodes one setup, because the layout should settle before it turns into options
- Installer settings are plugin options, not text in SKILL.md, because the plugin is shared from a public repo
- The plugin ships no credentials, because each installer signs in to their own GitHub account through the connector

## Parking lot
- (none)

## Gotchas
- Chat artifacts on claude.ai have no `db`; database-backed pages are made and written from Claude Code
- `ArtifactData` writes to an existing row need its `if_version`
