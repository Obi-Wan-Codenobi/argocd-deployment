# argocd-deployment


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