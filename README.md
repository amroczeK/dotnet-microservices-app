# .NET Microservices Project
ASP.NET Core Microservices Project utilising Docker, Kubernetes, API Gateway, HTTP, gRPC and RabbitMQ for Event Bus.

This project covers the following scope:
- Building two .NET Microservices using the REST API pattern
- Working with dedicated persistence layers for both services
- Deploying our services to Kubernetes cluster
- Employing the API Gateway pattern to route to our services
- Building Synchronous messaging between services (HTTP & gRPC)
- Building Asynchronous messaging between services using an Event Bus (RabbitMQ)

### Building service image and deploying to Docker Hub
1. Build image locally: `docker build -t <user id>/platformservice .` or `docker build -t <user id>/commandservice .`
2. Push image: `docker push <user id>/platformservice` or `docker push <user id>/commandservice`
3. Run as container: `docker run -p 8080:80 amroczek9990/commandservice` add `-d` argument if you want to run in detached mode.

### Kubernetes Deployments
- View current deployments: `kubectl get deployments`
- View running pods: `kubectl get pods`
- View services: `kubectl get services`
- Remove deployments: `kubectl get deploy` then `kubectl delete deploy <deployment name>`
- Apply deployment: `kubectl apply -f K8S/<file name>.yaml`
- Refresh deployments with latest build changes: `kubectl rollout restart deployment <deployment name>`

### Making requests to the services via Node Port service (entry point)
- Retrieve egress port for Node Port service: `kubectl get services platform-nodeport-service-srv`
```
NAME                            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
platform-nodeport-service-srv   NodePort   10.109.188.86   <none>        80:32620/TCP   38m
```
- Use the egress port about to make the request from localhost into the kubernetes cluster e.g. `curl http://localhost:32620/api/platforms`
- Use postman collection and environment attached, edit port value

### Making requests to the services via Ingress NGINX Load Balancer
- [Follow the installation guide your the Docker Desktop provider](https://kubernetes.github.io/ingress-nginx/deploy/#docker-desktop)
- e.g. use YAML manifest: `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.3/deploy/static/provider/cloud/deploy.yaml` NOTE: Issue with v1.0.9, had to downgrade to v1.0.3
- Ingress NGINX pods will reside in the `ingress-nginx` namespace
```
% kubectl get namespace 
NAME              STATUS   AGE
default           Active   234d
ingress-nginx     Active   77m
kube-node-lease   Active   234d
kube-public       Active   234d
kube-system       Active   234d

% kubectl get pods -n ingress-nginx
NAME                                        READY   STATUS      RESTARTS   AGE
ingress-nginx-admission-create-kvzd8        0/1     Completed   0          68m
ingress-nginx-admission-patch-ntrgv         0/1     Completed   1          68m
ingress-nginx-controller-646d5d4d67-rshxt   1/1     Running     0          68m
% kubectl get services -n ingress-nginx
NAME                                 TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx-controller             LoadBalancer   10.104.119.156   localhost     80:31184/TCP,443:30500/TCP   71m
ingress-nginx-controller-admission   ClusterIP      10.102.92.210    <none>        443/TCP                      71m

% kubectl describe ingress ingress-srv
Warning: extensions/v1beta1 Ingress is deprecated in v1.14+, unavailable in v1.22+; use networking.k8s.io/v1 Ingress
Name:             ingress-srv
Namespace:        default
Address:          localhost
Default backend:  default-http-backend:80 (<error: endpoints "default-http-backend" not found>)
Rules:
  Host        Path  Backends
  ----        ----  --------
  acme.com    
              /api/platforms     platforms-clusterip-srv:80 (10.1.13.83:80)
              /api/c/platforms   commands-clusterip-srv:80 (10.1.13.79:80)
Annotations:  kubernetes.io/ingress.class: nginx
              nginx.ingress.kubernetes.io/use-regex: true
Events:
  Type    Reason  Age                From                      Message
  ----    ------  ----               ----                      -------
  Normal  Sync    39m (x4 over 58m)  nginx-ingress-controller  Scheduled for sync
  Normal  Sync    20m                nginx-ingress-controller  Scheduled for sync
```