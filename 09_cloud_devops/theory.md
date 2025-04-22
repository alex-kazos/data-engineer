# Section 9: Cloud & DevOps for Data Engineering (Theory)

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

## 3. Deployment Basics (CI/CD)
- **CI:** Continuous Integration (automated testing)
- **CD:** Continuous Deployment/Delivery (auto-release)
- Tools: GitHub Actions, GitLab CI, Jenkins

## 4. Cloud Platforms (Serverless)
- **AWS Lambda:** Run Python code on demand
- **GCP Cloud Functions:** Similar for GCP
- **Benefits:** No server management, auto-scaling

## 5. Infrastructure as Code (Terraform)
- Define cloud resources as code
- Version, review, and automate infra changes

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
