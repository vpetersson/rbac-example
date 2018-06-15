# Locked down Nginx

tl;dr:
 * Roles define permission(s)
 * Service Accounts are used by deployments/pods/etc
 * RoleBindings binds a role to a Service Account

## Create a namespace and service account

```
$ kubectl create ns lockdown-secrets
$ kubectl create serviceaccount --namespace lockdown-secrets sa-lockdown
```

## Create the role and rolebinding

```
$ kubectl create -f role.yaml
$ kubectl create -f rolebinding.yaml
```

## Verify the result

```
$ kubectl auth can-i get pods --namespace lockdown-secrets --as system:serviceaccount:lockdown-secrets:sa-lockdown
no
$ kubectl auth can-i get pods --namespace lockdown-secrets --as system:serviceaccount:lockdown-secrets:sa-lockdown
yes
```

## Create the deployment

```
$ kubectl create -f secrets.yaml
$ kubectl create -f deployment.yaml
```

## Verify that the pod can access the credentials

```
$ kubectl exec -ti nginx-deployment-[...] -n lockdown-secrets env  | grep SECRET
SECRET_USERNAME=admin
SECRET_PASSWORD=1f2d1e2e67df
```
