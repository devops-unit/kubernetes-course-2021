https://kubernetes.io/docs/tasks/tools/
https://minikube.sigs.k8s.io/docs/start/
https://kubernetes.io/docs/reference/kubectl/cheatsheet/

curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb

minikube start
kubectl get po -A

kubectl create deployment hello-minikube --image=k8s.gcr.io/echoserver:1.4
kubectl expose deployment hello-minikube --type=NodePort --port=8080


minikube service hello-minikube --url
kubectl get services hello-minikube

#kubectl port-forward service/hello-minikube 7080:8080

curl -LO https://raw.githubusercontent.com/portainer/k8s/master/deploy/manifests/portainer/portainer.yaml
kubectl apply -n portainer -f portainer.yaml

minikube service portainer -n portainer

HW:

https://www.jenkins.io/doc/book/installing/kubernetes/


wget https://raw.githubusercontent.com/jenkins-infra/jenkins.io/master/content/doc/tutorials/kubernetes/installing-jenkins-on-kubernetes/jenkins-volume.yaml
wget https://raw.githubusercontent.com/jenkins-infra/jenkins.io/master/content/doc/tutorials/kubernetes/installing-jenkins-on-kubernetes/jenkins-sa.yaml

kubectl apply -f jenkins-volume.yaml
kubectl apply -f jenkins-sa.yaml

wget https://raw.githubusercontent.com/jenkinsci/helm-charts/main/charts/jenkins/values.yaml
