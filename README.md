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

