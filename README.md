# gallery-frontend-react-cd

In this demo, I will show you how ArgoCD deploy manifests to EKS and How Canary Release works in a production environment.


## ArgoCD Usage

- project.yaml
- application.yaml


## Canary Release

- What's a Canary Release ?

Canary release is when you release a new version, you won't put 100% of new version to Kubernetes, then you can set some strages for example:

20% network traffic use the new verision -> canary version

80% network traffic use the old version -> stable version

5 minutes later

50% network traffic use the new verision -> canary version

50% network traffic use the old version  -> stable version

10 minutes later

100% network traffic use the new verision -> canary version

0%   network traffic use the old version  -> stable version


## Canary Release with Argo CD

Install kubectl-argo-rollouts on Mac
```shell
brew install argoproj/tap/kubectl-argo-rollouts

```

Install argo-rollouts on EKS
```shell
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml

```

Check the argo-rollouts pod status
```shell
kubectl get pods -n argo-rollouts
NAME                             READY   STATUS    RESTARTS   AGE
argo-rollouts-79d7dd7db7-c7jmf   1/1     Running   0          5h29m

```

Run the argo-rollout dashboard
```shell
kubectl argo rollouts dashboard

```

Open the URL on browser
```shell
http://localhost:3100/rollouts

```
Abort the canary release when you find new version has issues
```shell
kubectl argo rollouts abort frontend-react-rollout -n frontend-prod

```

Get rollout status 
```shell
kubectl argo rollouts get rollout frontend-react-rollout -n frontend-prod

```







