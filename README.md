# ArgoCD
## Deployment Instructions

To deploy the app using Argo CD:

1. Apply the Argo CD application manifest:
    ```bash
    kubectl apply -f apps/webapp/webapp.yaml -n argocd
    ```

2. Retrieve the ArgoCD server URL:
    ```bash
    minikube service argocd-server -n argocd --url
    ```

3. Retrieve the ArgoCD admin password:
    ```bash
    kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d && echo
    ```

4. Log in to ArgoCD using the CLI:
    ```bash
    argocd login <ArgoCD server URL without http://>
    ```
    - Use the admin password retrieved in the previous step.
    - If prompted with a warning about TLS, type `y` to proceed.

5. Verify the application status in the ArgoCD dashboard or using the CLI:
    ```bash
    argocd app get webapp
    ```

6. Sync the application to deploy it:
    ```bash
    argocd app sync webapp
    ```

7. Monitor the deployment progress and ensure all resources are healthy.

8. To access the deployed application:
    - Retrieve the NodePort:
        ```bash
        kubectl get svc webapp -n default -o jsonpath='{.spec.ports[0].nodePort}'
        ```
    - Retrieve the Minikube IP:
        ```bash
        minikube ip
        ```
    - Combine the Minikube IP and NodePort to access the application, e.g., `http://<minikube-ip>:<node-port>`.