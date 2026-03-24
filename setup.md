# Environment Setup

Repeatable steps to get a working `kubectl` connection to the `icsg-cluster` GKE cluster.

---

## Prerequisites

- Ab Initio laptop/desktop running Ubuntu (WSL2 or native)
- Service account key file: `str-22391-gke.json` (placed in this directory)
- GCP project: `str-22391`
- Cluster: `icsg-cluster`, zone: `asia-southeast1-a`

---

## Step 1 — Install Google Cloud CLI

> Skip if `gcloud --version` already works.

```bash
sudo snap install google-cloud-cli --classic

# Verify
gcloud --version
```

---

## Step 2 — Install the GKE auth plugin

> The `gke-gcloud-auth-plugin` is required for `kubectl` to authenticate with GKE.
> `gcloud components install` does NOT work when gcloud was installed via snap — use apt instead.

```bash
# Add Google Cloud apt repo
sudo apt-get install -y apt-transport-https ca-certificates gnupg
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" \
  | sudo tee /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg \
  | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

# Install the plugin
sudo apt-get update && sudo apt-get install -y google-cloud-cli-gke-gcloud-auth-plugin

# Verify
gke-gcloud-auth-plugin --version
```

---

## Step 3 — Authenticate with Google

```bash
gcloud auth login
# Opens a browser — log in with your @abinitio.com account

gcloud config set project str-22391
```

---

## Step 4 — Point kubectl at the cluster

```bash
export GOOGLE_APPLICATION_CREDENTIALS=/mnt/d/docs/training/k8s/str-22391-gke.json
gcloud container clusters get-credentials icsg-cluster --zone asia-southeast1-a

# Verify
kubectl config current-context
# Expected: gke_str-22391_asia-southeast1-a_icsg-cluster

kubectl get nodes
```

---

## Step 5 — Create your personal namespace

> Use your name or initials. This avoids clashing with other users on the shared cluster.

```bash
kubectl create namespace yourname

# Set it as your default for the session
kubectl config set-context --current --namespace=yourname
```

---

## Every New Terminal Session

Steps 3 and 4 need to be repeated whenever you open a new terminal (the export and context are not persisted):

```bash
export GOOGLE_APPLICATION_CREDENTIALS=/mnt/d/docs/training/k8s/str-22391-gke.json
gcloud container clusters get-credentials icsg-cluster --zone asia-southeast1-a
kubectl config set-context --current --namespace=yourname
```

> To avoid typing this every time, add the `export` line to your `~/.bashrc`.

---

## Verify Everything Works

```bash
kubectl config current-context        # should show icsg-cluster context
kubectl get nodes                     # should list cluster nodes
kubectl get pods -A                   # should list all running pods
kubectl config view --minify          # shows current context config
```

---

## Cluster Details

| Property | Value |
|---|---|
| Project | `str-22391` |
| Cluster | `icsg-cluster` |
| Zone | `asia-southeast1-a` |
| User | `xxiang@abinitio.com` |
| Service account key | `str-22391-gke.json` (in this directory) |
| Node type | Spot instances (can be reclaimed by GCP at any time) |
| DNS | Not configured — use external IPs directly |

---

## Cleanup Reminder

There is no automation to turn off resources. Always clean up when done:

```bash
# Delete everything in your namespace
kubectl delete namespace yourname

# Or delete specific resources
kubectl delete -f your-file.yaml
```
