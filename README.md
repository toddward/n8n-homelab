# n8n-homelab

## Baseline

* Running on K3s Cluster
* Longhorn storage backing.
* Actual application available: https://n8n.io

## Home Lab Configuration
1. Install the chart either from remote or local to this repo.

    ```bash
    helm repo add community-charts https://community-charts.github.io/helm-charts
    
    helm repo update

    kc create ns n8n

    helm install --name n8n --namespace n8n community-charts/n8n
    ```

2. Set up application using port-forward to communicate with installed instance for testing.

    ```bash
    NAMESPACE: n8n
    STATUS: deployed
    REVISION: 1
    TEST SUITE: None
    NOTES:
        export POD_NAME=$(kubectl get pods --namespace n8n -l "app.kubernetes.io/name=n8n,app.kubernetes.io/instance=n8n" -o jsonpath="{.items[0].metadata.name}")
        
        export CONTAINER_PORT=$(kubectl get pod --namespace n8n $POD_NAME -o jsonpath="{.spec.containers[0].ports[0].containerPort}")
        
        echo "Visit http://127.0.0.1:8080 to use your application"
        
        kubectl --namespace n8n port-forward $POD_NAME 8080:$CONTAINER_PORT
    ```

3. Enable CloudFlare Tunnels to allow for seamless communication with the instance from across the web.  This is especially important for external applications to reach your solution for triggering, etc.

    a. Follow the logic outlined here: https://developers.cloudflare.com/cloudflare-one/tutorials/many-cfd-one-tunnel/

    b. After the tunnel has been created, create the Kubernetes secret that's associated with the project.

    c. Validate communication with the installed instance: https://n8n.wardzinski.dev