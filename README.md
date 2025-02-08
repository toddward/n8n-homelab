# n8n-homelab

1. Install the chart either from remote or local to this repo.

    ```bash
    helm repo add community-charts https://community-charts.github.io/helm-charts
    
    helm repo update

    kc create ns n8n

    helm install -g --namespace n8n community-charts/n8n
    ```