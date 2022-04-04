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