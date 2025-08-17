# Deploy ArgoCD and External Secrets Operator



## Deploy ArgoCD
```sh
ARGOCD_VERSION="v3.0.11"
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/${ARGOCD_VERSION}/manifests/install.yaml
```

## Deploy External Secrets Operator

```sh
helm repo add external-secrets https://charts.external-secrets.io

helm install external-secrets \
   external-secrets/external-secrets \
  -n external-secrets \
  --create-namespace
```

## Verify

```sh
$ kubectl get po -n argocd -n external-secrets
NAME                                                READY   STATUS    RESTARTS   AGE
external-secrets-665666979-ph7sq                    1/1     Running   0          110s
external-secrets-cert-controller-64db5d9cf6-5bw7v   1/1     Running   0          110s
external-secrets-webhook-8645f88b5b-r6dx9           1/1     Running   0          110s

$ k get po -n argocd                    
NAME                                               READY   STATUS    RESTARTS   AGE
argocd-application-controller-0                    1/1     Running   0          80s
argocd-applicationset-controller-6447d6698-7xsc4   1/1     Running   0          80s
argocd-dex-server-67ff89759d-mwrwp                 1/1     Running   0          80s
argocd-notifications-controller-7546fb6bd5-2g2tn   1/1     Running   0          80s
argocd-redis-87d469545-htc8h                       1/1     Running   0          80s
argocd-repo-server-58df7f59d9-sl5c5                1/1     Running   0          80s
argocd-server-57d9c7dd4d-4c9x6                     1/1     Running   0          80s
```

## Deploy helm chart from private ECR

The contents of `application.yaml` file are in this repository. Adjust the repositoryURL as needed.

```sh
$ kubectl apply -f application.yaml
```

At this point, it will fail with an error saying that credentials cannot be found.

## Generate ArgoCD repo secret with ESO

```sh
# in my case, I am simply using static AWS credentials
$ kubectl create secret generic ecr-access --from-literal=access_key=CHANGEME --from-literal=secret_access_key=CHANGEME -n argocd
```

```sh
$ kubectl apply -f repository-externalsecret.yaml
```

You should see ArgoCD successfully pulling the chart and deploying the application.