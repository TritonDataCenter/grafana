![Grafana Logo (Light)](docs/logo-horizontal.png#gh-light-mode-only)
![Grafana Logo (Dark)](docs/logo-horizontal-dark.png#gh-dark-mode-only)

This repository represents the version of
[Grafana](https://github.com/grafana/grafana) that is used as part of
[Triton](https://github.com/TritonDataCenter/triton).

This fork uses fixes in go dependencies from `jperkin`, to make them build
on illumos.

## Repository Management

This repository is downstream of
[Grafana](https://github.com/grafana/grafana).

To better understand and maintain our differences from Grafana, we try to
manage branches and tags in a specific fashion. First and foremost, all
branches and tags from the upstream Grafana repository are mirrored here.

Anything that is Triton-specific begins with a `triton/` prefix.

Branches with Triton modifications are named `triton/<version>`, such as
`triton/11.0.x`. This is a branch that tracks the Grafana
`v11.0.x` branch. These branches will have all of our patches
rebased on top of them. Currently, this repository is consumed by
`triton-grafana`, which includes a submodule for this repository. The
submodule version will be based on a tag in this repository that uses the form
`triton/v<version>t<branch release num>`. Note that Grafana uses a branch per
secondary version number (e.g. `11.0.x`), and assigns multiple tags and releases
with tertiary version numbers (e.g. `11.0.1`) from each branch. Thus, assuming
a given Triton release were based on version `11.0.1`, the release tag would be:
`triton/v11.0.1t1`. If we need to cut another release
from this upstream release, we would tag it `triton/v11.0.1t2` and continue to
increment the number after the `t`. Note we use the `t` instead of `r`
which would more traditionally be used to indicate a revision.  We use
`t` in case Grafana for some reason wants to use `r` in its version strings.

When it comes time to update to a newer version of Grafana, we would take
the following steps:

* Ensure that we have pushed all changes from `grafana/grafana` and synced
  all of our branches and tags.
* Identify the release tag that corresponds to the point release. For
  this example, we'll say that's `v11.0.1`.
* Create a new branch named `triton/<version>` from the tag. In this
  case we would name the branch `triton/11.0.x` to match Grafana's naming
  scheme.
* Rebase all of our patches on to that new branch, removing any patches
  that are no longer necessary.
* Test the new version of Grafana.
* Review and Commit all relevant changes.
* Create a new tag `triton/v11.0.1t1`.
* Update [triton-grafana](https://github.com/TritonDataCenter/triton-grafana) to
  point to the new tag.

![Grafana Logo (Light)](docs/logo-horizontal.png#gh-light-mode-only)
![Grafana Logo (Dark)](docs/logo-horizontal-dark.png#gh-dark-mode-only)

The open-source platform for monitoring and observability

[![License](https://img.shields.io/github/license/grafana/grafana)](LICENSE)
[![Drone](https://drone.grafana.net/api/badges/grafana/grafana/status.svg)](https://drone.grafana.net/grafana/grafana)
[![Go Report Card](https://goreportcard.com/badge/github.com/grafana/grafana)](https://goreportcard.com/report/github.com/grafana/grafana)

Grafana allows you to query, visualize, alert on and understand your metrics no matter where they are stored. Create, explore, and share dashboards with your team and foster a data-driven culture:

- **Visualizations:** Fast and flexible client side graphs with a multitude of options. Panel plugins offer many different ways to visualize metrics and logs.
- **Dynamic Dashboards:** Create dynamic & reusable dashboards with template variables that appear as dropdowns at the top of the dashboard.
- **Explore Metrics:** Explore your data through ad-hoc queries and dynamic drilldown. Split view and compare different time ranges, queries and data sources side by side.
- **Explore Logs:** Experience the magic of switching from metrics to logs with preserved label filters. Quickly search through all your logs or streaming them live.
- **Alerting:** Visually define alert rules for your most important metrics. Grafana will continuously evaluate and send notifications to systems like Slack, PagerDuty, VictorOps, OpsGenie.
- **Mixed Data Sources:** Mix different data sources in the same graph! You can specify a data source on a per-query basis. This works for even custom datasources.

## Get started

- [Get Grafana](https://grafana.com/get)
- [Installation guides](https://grafana.com/docs/grafana/latest/setup-grafana/installation/)

Unsure if Grafana is for you? Watch Grafana in action on [play.grafana.org](https://play.grafana.org/)!

## Documentation

The Grafana documentation is available at [grafana.com/docs](https://grafana.com/docs/).

## Contributing

If you're interested in contributing to the Grafana project:

- Start by reading the [Contributing guide](https://github.com/grafana/grafana/blob/HEAD/CONTRIBUTING.md).
- Learn how to set up your local environment, in our [Developer guide](https://github.com/grafana/grafana/blob/HEAD/contribute/developer-guide.md).
- Explore our [beginner-friendly issues](https://github.com/grafana/grafana/issues?q=is%3Aopen+is%3Aissue+label%3A%22beginner+friendly%22).
- Look through our [style guide and Storybook](https://developers.grafana.com/ui/latest/index.html).

## Get involved

- Follow [@grafana on Twitter](https://twitter.com/grafana/).
- Read and subscribe to the [Grafana blog](https://grafana.com/blog/).
- If you have a specific question, check out our [discussion forums](https://community.grafana.com/).
- For general discussions, join us on the [official Slack](https://slack.grafana.com) team.

This project is tested with [BrowserStack](https://www.browserstack.com/)

## License

Grafana is distributed under [AGPL-3.0-only](LICENSE). For Apache-2.0 exceptions, see [LICENSING.md](https://github.com/grafana/grafana/blob/HEAD/LICENSING.md).
