# yopass-chart

A lightweight Helm chart for [Yopass](https://github.com/jhaals/yopass) that prioritizes
transparency and flexibility.

Linted with [`kube-score`](https://github.com/zegl/kube-score),
[`kube-linter`](https://github.com/stackrox/kube-linter)
and [`yamllint`](https://github.com/stackrox/kube-linter).
Tested with [`helm-unittest`](https://github.com/helm-unittest/helm-unittest).

## Design Philosophy

### Transparency and Flexibility over Abstraction and Convenience

This chart may diverge from some common chart conventions.
It favors transparency and flexibility over abstraction and convenience:

- **Values-Driven:** The goal is to provide a chart that can be understood just by looking at the values file.
  There should be no need to look into the templates to understand what the chart does and how it works.
- **Minimal Abstraction:** The chart avoids hidden logic and magic behavior as much as possible.
  A lot of what you see in the values file is what gets rendered in the templates.
- **Flexibility over Convenience:** The chart is designed to be flexible and understandable.
  But this comes at the cost of convenience. Often you'll have to change multiple values to achieve the desired outcome.
  For example, if you want to change the ports of the application,
  you'd have to adapt the environment at `.Values.app.env`, as well as `.Values.containerPorts`.

### No Dependencies

This chart does not bundle any databases (e.g., Postgres, Valkey) as a dependency.
While this may seem inconvenient, it keeps the chart lightweight, and ensures users actively design their setup.
Which database should be used? Is one already deployed? Should a new one be deployed? Is it deployed via helm chart or
operator? Which helm chart should be used? Is a managed database service used?
These are all questions that should be answered by the user, not the chart.

## Getting Started

### Deploy the Helm Chart

If the default values in the [`values.yaml`](./values.yaml) fit your needs,
you can deploy the helm chart using this command:

```shell
helm install yopass oci://ghcr.io/ernail/charts/yopass \
--namespace yopass \
--create-namespace
```

### Configure the Helm Chart

Helm provides different ways to [configure helm charts via values](https://helm.sh/docs/helm/helm_install/#synopsis).
A common way is to create your own values file,
which overrides values of the charts default [`values.yaml`](./values.yaml):

```shell
helm install yopass oci://ghcr.io/ernail/charts/yopass \
--namespace yopass \
--create-namespace \
--values values-base.yaml
```

You can also pass in multiple values files. For example if you need seperate configuration for your `dev` environment:

```shell
helm install yopass oci://ghcr.io/ernail/charts/yopass \
--namespace yopass \
--create-namespace \
--values values-base.yaml \
--values values-dev.yaml
```

All configuration options are documented in the [`values.yaml`](./chart/values.yaml).

### Key Configuration Options

An example config is available in the [`values.yaml`](./chart/values.yaml).
Example deployments are available in the [`examples`](./examples) directory.

#### Database

The chart does not bundle any database, so you need to provide your own.
Check the Yopass documentation for the supported databases.
You can configure the database connection via the `app.env` values.

#### Miscellaneous

Other important configuration options that should be reviewed are:

- `ingress` - The ingress configuration
- `metrics` - The metrics configuration
- `resources` - The resource requests and limits

## Contributing

Please check the [`CONTRIBUTING.md`](./CONTRIBUTING.md) to learn how to contribute.

## Development

### Installing dependencies

You can install all required dependencies via `Task` and `Homebrew`.

```shell
brew install go-task
task install
```

If you'd like to use other tools,
you can find all dependencies and relevant commands in the [`taskfile.yaml`](./taskfile.yaml)

### Running Tasks

You can find all available tasks in the [`taskfile.yaml`](./taskfile.yaml).
