# Locked down Nginx

tl;dr:
 * Roles define permission(s)
 * Service Accounts are used by deployments/pods/etc
 * RoleBindings binds a role to a Service Account

## Create a namespace and service account

```
$ kubectl create ns lockdown
$ kubectl create serviceaccount --namespace lockdown sa-lockdown
```

## Create the role and rolebinding

```
$ kubectl create -f role.yaml
$ kubectl create -f rolebinding.yaml
```

## Verify the result

```
$ kubectl auth can-i get pods --namespace lockdown --as system:serviceaccount:lockdown:sa-lockdown
no
```

## Create the deployment

```
$ kubectl create -f deployment.yaml
```

