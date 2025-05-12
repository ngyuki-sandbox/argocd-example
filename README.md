# argocd

## quick start

```sh
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

kind create cluster
kubectl create namespace argocd

helm install -n argocd argocd argo/argo-cd --version 5.46.7 -f argocd.values.yaml

kubectl get pods -n argocd -w
kubectl get service -n argocd

kubectl port-forward service/argocd-server -n argocd 8080:443

open http://localhost:8080
```

## argocd-apps

```sh
kubectl apply -f argocd-apps/argocd-apps.yaml
```

## myapp

```sh
kubectl -n myapp exec svc/app -c nginx -- curl -s http://localhost
```

## argocd-hooks

> https://argo-cd.readthedocs.io/en/stable/user-guide/resource_hooks/

```sh
cp apps/argocd-hooks/secrets.yaml.example apps/argocd-hooks/secrets.yaml
vim apps/argocd-hooks/secrets.yaml

kubectl apply -n argocd-hooks -f apps/argocd-hooks/secrets.yaml
```

## argocd-notifications

> https://argo-cd.readthedocs.io/en/stable/operator-manual/notifications/

```sh
kubectl -n argocd apply -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/notifications_catalog/install.yaml
kubectl -n argocd get pods -w

cp argocd-notifications/argocd-notifications-secret.yaml.example argocd-notifications/argocd-notifications-secret.yaml
vim argocd-notifications/argocd-notifications-secret.yaml
kubectl -n argocd apply -f argocd-notifications/argocd-notifications-secret.yaml
kubectl -n argocd apply -f argocd-notifications/argocd-notifications-cm.yaml

kubectl -n argocd get cm argocd-notifications-cm -o yaml > argocd-notifications/orig.yaml
```
