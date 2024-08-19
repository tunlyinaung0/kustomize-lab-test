# Kustomize Lab Test


## Folder Structure


```bash
kustomize-lab-test
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
    │   ├── dev-ns.yaml
    │   └── kustomization.yaml
    ├── prod
    │   ├── db-depl-patch.yaml
    │   ├── kustomization.yaml
    │   ├── prod-ns.yaml
    │   └── web-depl-patch.yaml
    └── uat
        ├── kustomization.yaml
        └── uat-ns.yaml
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


