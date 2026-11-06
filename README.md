# Kindergarten Project — Run Everything in Docker
 
The whole project has been containerized. Every service has its own
`Dockerfile` and `.dockerignore`, and `docker-compose.yml` starts the
frontend, all APIs, and the thumbnail worker together with MongoDB, RabbitMQ,
RustFS, and the MongoDB dashboard.
 
## Services
 
| Service | Base image | Port |
| --- | --- | --- |
| Frontend | `node:24` to build, `nginx` to serve | 3000 |
| Student service | `golang:1.23` | 5001 |
| Teacher service | `maven:3.9-eclipse-temurin-17` | 5002 |
| Employee service | `python:3.13-slim` | 5003 |
| Profile service | `node:22` | 5004 |
| Thumbnail worker | `python:3.13-slim` | — |
 
## What changed
 
- The frontend is built with `npm run build` and served by nginx.
- `.env` files are not copied into the images. Docker Compose passes each
service's `.env` in when the container starts (`env_file`).
- Inside Docker, each service's `.env` must use the container names as hosts:
`mongo:27017`, `rabbitmq:5672`, and `object-storage:9000` (not `localhost`).
- Backend services wait for MongoDB (and RabbitMQ and RustFS where needed) to
be healthy before starting, and restart automatically if they fail.
## Run
 
From the repository root, run:
 
```sh
docker compose up -d --build
docker ps
```
 
Wait about 30 seconds, then open [http://localhost:3000](http://localhost:3000).
 
## Troubleshooting
 
If a service is not working, check its logs:
 
```sh
docker logs -f <service-name>
```
 
## Stop
 
```sh
docker compose down
```
 
> **Note:** when everything runs in Docker, do not start the services locally
> at the same time, because ports `3000` and `5001`–`5004` are already in use.