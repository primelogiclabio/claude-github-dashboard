# github-dashboard

A planned Claude skill: a morning snapshot of your GitHub pull requests and
issues across the repos you track, on a claude.ai dashboard.

**Status:** planning. Nothing to install yet. The plan, its open questions
and the decisions so far are in [`PLAN.md`](PLAN.md).

It follows the same approach as
[jira-dashboard](https://github.com/primelogiclabio/claude-jira-dashboard): an
artifact page on claude.ai whose rows live in the artifact's own database,
refreshed each morning by a scheduled task and on request, using your own
GitHub access (a GitHub connector or the signed-in `gh` CLI) rather than a
bundled server. Once it works, it will install from the
[claude-tools](https://github.com/primelogiclabio/claude-tools) marketplace.

## License

MIT, see [LICENSE](LICENSE).
