# ArgoCD
The following instruction is specific for VDI EUCGroup01126. _(Then step 2 and step 9 ONLY)_

Follow this [Getting Started](https://github.com/kent-cheung-usps/ArgoCD/wiki/03-Getting-Started) to install Argo CD.

### Jump Started
1. Launch the Docker engine
2. Start Minikube cluster
   ```
   minikube start --memory=4096 --cpus=2 --kubernetes-version=1.33.1 --driver=docker --profile argocd-cluster
   ```
3. Health Check
   ```
   kubectl get pod -n argocd
   ```
4. Port Forward for terminal access (svc/argocd-server)
   ```
   kubectl -n argocd port-forward svc/argocd-server 2746:80 &
   ```
6. Export default admin password to Env variable
   ```
   ARGOCDPASSWORD=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo)
   ```
7. Login to argoCD
   ```
   argocd login 127.0.0.1:2746 --username admin --password $ARGOCDPASSWORD
   ```
8. Verification
   `argocd account get-user-info`
   `argocd context `
   `argocd app get usps-api-sample`

9. Launch the usps_api_sample App
   - Port forward to access the UI app locally
     ```
     kubectl port-forward svc/usps-api-sample 8080:80 -n default
     ```
   - Allow access from other machine **(Not Recommanded due to security concern.)**
     ```
     kubectl port-forward --address 0.0.0.0 svc/usps-api-sample 8080:80 -n default
     ```
   - Navigate to: http://localhost:8080
     
   
