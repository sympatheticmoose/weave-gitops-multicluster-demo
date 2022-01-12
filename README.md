# weave-gitops-multicluster-demo
Example showing 4 local k8s clusters and a variety of deployments managed with Weave GitOps Core using this repository as the config repo.   
TODO: add GitLab equivalent

# Kubernetes clusters
- [Kind](https://kind.sigs.k8s.io/) - 2 versions running v1.23.1 and v1.22.4 (`kind create cluster --name <name> --image kindest/node:<tag>`)
- [Minikube](https://minikube.sigs.k8s.io/)
- [k3d](https://k3d.io/)
- [OpenShift](https://console.redhat.com/openshift/create/local) (CodeReady Containers)
  - Note for OpenShift, there are additional steps for install which require kubeadmin. Detailed [here](https://operatorhub.io/operator/flux).  
  TODO: write up WGC specifics

# Demo applications
- [podinfo](https://github.com/stefanprodan/podinfo/)
- [podinfo-deploy](https://github.com/wego-example/podinfo-deploy) (same app, repo has deployment artifacts only)
- [Loki](https://github.com/grafana/) (Helm chart)

# Application deployments by cluster
- Kind 1.23.1: podinfo-deploy (kustomize)
- Kind 1.22.4: podinfo-deploy (kustomize), Loki (helm)
- Minikube: podinfo (kustomize), Loki (helm)
- k3d: podinfo-deploy (kustomize)
- OpenShift: podinfo (kustomize)

# Try for yourself
1. Fork this repo (LPT: store the location of this repo with `export CONFIG_REPO='--config-repo=https://github.com/<username>/weave-gitops-multicluster-demo` for easier CLI actions)
2. Install [Weave GitOps Core](https://github.com/weaveworks/weave-gitops)
3. Run `gitops install --config-repo <path-to-your-fork-of-this-repo>`. If your cluster name matches the default for any of the above, the appropriate apps will be installed, if not - a new cluster entry will appear and you can run any of the following to add an application, note this shows a few different ways of adding an app including https/ssh url format:
- **podinfo-deploy**: `gitops add app --url https://github.com/wego-example/podinfo-deploy --config-repo <your-config-repo>`
- **podinfo**: `gitops add app --url https://github.com/stefanprodan/podinfo/ --path ./kustomize --config-repo <your-config-repo>`
- **sock**-**shop** (Kustomize): `gitops add app --url git@github.com:microservices-demo/microservices-demo.git --path ./deploy/kubernetes/manifests --config-repo <your-config-repo>`
- **sock**-**shop** (Helm): first create namespace - `kubectl create ns sock-shop` then `gitops add app --url ssh://git@github.com/microservices-demo/microservices-demo.git --path ./deploy/kubernetes/helm-chart --app-config-url <config-repo> --deployment-type helm --helm-release-target-namespace sock-shop
Adding application`


# Notes
- running multiple clusters on laptop is a bit resource hungry. Can stop/restart (kind at least) by doing `docker ps -a` to find the appropriate containers and then `docker start|stop`.
