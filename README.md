# kubectl-tail

`kubectl tail` is a [kubectl](https://kubernetes.io/docs/reference/kubectl/) plugin that tails pod logs using
[stern](https://github.com/stern/stern).

It wraps `stern` and exposes it as a familiar `kubectl` subcommand, so you can stream logs from every
matching pod in a namespace without leaving your kubectl workflow.

## Requirements

- `kubectl`
- [stern](https://github.com/stern/stern) installed and on your `PATH`
- Access to a Kubernetes cluster with a working kubeconfig

## Installation

### krew

This plugin is published in a personal [krew index](https://github.com/davidcollom/krew-index):

```bash
kubectl krew index add davidcollom https://github.com/davidcollom/krew-index
kubectl krew install kubectl-tail
```

### Manual

Download the latest release archive, unzip it, and place the `kubectl-tail` script on your `PATH`:

```bash
kubectl tail get pods
```

`kubectl` discovers plugins by looking for an executable named `kubectl-<plugin>` on your `PATH`, so the
script must keep its `kubectl-tail` name and be executable (`chmod +x kubectl-tail`).

## Usage

```bash
# Tail logs from all pods in the current namespace
kubectl tail

# Tail logs from a specific pod
kubectl tail pod/my-pod

# Tail logs from pods matching a label selector
kubectl tail -l app=my-app

# Follow logs across all namespaces
kubectl tail --all-namespaces
```

Any additional arguments are passed straight through to `stern`, so the full `stern` flag set is available.

## Configuration

| Environment variable      | Default | Description                                                        |
| ------------------------- | ------- | ------------------------------------------------------------------ |
| `KUBECTL_TAIL_START`      | `0`     | Number of lines of log history to show before streaming new lines. |

## How it works

The plugin is a thin wrapper that maps `kubectl tail ...` onto `stern`:

```bash
exec stern --tail=${KUBECTL_TAIL_START:-0} $@
```

## Releases

Releases are automated with GitHub Actions. Pushing a `v*.*.*` tag builds a `release.zip` archive and
publishes it to the [GitHub releases page](https://github.com/davidcollom/kubectl-tail/releases).

## License

See the repository for licensing details.
