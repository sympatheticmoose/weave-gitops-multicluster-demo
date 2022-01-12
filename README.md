# weave-gitops-multicluster-demo
Example showing 4 local k8s clusters and a variety of deployments managed with Weave GitOps Core using this repository as the config repo.   

# Kubernetes clusters
- [Kind](https://kind.sigs.k8s.io/) - 2 versions running v1.23.1 and v1.22.4 (`kind create cluster --name <name> --image kindest/node:<tag>`)
- [Minikube](https://minikube.sigs.k8s.io/)
- [k3d](https://k3d.io/)

# Demo applications
- [podinfo](https://github.com/stefanprodan/podinfo/)
- [podinfo-deploy](https://github.com/wego-example/podinfo-deploy) (same app, repo has deployment artifacts only)
- [Loki](https://github.com/grafana/) (Helm chart)

# Application deployments by cluster
- Kind 1.23.1: podinfo-deploy (kustomize)
- Kind 1.22.4: podinfo-deploy (kustomize), Loki (helm)
- Minikube: podinfo (kustomize), Loki (helm)
- k3d: podinfo-deploy (kustomize)

# Try for yourself
1. Install [Weave GitOps Core CLI](https://github.com/weaveworks/weave-gitops)
2. Run `gitops install --config-repo <path-to-your-config-repo>`.
3. Add apps from your cluster(s):
- **podinfo-deploy** - https://github.com/wego-example/podinfo-deploy: `gitops add app --url https://github.com/<username>/podinfo-deploy --config-repo <config-repo>`
- **podinfo** - https://github.com/stefanprodan/podinfo/: `gitops add app --url https://github.com/<username>/podinfo/ --path ./kustomize --config-repo <config-repo>`
- **loki** - https://github.com/grafana/loki/: `gitops add app --url https://grafana.github.io/helm-charts --chart loki-stack --deployment-type helm --config-repo <config-repo>`


# Notes
- can save time by exporting your config repo as `export CONFIG_REPO=<"url of config repo">` then just adding `$CONFIG_REPO` into CLI calls.
- running multiple clusters on laptop is a bit resource hungry. Can stop/restart (kind at least) by doing `docker ps -a` to find the appropriate containers and then `docker start|stop` and will restore cluster state.
