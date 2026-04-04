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
  --from-literal=DB_USER=db_user \
  --from-literal=DB_PASS=db_password \
  --from-literal=DB_HOST=db_host \
  --from-literal=DB_NAME=db_name
```
keep secrets from being updated
```
kubectl annotate secret dispatcherhq-secrets \
  -n dev-dispatcherhq \
  argocd.argoproj.io/sync-options=Skip=true
```

### cloudflare-tunnel

create secret before deploying
```
kubectl create secret generic cloudflare-tunnel-credentials \
  -n cloudflare-tunnel \
  --from-literal=TUNNEL_ID='your-tunnel-uuid' \
  --from-literal=credentials.json='{"AccountTag":"ACCOUNT_TAG","TunnelID":"TUNNEL_ID","TunnelSecret":"TUNNEL_SECRET"}'
```
keep secret from being updated
```
kubectl annotate secret cloudflare-tunnel-credentials \
  -n cloudflare-tunnel \
  argocd.argoproj.io/sync-options=Skip=true
```