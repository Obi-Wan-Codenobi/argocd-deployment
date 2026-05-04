# argocd-deployment
Exposing argo server locally:
```
sudo kubectl port-forward --address 0.0.0.0 svc/argocd-server -n argocd 8090:80
```

## Manually defining secrets in cluster:
(The scuffed way)

### dev-api

create secret before deploying
```
kubectl create secret generic dispatcherhq-secrets \
  -n dev-dispatcherhq \
  --from-env-file=.env \
  --dry-run=client -o yaml | kubectl apply -f -
```

keep secrets from being updated
```
kubectl annotate secret dispatcherhq-secrets \
  -n dev-dispatcherhq \
  argocd.argoproj.io/sync-options=Skip=true
```

### cloudflare-tunnel

Create secret before deploying:
```
kubectl create secret generic cloudflare-tunnel-credentials \
  -n cloudflare-tunnel \
  --from-literal=TUNNEL_TOKEN="CF_TUNNEL_TOKEN" \
  --dry-run=client -o yaml | kubectl apply -f -
```

Keep secret from being updated by ArgoCD:
```
kubectl annotate secret cloudflare-tunnel-credentials \
  -n cloudflare-tunnel \
  argocd.argoproj.io/sync-options=Skip=true
```