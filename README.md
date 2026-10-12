# Case gitops

## Deploy automatico domini (ANC / PGCC)

Il bootstrap Argo CD in [argocd/bootstrap](./argocd/bootstrap) include un `ApplicationSet` che genera automaticamente 4 applicazioni:

- `case-platform-anc-backend`
- `case-platform-anc-frontend`
- `case-platform-pgcc-backend`
- `case-platform-pgcc-frontend`

Tutte le applicazioni usano:

- `targetRevision: develop`
- `syncPolicy.automated.prune=true`
- `syncPolicy.automated.selfHeal=true`

Quindi ogni push su `develop` del repository GitOps viene rilevato da Argo CD e avvia il deploy dei componenti coinvolti.

Per i backend dominio ANC/PGCC è configurato anche Argo CD Image Updater con strategia `digest` sul tag `latest`: quando GHCR aggiorna `latest`, Image Updater aggiorna i values Helm e innesca il nuovo sync.
Le immagini backend attese sono:

- `ghcr.io/landrimarina/case-platform-anc-backend:latest`
- `ghcr.io/landrimarina/case-platform-pgcc-backend:latest`

Nomenclatura immagini dominio allineata:

- backend: `case-platform-<dominio>-backend`
- frontend MFE: `case-platform-<dominio>-mfe`

Immagini frontend attese:

- `ghcr.io/landrimarina/case-platform-anc-mfe`
- `ghcr.io/landrimarina/case-platform-pgcc-mfe`

### Configurazione values per dominio

- ANC backend: [applications/domains/anc/backend/values.yaml](./applications/domains/anc/backend/values.yaml)
- ANC frontend: [applications/domains/anc/frontend/values.yaml](./applications/domains/anc/frontend/values.yaml)
- PGCC backend: [applications/domains/pgcc/backend/values.yaml](./applications/domains/pgcc/backend/values.yaml)
- PGCC frontend: [applications/domains/pgcc/frontend/values.yaml](./applications/domains/pgcc/frontend/values.yaml)