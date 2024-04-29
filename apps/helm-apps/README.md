# Helm で Application Set みたいなことをやる

```sh
kubectl apply -f argocd-apps/helm-apps.yaml
```

.Files.Glob でファイル一覧を得て range で Argo Application をループします。
.Files.Glob がファイルにしか聞かないため、ディレクトリごとに必ず存在する
適当なファイルを指定する必要があります（この例だと cm.yaml）。
