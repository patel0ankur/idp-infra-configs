# IDP Infra Configs

GitOps repository for self-service AWS infrastructure provisioning via ACK (AWS Controllers for Kubernetes).

ArgoCD watches this repository and automatically syncs changes to the EKS cluster. ACK controllers translate Kubernetes CRs into real AWS resources.

## How It Works

```
Developer requests resource in Backstage
    → Backstage template generates ACK CR YAML
    → PR opened in this repo (teams/<team>/infra/)
    → Platform team reviews and merges PR
    → ArgoCD ApplicationSet detects new/changed files
    → ArgoCD syncs to cluster
    → ACK creates the AWS resource (S3, RDS, SQS, etc.)
```

## Repository Structure

```
idp-infra-configs/
├── platform/
│   └── applicationset.yaml    ← ArgoCD ApplicationSet (manages all team apps)
└── teams/
    ├── team-a/
    │   └── infra/
    │       ├── s3-bucket.yaml         ← ACK S3 Bucket CR
    │       └── rds-instance.yaml      ← ACK RDS Instance CR
    └── team-b/
        └── infra/
            └── s3-bucket.yaml
```

## Adding Resources

Resources are added automatically by Backstage Scaffolder templates. Developers should NOT manually edit files in `teams/` — use the Backstage portal instead.

## ArgoCD ApplicationSet

One ApplicationSet (`platform/applicationset.yaml`) automatically creates an ArgoCD Application for each directory under `teams/`. New teams get their own ArgoCD Application automatically when the first PR for that team is merged.
