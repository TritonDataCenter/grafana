# Grafana

This repository represents the version of
[Grafana](https://github.com/grafana/grafana) that is used as part of
[Joyent Triton](https://github.com/joyent/triton).

This fork removes PhantomJS from the build, because PhantomJS is not supported
on illumos.

## Repository Management

This repository is downstream of
[Grafana](https://github.com/grafana/grafana).

To better understand and maintain our differences from Grafana, we try to
manage branches and tags in a specific fashion. First and foremost, all
branches and tags from the upstream Grafana repository are mirrored here.

Anything that is Joyent-specific begins with a `joyent/` prefix.

Branches with Joyent modifications are named `joyent/<version>`, such as
`joyent/5.3.x`. This is a branch that tracks the Grafana
`v5.3.x` branch. These branches will have all of our patches
rebased on top of them. Currently, this repository is consumed by
`triton-grafana`, which includes a submodule for this repository. The
submodule version will be based on a tag in this repository that uses the form
`joyent/v<version>j<branch release num>`. Note that Grafana uses a branch per
secondary version number (e.g. `5.3.x`), and assigns multiple tags and releases
with tertiary version numbers (e.g. `5.3.2`) from each branch. Thus, assuming
a given Joyent release were based on version `5.3.2`, the release tag would be:
`joyent/v5.3.2j1`. If we need to cut another release
from this upstream release, we would tag it `joyent/v5.3.2j2` and continue to
increment the number after the `j`. Note we use the `j` instead of `r`
which would more traditionally be used to indicate a revision.  We use
`j` in case Grafana for some reason wants to use `r` in its version strings.

When it comes time to update to a newer version of Grafana, we would take
the following steps:

* Ensure that we have pushed all changes from `grafana/grafana` and synced
  all of our branches and tags.
  * Identify the release tag that corresponds to the point release. For
    this example, we'll say that's `v5.3.2`.
    * Create a new branch named `joyent/<version>` from the tag. In this
      case we would name the branch `joyent/5.3.x` to match Grafana's naming
        scheme.
	* Rebase all of our patches on to that new branch, removing any patches
	  that are no longer necessary.
	  * Test the new version of Grafana.
	  * Review and Commit all relevant changes.
	  * Create a new tag `joyent/v5.3.2j1`.
	  * Update [triton-grafana](https://github.com/joyent/triton-grafana) to
	    point to the new tag.

	    [Grafana](https://grafana.com) [![Circle CI](https://circleci.com/gh/grafana/grafana.svg?style=svg)](https://circleci.com/gh/grafana/grafana) [![Go Report Card](https://goreportcard.com/badge/github.com/grafana/grafana)](https://goreportcard.com/report/github.com/grafana/grafana) [![codecov](https://codecov.io/gh/grafana/grafana/branch/master/graph/badge.svg)](https://codecov.io/gh/grafana/grafana)
	    ================
	    [Website](https://grafana.com) |
	    [Twitter](https://twitter.com/grafana) |
	    [Community & Forum](https://community.grafana.com)

	    Grafana is an open source, feature rich metrics dashboard and graph editor for
	    Graphite, Elasticsearch, OpenTSDB, Prometheus and InfluxDB.

	    ![](http://docs.grafana.org/assets/img/features/dashboard_ex1.png)

## Installation
Head to [docs.grafana.org](http://docs.grafana.org/installation/) and [download](https://grafana.com/get)
the latest release.

If you have any problems please read the [troubleshooting guide](http://docs.grafana.org/installation/troubleshooting/).

## Documentation & Support
Be sure to read the [getting started guide](http://docs.grafana.org/guides/gettingstarted/) and the other feature guides.

## Run from master
If you want to build a package yourself, or contribute - Here is a guide for how to do that. You can always find
the latest master builds [here](https://grafana.com/grafana/download)

### Dependencies

- Go 1.11
- NodeJS LTS

### Building the backend
```bash
go get github.com/grafana/grafana
cd $GOPATH/src/github.com/grafana/grafana
go run build.go setup
go run build.go build
```

### Building frontend assets

For this you need nodejs (v.6+).

To build the assets, rebuild on file change, and serve them by Grafana's webserver (http://localhost:3000):
```bash
npm install -g yarn
yarn install --pure-lockfile
yarn watch
```

Build the assets, rebuild on file change with Hot Module Replacement (HMR), and serve them by webpack-dev-server (http://localhost:3333):
```bash
yarn start
# OR set a theme
env GRAFANA_THEME=light yarn start
```
Note: HMR for Angular is not supported. If you edit files in the Angular part of the app, the whole page will reload.

Run tests
```bash
yarn jest
```

### Recompile backend on source change

To rebuild on source change.
```bash
go get github.com/Unknwon/bra
bra run
```

Open grafana in your browser (default: `http://localhost:3000`) and login with admin user (default: `user/pass = admin/admin`).

### Building a docker image (on linux/amd64)

This builds a docker image from your local sources:

1. Build the frontend `go run build.go build-frontend`
2. Build the docker image `make build-docker-dev`

The resulting image will be tagged as `grafana/grafana:dev`

### Dev config

Create a custom.ini in the conf directory to override default configuration options.
You only need to add the options you want to override. Config files are applied in the order of:

1. grafana.ini
1. custom.ini

In your custom.ini uncomment (remove the leading `;`) sign. And set `app_mode = development`.

### Running tests

#### Frontend
Execute all frontend tests
```bash
yarn test
```

Writing & watching frontend tests

- Start watcher: `yarn jest`
- Jest will run all test files that end with the name ".test.ts"

#### Backend
```bash
# Run Golang tests using sqlite3 as database (default)
go test ./pkg/...

# Run Golang tests using mysql as database - convenient to use /docker/blocks/mysql_tests
GRAFANA_TEST_DB=mysql go test ./pkg/...

# Run Golang tests using postgres as database - convenient to use /docker/blocks/postgres_tests
GRAFANA_TEST_DB=postgres go test ./pkg/...
```

## Building custom docker image

You can build a custom image using Docker, which doesn't require installing any dependencies besides docker itself.
```bash
git clone https://github.com/grafana/grafana
cd grafana
docker build -t grafana:dev .
docker run -d --name=grafana -p 3000:3000 grafana:dev
```

Open grafana in your browser (default: `http://localhost:3000`) and login with admin user (default: `user/pass = admin/admin`).

## Contribute

If you have any idea for an improvement or found a bug, do not hesitate to open an issue.
And if you have time clone this repo and submit a pull request and help me make Grafana
the kickass metrics & devops dashboard we all dream about!

## Plugin development

Checkout the [Plugin Development Guide](http://docs.grafana.org/plugins/developing/development/) and checkout the [PLUGIN_DEV.md](https://github.com/grafana/grafana/blob/master/PLUGIN_DEV.md) file for changes in Grafana that relate to
plugin development.

## License

Grafana is distributed under Apache 2.0 License.
