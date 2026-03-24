I have provisioned a Google Kubernetes Engine cluster for you to use for learning/testing all things Ab Initio in Kubernetes within the str-22391 project (this is a dedicated project for internal-consulting).

You should all have viewer access to the entire project from the GCP online portal, and Kubernetes Developer access (essentially no restrictions inside the cluster) via the attached GCP service account key for authenticating with the cluster.

To authenticate, you will need to ensure that you have the gcloud cli installed then run the following:

gcloud auth login
export GOOGLE_APPLICATION_CREDENTIALS=<PATH-TO-KEY>/str-22391-gke.json
gcloud container clusters get-credentials icsg-cluster --zone asia-southeast1-a

Please note that the nodes inside the cluster are "spot" instances - this means they are significantly cheaper than regular instances, but they can be reclaimed by Google at any time (so you may notice that applications/jobs fail periodically).

Currently the project does not have a DNS entry configured, so you will need to access applications directly via the externally provisioned IP addresses for the nginx ingress controller / loadBalancer services.

There is also no automation setup to turn off applications running in the cluster, please ensure that you remove your resources when you are finished with them.

If you have any issues accessing the cluster, or any questions, please feel free reach out.

devcontainers@ABI-PW0GRDRW:/mnt/d/docs/training/k8s$ sudo snap install google-cloud-cli --classic  
2026-03-24T09:46:20+08:00 INFO Waiting for automatic snapd restart...
google-cloud-cli 561.0.0 from Cloud SDK (google-cloud-sdk✓) installed
devcontainers@ABI-PW0GRDRW:/mnt/d/docs/training/k8s$ gcloud --version 
Google Cloud SDK 561.0.0
alpha 2026.03.13
beta 2026.03.13
bq 2.1.29
bundled-python3-unix 3.13.10
core 2026.03.13
gcloud-crc32c 1.0.0
gsutil 5.36
minikube 1.38.1
skaffold 2.17.3
devcontainers@ABI-PW0GRDRW:/mnt/d/docs/training/k8s$ gcloud auth login
Your browser has been opened to visit:

    https://accounts.google.com/o/oauth2/auth?response_type=code&client_id=32555940559.apps.googleusercontent.com&redirect_uri=http%3A%2F%2Flocalhost%3A8085%2F&scope=openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fappengine.admin+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fsqlservice.login+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcompute+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Faccounts.reauth&state=wqgIe5BgMUTxkmMUjBXzwi9wams0fY&access_type=offline&code_challenge=d41Pv9ii-UW9XzRLYZALOlxhcKXP9h1a9eXtCgphdrU&code_challenge_method=S256

gio: https://accounts.google.com/o/oauth2/auth?response_type=code&client_id=32555940559.apps.googleusercontent.com&redirect_uri=http%3A%2F%2Flocalhost%3A8085%2F&scope=openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fappengine.admin+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fsqlservice.login+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcompute+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Faccounts.reauth&state=wqgIe5BgMUTxkmMUjBXzwi9wams0fY&access_type=offline&code_challenge=d41Pv9ii-UW9XzRLYZALOlxhcKXP9h1a9eXtCgphdrU&code_challenge_method=S256: Operation not supported

You are now logged in as [xxiang@abinitio.com].
Your current project is [None].  You can change this setting by running:
  $ gcloud config set project PROJECT_ID
devcontainers@ABI-PW0GRDRW:/mnt/d/docs/training/k8s$ gcloud config set project str-22391
[environment: untagged] Read more g.co/cloud/project-env-tag.
Updated property [core/project].