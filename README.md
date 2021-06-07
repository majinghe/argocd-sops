# argocd-sops
Argocd installation with sops, then can using **kustomize** to manage cloud native yaml file, then all the application can be managed by Argocd.

## Argocd Installation


### Step 1: Create argocd ns

```
$ kubectl create ns argocd
```

### Step 2: Generating the gpg key

Running the `gpg --full-generate-key` command to generate gpg key.

### Step 3: Export the gpg private key

Running the `gpg --armor --export-secret-keys <your gpg identity key id>` command to export the gpg secret key and save to `deploy-key.asc` file

```
$ gpg --armor --export-secret-keys <your gpg identity key id > deploy-key.asc
```
### Step 4: Create gpg secret

```
$ kubectl -n argocd create secret generic deploy-pgp-key --from-file=deploy-key.asc
```

### Step 5: Install argocd
```
$ kubectl -n argocd apply -f install.yaml
```

Then checking the pods
```
$ kubectl -n argocd get pods
NAME                                  READY   STATUS    RESTARTS   AGE
argocd-application-controller-0       1/1     Running   0          5h3m
argocd-dex-server-5dd657bd9-7k6dg     1/1     Running   0          5h3m
argocd-redis-759b6bc7f4-vtv76         1/1     Running   0          5h3m
argocd-repo-server-86d495c7c4-9rb2v   1/1     Running   0          5h3m
argocd-server-859b4b5578-4t5m2        1/1     Running   0          5h3m
```

### Step 6: Setup ingress
```
$ kubectl -n argocd apply -f ingress.yaml
```

Then you can get the ingress.
```
$ kubectl -n argocd get ing
NAME             CLASS    HOSTS             ADDRESS          PORTS     AGE
argocd-ingress   <none>   your.devops.com   x.x.x.x          80, 443   25h
```

Login url is 
`https://your.devops.com`

