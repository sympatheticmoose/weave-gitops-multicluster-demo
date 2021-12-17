# weave-gitops-multicluster-demo
Example showing 4 local k8s clusters and a variety of deployments managed with Weave GitOps Core using this repository as the config repo.   
TODO: add GitLab equivalent

# Kubernetes clusters
- [Kind](https://kind.sigs.k8s.io/)
- [Minikube](https://minikube.sigs.k8s.io/)
- [k3d](https://k3d.io/)
- [OpenShift](https://console.redhat.com/openshift/create/local) (CodeReady Containers)
  - Note for OpenShift, there are additional steps for install which require kubeadmin. Detailed [here](https://operatorhub.io/operator/flux).  
  TODO: write up WGC specifics

# Demo applications
- podinfo
- podinfo-deploy (repo with deployment artifacts only)
- sock-shop (Kustomize deployment
- sock-shop (Helm chart deployment)

# Application deployments by cluster
- Kind: podinfo, Sock-Shop (Helm)
- Minikube: podinfo-deploy, Sock-Shop (kustomize)
- k3d: podinfo-deploy
- OpenShift: podinfo-deploy, Sock-Shop (Helm)

# Try for yourself
1. Fork this repo
2. Install [Weave GitOps Core](https://github.com/weaveworks/weave-gitops)
3. Run `gitops install --config-repo <path-to-your-fork-of-this-repo>`. If your cluster name matches the default for any of the above, the appropriate apps will be installed, if not - a new cluster entry will appear and you can run any of the following to add an application, note this shows a few different ways of adding an app including https/ssh url format:
- **podinfo-deploy**: `gitops add app --url https://github.com/wego-example/podinfo-deploy --config-repo <your-config-repo>`
- **podinfo**: `gitops add app --url https://github.com/stefanprodan/podinfo/ --path ./kustomize --config-repo <your-config-repo>`
- **sock**-**shop** (Kustomize): `gitops add app --url git@github.com:microservices-demo/microservices-demo.git --path ./deploy/kubernetes/manifests --config-repo <your-config-repo>`
- **sock**-**shop** (Helm): first create namespace - `kubectl create ns sock-shop` then `gitops add app --url ssh://git@github.com/microservices-demo/microservices-demo.git --path ./deploy/kubernetes/helm-chart --app-config-url <config-repo> --deployment-type helm --helm-release-target-namespace sock-shop
Adding application
