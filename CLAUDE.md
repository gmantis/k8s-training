# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Environment Overview

This is a Kubernetes/Helm training workspace for Ab Initio internal consulting. Work is done against a shared GKE cluster provisioned in the `str-22391` GCP project.

- **Cluster:** `icsg-cluster`
- **Zone:** `asia-southeast1-a`
- **Project:** `str-22391`
- **User:** `xxiang@abinitio.com`
- **Nodes:** Spot instances (can be reclaimed by Google at any time — expect intermittent failures)
- **DNS:** Not configured — access applications via external IP addresses of the nginx ingress controller / LoadBalancer services
- **Cleanup:** Always remove resources from the cluster when done (no automation turns things off)

## Cluster Authentication

```bash
gcloud auth login
export GOOGLE_APPLICATION_CREDENTIALS=/mnt/d/docs/training/k8s/str-22391-gke.json
gcloud container clusters get-credentials icsg-cluster --zone asia-southeast1-a
gcloud config set project str-22391
```

The service account key is at `str-22391-gke.json` (grants Kubernetes Developer access — essentially no restrictions inside the cluster).

## Common kubectl Commands

```bash
kubectl get nodes
kubectl get pods -A
kubectl get svc -A                  # Find external IPs for LoadBalancer services
kubectl config current-context      # Verify connected to correct cluster
```

## Key Constraints

- No DNS — use raw external IPs to reach deployed apps
- Spot nodes may disappear; pods/jobs may fail unexpectedly — this is normal
- Access to GCP portal is viewer-only; cluster access is via the service account key
