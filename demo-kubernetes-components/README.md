## TechWorld with Nana

### Resource
* https://youtu.be/EQNO_kM96Mo?si=Gfa4zOvPRqEnD-4Q

### kubectl apply commands in order
    
    kubectl create namespace techworld-nana
    kubectl apply -f mongo-secret.yaml
    kubectl apply -f mongo.yaml
    kubectl apply -f mongo-configmap.yaml
    kubectl apply -f mongo-express.yaml

### give a URL to external service in minikube

    minikube service mongo-express-service -n techworld-nana
    username = admin
    password = pass

### kubectl get commands

    kubectl get pod -n techworld-nana
    kubectl get pod -o wide -n techworld-nana
    kubectl get service -n techworld-nana
    kubectl get secret -n techworld-nana
    kubectl get pod --watch

### kubectl debugging commands

    kubectl describe pod mongodb-deployment-xxxxxx
    kubectl describe service mongodb-service
    kubectl logs mongo-express-xxxxxx
