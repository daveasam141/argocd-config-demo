### For Mac - Install Argo CLI 
brew install argocd

### Install Argo CD to the cluster
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

### The password is auto-generated, we can get it with:
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
passwd:EmkI3eEe-bMdkQw6

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
argocd login 127.0.0.1:8080 --username admin --password EmkI3eEe-bMdkQw6 --insecure

###  To sync an argocd deployment using cli 
argocd --port-forward --port-forward-namespace=argocd app sync (appname)

### To log into argocd thriugh command line to connect it with the ui so you can deploy from git with argo from CL instead of ui
argocd login 127.0.0.1:8080 --username admin --password UbtbfXjlatGMJ7TX --insecure

### How to  deploy an app with argocd
argocd --port-forward --port-forward-namespace=argocd app create homesite --repo https://github.com/daveasam141/argocd-config-demo.git --path homesite --dest-server https://kubernetes.default.svc --dest-namespace argocd

### If your're getting a connection refused error with argocd include port forward in every argocd command 
argocd --port-forward --port-forward-namespace=argocd login --username=admin --password=EmkI3eEe-bMdkQw6 

### Apps deployed with argocd can be deleted with cascade and without it. (cascade removes all the resources related to the application as well)
To perform a non cascade remove: argocd app delete APPNAME --cascade=false

### To delete using cascade 
argocd app delete APPNAME --cascade OR argocd app delete APPNAME
argocd --port-forward --port-forward-namespace=argocd app delete homesite 

### To get argocd app 
argocd --port-forward --port-forward-namespace=argocd app get homesite 

### How to  deploy an app with argocd
argocd --port-forward --port-forward-namespace=argocd app create homesite --repo https://github.com/daveasam141/argocd-config-demo.git --revision argocdbranch --path homesite --dest-server https://kubernetes.default.svc --dest-namespace argocd
argocd  app create homesite --repo https://github.com/daveasam141/argocd-config-demo.git --path argocdbranch/homesite --dest-server https://kubernetes.default.svc --dest-namespace argocd

### If your're getting a connection refused error with argocd include port forward in every argocd command 
argocd --port-forward --port-forward-namespace=argocd login --username=admin --password=UbtbfXjlatGMJ7TX

### References 
https://argo-cd.readthedocs.io/en/latest/user-guide/app_deletion/
