### For Mac - Install Argo CLI 
brew install argocd

### Install Argo CD to the cluster
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

### The password is auto-generated, we can get it with:
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
passwd:TbCIanpAT5XR6CyT  

### Accessing the Argo CD Web UI
kubectl port-forward svc/argocd-server -n argocd 8080:443

kubectl get po -n argocd
kubectl get svc -n argocd
kubectl get ing -n argocd

#### or use this method after installing argocd to retrieve paswd and log in 
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}' > /dev/null 2>&1 
export argo_url=$(kubectl get svc -n argocd | grep argocd-server | awk '{print $4}' | grep -v none)
echo "argo_url: http://$argo_url/"
echo username: "admin"
echo password: $(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d)

### To log into argocd thriugh command line to connect it with the ui so you can deploy from git with argo from CL instead of ui
argocd login 127.0.0.1:8080 --username admin --password TbCIanpAT5XR6CyT --insecure