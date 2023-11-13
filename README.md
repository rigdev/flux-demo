# flux-demo

This repository serves as a quick demo of how rig can be installed in a flux
setup.

In order to be able to demo how to setup `Ingress` support, we create a simple
self-signed `ClusterIssuer`.

To read more about GitOps in conjunction with rig, please see [the
documentation](https://docs.rig.dev/operator-manual/gitops).

## Running the demo in kind

The demo can be run locally in a kind cluster using the following
instructions.

Install kind using the official installation instructions:
https://kind.sigs.k8s.io/#installation-and-usage

Start a kind cluster:

```bash
kind create cluster -n rig-demo
```

Install the Flux CLI using the official instructions:
https://fluxcd.io/flux/installation/#install-the-flux-cli

Fork this repository on your personal GitHub account and export your GitHub
access token, username and repo name:

```bash
export GITHUB_TOKEN="<your-token>"
export GITHUB_USER="<your-username>"
export GITHUB_REPO="<repository-name>"
```

Bootstrap flux to point to your fork:

```bash
flux bootstrap github \
    --context=kind-rig-demo \
    --owner=${GITHUB_USER} \
    --repository=${GITHUB_REPO} \
    --branch=main \
    --personal \
    --path=clusters/demo
```

Watch for helm releases being installed in the cluster:

```bash
watch flux get helmreleases -A
```

Initiate rig-platform:

```bash
kubectl exec -it -n rig-system deploy/rig-platform -- rig-admin init
```

Port-forward rig-platform:

```bash
kubectl port-forward -n rig-system svc/rig-platform 4747
```

Visit http://localhost:4747 in your browser to access the UI and login.
