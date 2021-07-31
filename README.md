## Suse Cloud Foundation

* https://cloud.ibm.com/docs/blockchain-sw?topic=blockchain-sw-Removing-k8
* https://hub.docker.com/u/pixelpotato
* https://k3s.io/
* https://www.vagrantup.com/docs/synced-folders/basic_usage
* https://www.vagrantup.com/docs/provisioning/file

````
cd Downloads
cd nd064_course_1
cd exercises
cd go.helloworld

````
docker login
ll
vi main.go
vi dockerfile
**Edit the dockerfile on course folder to add "Run go mod init" just before "Run go mod init"
:qa
docker build -t go-helloworld .
docker images
docker run -d -p 6111:6111 go-helloworld
docker ps
**see the helloworld on web 127.0.0.1:6111
**create a repository on dockerhub named go-helloworld
docker tag go-helloworld nduati/go-helloworld:V1.0.0
docker images
docker push nduati/go-helloworld:V1.0.0
**validate by opening dockerhub to see the tag

````
ll
vi vagrantfile
:qa
vagrant --version
vagrant up
vagrant status
vagrant ssh
sudo su
kubectl get no


````

kubectl describe pods | grep -i apparmor_parser

````

````
zypper install -t pattern apparmor

````

````
sudo curl -sfL https://get.k3s.io | K3S_KUBECONFIG_MODE="644" INSTALL_K3S_VERSION="v1.20.7+k3s1" sh -s -

````
kubectl run test --image nduati/go-helloworld:v1.0.0

````
kubectl port-forward nduati/go-helloworld-cb593381f60d 6111:6111 --address 0.0.0.0

````

````
kubectl expose deploy go-helloworld --port=6112 --target-port=6112
kubectl get svc
kubectl run test-$RANDOM --namespace=default --rm -it --image=alpine -- sh
wget -qO- 10.43.30.56:6112

````

````
kubectl get cm
kubectl create cm test-cm --from-literal=color=blue
kubectl describe cm test-cm
kubectl get secrets
kubectl create secret generic  test-sec --from-literal=color=red
kubectl get secrets
kubectl describe secrets test-sec
kubectl get secrets test-sec -o yaml
echo "cmVk" | base64 -d

````

````
kubectl get ns
kubectl create ns test-udacity
kubectl get po
kubectl get po -n test-udacity
kubectl create deploy [PARAMS] -n test-udacity

````

````
# create the namespace 
# note: label option is not available with `kubectl create`
kubectl create ns demo

# label the namespace
kubectl label ns demo tier=test

# create the nginx-alpine deployment 
kubectl create deploy nginx-alpine --image=nginx:alpine  --replicas=3 --namespace demo

# label the deployment
kubectl label deploy nginx-alpine app=nginx tag=alpine --namespace demo

# expose the nginx-alpine deployment, which will create a service
kubectl expose deployment nginx-alpine --port=8111 --namespace demo

# create a config map
kubectl create configmap nginx-version --from-literal=version=alpine --namespace demo

````

````
kubectl create ns demo2 --dry-run=client -o yaml
kubectl create ns demo2 --dry-run=client -o yaml > namespace.yaml
kubectl apply -f namespace.yaml
kubectl create deploy busybox --image=busybox -r=5 -n demo2 --dry-run -o yaml
kubectl create deploy busybox --image=busybox -r=5 -n demo2 --dry-run -o yaml > deploy.yaml
kubectl apply -f deploy.yaml
kubectl get deploy -n demo2
kubectl get po -n demo2
````

````
* https://argoproj.github.io/argo-cd/getting_started/#1-install-argo-cd
* https://argoproj.github.io/argo-cd/getting_started/

kubectl get po -n argocd
kubectl get svc -n argocd
kubectl get svc -n argocd argocd-server -o yaml
kubectl get svc -n argocd argocd-server -o yaml > argocd-nodeport.yaml

apiVersion: v1
kind: Service
metadata:
  annotations:
  labels:
    app.kubernetes.io/component: server
    app.kubernetes.io/name: argocd-server
    app.kubernetes.io/part-of: argocd
  name: argocd-server-nodeport
  namespace: argocd
spec:
  ports:
  - name: http
    port: 80
    protocol: TCP
    targetPort: 8080
    nodePort: 30007
  - name: https
    port: 443
    protocol: TCP
    targetPort: 8080
    nodePort: 30008
  selector:
    app.kubernetes.io/name: argocd-server
  sessionAffinity: None
  type: NodePort
status:
  loadBalancer: {}

kubectl apply -f argocd-nodeport.yaml
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d

````
