# Plan: github-dashboard

Goal: a plugin that shows a morning snapshot of the installer's relevant GitHub pull requests and issues (title, date, status, link) in the repos they choose to track, and flags new or changed ones. It replaces going to GitHub and searching by hand; it is not an archive. Each installer configures it; nothing is tied to one person's setup.

## Current focus
- (none) — answer the Open questions, then start Phase 1

## Open questions
- [ ] Issue scope, within the tracked repos: filed by them, assigned to them, or all open issues? (Decisions proposes open items only, whichever scope.) Answer: _
- [ ] Default refresh schedule? Answer: _

## Next

### Phase 1: MVP on one setup
- [ ] With the GitHub access set up here (the signed-in `gh` CLI, or a GitHub connector if one is added), check that search returns each PR's and issue's title, state, review state, dates and URL, limited to a list of repos, for PRs awaiting their review and PRs they created
- [ ] SKILL.md: one account, the repos to track and the PR and issue searches as constants at the top, plus the layout and sorting rules
- [ ] Two tabs, Pull requests and Issues, each with title, date, status and link, capped per tab with a link to the full GitHub search when more match
- [ ] Render the dashboard as an artifact page whose rows live in its database
- [ ] Each refresh replaces all rows with the fresh pull: rows not in the pull are deleted, in one batch write
- [ ] Check whether a plugin can ship a scheduled task, or only a step that creates one, and that the scheduled run can use the GitHub connector or `gh`
- [ ] Weekday-morning scheduled task that runs the skill and refreshes the rows
- [ ] Use it for a week on real PRs and issues, and note what to change before Phase 2

### Phase 2: configurable for any installer
- [ ] Turn every Phase 1 constant into a plugin option: repos to track, issue scope, row cap, refresh time
- [ ] Defaults come from the Open questions answers; the repo list has none, since the installer names it
- [ ] Setup step (`/github-dashboard setup`): checks for a GitHub connector or a signed-in `gh` and says how to set one up when neither is there, asks for unset options starting with the repos to track, creates the scheduled task
- [ ] Remove every author-specific value; test from a fresh install on a different GitHub account
- [ ] README in the shared layout, marketplace entry, root and `plugins/` README rows; `claude plugin validate .`

### Phase 3: alerts and notifications
- [ ] Before replacing the rows, compare the fresh pull with them to find new and changed items; keep no other history
- [ ] Mark PRs and issues that are new or changed since the last run at the top of their tab
- [ ] Notify when something is new or changed, such as a review requested, where the host supports notifications; stay quiet otherwise
- [ ] Option to turn alerts off, or limit them to chosen repos

## Done (last 10)
- (none)

## Decisions
- One plugin holds a skill and a scheduled task and ships no MCP server: it uses the installer's own GitHub access, a GitHub connector when one is set up or else the signed-in `gh` CLI, because both are already maintained and signed in, and a bundled server would duplicate them
- The skill holds the formatting and behaviour rules, because the scheduled run and an on-demand ask then produce the same dashboard
- The dashboard is an artifact page with its rows in the page's database, because a scheduled run can update rows without republishing (same as daily-organizer); proposed, confirm before Phase 1
- Each refresh overwrites the rows with a fresh pull and keeps no history, because the dashboard is a snapshot of what's relevant now, not an archive
- The page has two tabs, Pull requests and Issues, not one combined list, because each is read on its own; the cap is per tab for the same reason
- Repos come from an explicit list the installer sets, not every repo they can see or contribute to, because the installer decides what's relevant and the searches stay bounded
- The Pull requests tab combines PRs assigned to them for review and PRs they created, in one list, because both need their attention each morning
- Rows are open PRs and issues only, most recently updated first, capped at 25 per tab (50 in all), with a link to the full GitHub search when more match, because the database must not grow over time; proposed default, confirm before Phase 1
- Phase 1 hardcodes one setup, because the layout should settle before it turns into options
- Installer settings are plugin options, not text in SKILL.md, because the plugin is shared from a public repo
- The plugin ships no credentials, because each installer signs in to their own GitHub account through the connector or `gh`

## Parking lot
- (none)

## Gotchas
- Chat artifacts on claude.ai have no `db`; database-backed pages are made and written from Claude Code
- `ArtifactData` writes to an existing row need its `if_version`
