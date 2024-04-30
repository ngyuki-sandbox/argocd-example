# argocd

## quick start

```sh
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

kubectl create namespace argocd

helm install -n argocd argocd argo/argo-cd --version 5.46.7 -f argocd.values.yaml

kubectl get pods -n argocd -w
kubectl get service -n argocd

kubectl port-forward service/argocd-server -n argocd 8080:443

open http://localhost:8080
```

```sh
curl http://localhost:8080/api/webhook -H content-type:application/json -H X-GitHub-Event:push \
  -d '{"ref":"refs/heads/main","repository":{"html_url":"https://github.com/ngyuki-sandbox/argocd-example"}}'
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

> https://argocd-notifications.readthedocs.io/en/stable/

```sh
cp apps/argocd-notifications/argocd-notifications-secret.yaml.example apps/argocd-notifications/argocd-notifications-secret.yaml
vim apps/argocd-notifications/argocd-notifications-secret.yaml

kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj-labs/argocd-notifications/release-1.0/manifests/install.yaml
kubectl apply -n argocd -f apps/argocd-notifications/argocd-notifications-secret.yaml
kubectl apply -n argocd -f apps/argocd-notifications/argocd-notifications-cm.yaml

kubectl get pods -n argocd -w
```
