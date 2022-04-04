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
1. Build image locally: `docker build -t <user id>/commandservice .`
2. Push image: `docker push <user id>/commandservice`
3. Run as container: `docker run -p 8080:80 amroczek9990/commandservice` add `-d` argument if you want to run in detached mode.
