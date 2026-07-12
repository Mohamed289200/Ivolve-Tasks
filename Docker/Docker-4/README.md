# Lab 6 - Managing Docker Environment Variables Across Build and Runtime

## Objective

This lab demonstrates three different ways to pass environment variables to a Dockerized Flask application.

---

## Technologies

- Python 3.11
- Flask
- Docker

---

## Dockerfile

```dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY app.py .

RUN pip install --no-cache-dir flask

ENV APP_MODE=production
ENV APP_REGION=canada-west

EXPOSE 8080

CMD ["python", "app.py"]
```

---

## Build Docker Image

```bash
docker build -t flask-app .
```

![Docker Build](screenshots/01-docker-build.png)

---

## Method 1 - Runtime Variables (-e)

```bash
docker run -d --name flask-dev \
-p 8080:8080 \
-e APP_MODE=development \
-e APP_REGION=us-east \
flask-app
```

**Output**

```
App mode: development, Region: us-east
```

![Development Environment](screenshots/02-development-env.png)

---

## Method 2 - Environment File

Create:

```text
staging.env
```

```
APP_MODE=staging
APP_REGION=us-west
```

Run:

```bash
docker run -d --name flask-stage \
-p 8081:8080 \
--env-file staging.env \
flask-app
```

**Output**

```
App mode: staging, Region: us-west
```

![Staging Environment](screenshots/03-staging-env.png)

---

## Method 3 - Dockerfile ENV

```dockerfile
ENV APP_MODE=production
ENV APP_REGION=canada-west
```

Build:

```bash
docker build -t flask-app-prod .
```

Run:

```bash
docker run -d --name flask-prod \
-p 8082:8080 \
flask-app-prod
```

**Output**

```
App mode: production, Region: canada-west
```

![Production Environment](screenshots/04-production-env.png)

---

## Running Containers

```bash
docker ps
```

![Docker PS](screenshots/05-docker-ps.png)

---

## Docker Logs

```bash
docker logs flask-dev
```

![Docker Logs](screenshots/06-docker-logs.png)

---

## Cleanup

```bash
docker stop flask-dev flask-stage flask-prod

docker rm flask-dev flask-stage flask-prod

docker rmi flask-app flask-app-prod
```

![Cleanup](screenshots/07-cleanup.png)

---

## Result

- ✅ Successfully built the Docker image.
- ✅ Successfully configured runtime environment variables using `-e`.
- ✅ Successfully configured environment variables using `--env-file`.
- ✅ Successfully configured default environment variables using `ENV`.
- ✅ Successfully tested all three deployment methods.
- ✅ Successfully cleaned up containers and images.

---

## Author

**Mohamed Abdelhamed**

Cloud DevOps Accelerator Program
