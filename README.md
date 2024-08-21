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


```bash
kubeseal --format yaml --scope cluster-wide < db-secret.yaml > sealed-db-secret.yaml
```


## What if you try to create your sealed secret without " -- scope cluster-wide"

The answer is you will be  facing "no key could decrypt secret" error since your password can be decrypted only for namespace where you created your seal secret.

One of the solution that I can think of is that you can create your sealed secret across multiple environments. 

Another solution is to add "--scope cluster-wide" when you seal your secrets. 

<img width="1207" alt="Screenshot 2024-08-20 at 9 34 38 AM" src="https://github.com/user-attachments/assets/3f3e2022-498e-4f26-b23f-7d56ee39d6e6">




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

Run same steps for other environments. 



