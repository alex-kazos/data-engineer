# Section 9: Cloud & DevOps for Data Engineering

## Overview
Cloud and DevOps skills enable scalable, automated, and reliable data engineering workflows.

---

## 1. What is DevOps?
- Practices/tools for automating software delivery and infrastructure
- Data engineers use DevOps for reproducibility and scale

## 2. Containerization (Docker)
- **Docker:** Package code + dependencies into containers
- **Images:** Blueprints for containers
- **Volumes:** Persist data outside containers
- **Networking:** Connect containers and services

Here is an example of a Dockerfile:
```Dockerfile
FROM python:3.8-slim
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

Here is an example of a docker-compose.yml:
```yaml
version: '3'
services:
  app:
    build: .
    ports:
      - "8080:8080"
    volumes:
      - .:/app
```

Here is an example of a docker run command:
```bash
# Build a Docker image
docker build -t myapp .
# Run a container
docker run -p 8080:8080 myapp
```

## 3. Deployment Basics (CI/CD)
- **CI:** Continuous Integration (automated testing)
- **CD:** Continuous Deployment/Delivery (auto-release)
- Tools: GitHub Actions, GitLab CI, Jenkins

Here is an example of a GitHub Actions workflow:
```yaml
name: CI
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run tests
        run: pytest
```

## 4. Cloud Platforms (Serverless)
- **AWS Lambda:** Run Python code on demand
- **GCP Cloud Functions:** Similar for GCP
- **Benefits:** No server management, auto-scaling

Here is an example of a AWS Lambda function:
```python
def handler(event, context):
    return {'statusCode': 200, 'body': 'Hello from Lambda!'}
```

## 5. Infrastructure as Code (Terraform)
- Define cloud resources as code
- Version, review, and automate infra changes

Here is an example of a Terraform resource:
```hcl
resource "aws_s3_bucket" "bucket" {
  bucket = "my-bucket"
}
```

## 6. Monitoring & Logging
- Use cloud-native tools (CloudWatch, Stackdriver)
- Log pipeline events/errors

## 7. Best Practices
- Use Docker for reproducibility
- Automate tests and deployments
- Secure secrets (never hardcode)

## References
- [Docker Docs](https://docs.docker.com/)
- [GitHub Actions](https://docs.github.com/en/actions)
- [Terraform Docs](https://developer.hashicorp.com/terraform/docs)
