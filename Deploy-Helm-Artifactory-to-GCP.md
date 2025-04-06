# Deploy Nginx Helm Chart to GCloud Using Artifactory Image Repo


### Load Docker Image
    docker load -i docker-image-<name>.tar

### Create values override
```shell
cat mydemo_values.yaml

ingress:
  enabled: true
  className: "nginx"
  hosts:
    - host: localhost
      paths:
        - path: /
          pathType: ImplementationSpecific
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
```


### Add artifactory secret
```shell
kubectl create secret docker-registry artifactory-creds \
  --docker-server=<artifactory-domain>:443 \
  --docker-username=<artifactory-email> \
  --docker-password='<artifactory-pass>' \
  --docker-email=<artifactory-email>
```

### Install helm chart
    helm install mydemo -f mydemo_values.yaml helm-package-<name>.tgz

### Get Services and check if EXTERNAL_IP is exposed 
    kubectl get svc --namespace default
    NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
    mydemo-app      ClusterIP   34.118.225.30   <none>        8080/TCP   4m50s
    kubernetes      ClusterIP   34.118.224.1    <none>        443/TCP    4h54m

### Install Nginx Ingress
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.9.4/deploy/static/provider/cloud/deploy.yaml


### Is the Ingress resource created and active?
    kubectl get ingress
    NAME                 CLASS   HOSTS       ADDRESS         PORTS   AGE
    mydemo-app   nginx   localhost   34.70.180.157   80      10m

### Does the Ingress Controller have an EXTERNAL-IP?
    kubectl get svc -n ingress-nginx
    NAME                                 TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)                      AGE
    ingress-nginx-controller             LoadBalancer   34.118.233.6     34.70.180.157   80:32251/TCP,443:30865/TCP   12m
    ingress-nginx-controller-admission   ClusterIP      34.118.232.189   <none>          443/TCP                      12m

### Re-install helm chart
    helm uninstall mydemo
    helm install mydemo -f mydemo_values.yaml helm-package-<name>.tgz

### Test mydemo-app
    curl -H "Host: localhost" http://34.70.180.157