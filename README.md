\# ArgoCD GitOps Demo 🚀



\## Architecture

```

Git Repository (Source of Truth)

&nbsp;        ↓

&nbsp;   ArgoCD (root-app)

&nbsp;        ↓

&nbsp;   ├── nginx-dev (3 replicas)

&nbsp;   ├── nginx-staging (3 replicas)

&nbsp;   └── nginx-prod (5 replicas + resource limits)

```



\## Structure

```

├── dev/apps/          # Environnement développement

├── staging/apps/      # Environnement pré-production

├── prod/apps/         # Environnement production

└── argocd-apps/       # Applications ArgoCD

&nbsp;   ├── nginx-dev.yaml

&nbsp;   ├── nginx-staging.yaml

&nbsp;   └── nginx-prod.yaml

```



\## Déploiement

```bash

\# Appliquer la root app (gère tout automatiquement)

kubectl apply -f argocd-apps/root-app.yaml

```



\## Accès ArgoCD

```bash

\# Port-forward

kubectl port-forward svc/argocd-server -n argocd 8080:443



\# Mot de passe

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

```



URL: https://localhost:8080

User: admin



\## Workflow GitOps



1\. Modifier un fichier YAML dans Git

2\. Commit + Push

3\. ArgoCD synchronise automatiquement (< 3 min)

4\. Kubernetes est mis à jour



\## Commandes utiles

```bash

\# Voir toutes les apps

kubectl get applications -n argocd



\# Voir les pods de tous les environnements

kubectl get pods -n dev

kubectl get pods -n staging

kubectl get pods -n prod



\# Forcer la synchro

kubectl patch application nginx-dev -n argocd -p '{"metadata":{"annotations":{"argocd.argoproj.io/refresh":"hard"}}}' --type merge

```

