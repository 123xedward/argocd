# argocd (draft)

- Install minikube, check this link
- Install argocd
  kubectl create ns argo
  kubectl apply -n argo -f https://raw.githubusercontent.com/argoproj/argo/stable/manifests/quick-start-postgres.yaml
  kubectl -n argo port-forward deployment/argo-server 2746:2746


- install nginx
  sudo apt install nginx -y
- #editamos el servicio de argo-server, NodePort, port: 30000
  k edit svc argo-server -n argo
- #validamos, debe quedar algo similar:

  cloud_user@fadf2d00a61c:~$ k get svc -n argo
  NAME                          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
  argo-server                   NodePort    10.97.91.253     <none>        2746:30000/TCP   174m
  minio                         ClusterIP   10.101.81.225    <none>        9000/TCP         174m
  postgres                      ClusterIP   10.99.216.62     <none>        5432/TCP         174m
  workflow-controller-metrics   ClusterIP   10.104.185.168   <none>        9090/TCP         174m
  
- #obtenemos la ip de minikube (ejemplo 192.168.49.2)
  minikube ip
  
 - #esta ip la utilizaremos para el siguiente paso

  cd /etc/nginx
  sudo vim ./sites-enabled/default
  
  location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                proxy_pass https://192.168.49.2:30000;
        }
  
  
- #reiniciamos nginx
  
  sudo systemctl restart nginx
