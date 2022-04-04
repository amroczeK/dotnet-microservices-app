### Building service image and deploying to Docker Hub
1. Build image locally: `docker build -t <user id>/commandservice .`
2. Push image: `docker push <user id>/commandservice`
3. Run as container: `docker run -p 8080:80 amroczek9990/commandservice` add `-d` argument if you want to run in detached mode.