# Resources
* https://medium.com/@SabujJanaCodes/kubernetes-club-ep-01-decrypting-a-k8s-cluster-hands-on-84a9b7b7bd4d

### Commands
* kubectl create namespace fightclub
* kubectl get pods -n fightclub
* kubectl get pods -n fightclub -o wide
* kubectl apply -f goapp1.yaml
* kubectl apply -f goapp2.yaml
* kubectl apply -f goapp3.yaml
* kubectl port-forward -n fightclub mygoapp-pod1 8083