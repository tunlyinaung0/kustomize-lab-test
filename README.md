# Kustomize Lab Test


## Folder Structure


```bash
kustomize-lab-test
├── README.md
├── base
│   ├── db
│   │   ├── db-depl.yaml
│   │   ├── db-svc.yaml
│   │   ├── kustomization.yaml
│   │   └── sealed-db-secret.yaml
│   ├── kustomization.yaml
│   └── web
│       ├── kustomization.yaml
│       ├── web-depl.yaml
│       └── web-svc.yaml
└── overlays
    ├── dev
    │   ├── db-depl-patch.yaml
    │   ├── dev-ns.yaml
    │   └── kustomization.yaml
    ├── prod
    │   ├── db-depl-patch.yaml
    │   ├── kustomization.yaml
    │   ├── prod-ns.yaml
    │   └── web-depl-patch.yaml
    └── uat
        ├── db-depl-patch.yaml
        ├── kustomization.yaml
        ├── uat-ns.yaml
        └── web-depl-patch.yaml
```


## Creating cluster wide sealed secret step by step

Firstly, create a database secret.

```bash
kubectl create secret generic db-secret --from-literal=MYSQL_ROOT_PASSWORD=password --dry-run=client -oyaml > db-secret.yaml
```

Encrypt the kubernetes encoded secret using kubeseal.



## What if your didn't create your sealed secret without " -- scope cluster-wide"

The answer is you will be  facing "no key could decrypt secret" error since your password can be decrypted only for default namespace.

One of the solution that I can think of is that you can create your sealed secret across multiple environments. 

Another solution is to add "--scope cluster-wide" when you seal your secrets. 





## Running different environments

```bash
cd kustomize-lab-test
```

Run for dev environment.

```bash
kustomize build overlays/dev | kubectl apply -f -
```
Or you can run :

```bash
kustomize build overlays/dev
kubectl apply -k overlays/dev
```

Also the same steps for other environments. 

```bash
kubeseal --format yaml --scope cluster-wide < db-secret.yaml > sealed-db-secret.yaml
```


