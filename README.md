# coaching-18-ecs-multi-service-applications

# 🚀 Application Services


This repo contains two Flask microservices deployed on ECS via ECR.

---

## 📁 Structure
```bash
application-services/
├── service1/
│   ├── app.py
│   ├── Dockerfile
│   └── requirements.txt
├── service2/
│   ├── app.py
│   ├── Dockerfile
│   └── requirements.txt
└── .github/
    └── workflows/
        └── deploy.yml
```

---

## 🛠️ Setup
1. Add these secrets under repo Settings → Secrets → Actions:

| Name                    | Description                       |
|-------------------------|-----------------------------------|
| `AWS_ACCESS_KEY_ID`     | IAM user key                      |
| `AWS_SECRET_ACCESS_KEY` | IAM user secret                   |
| `AWS_REGION`            | AWS region (e.g. `us-east-1`)     |
| `AWS_ACCOUNT_ID`        | Your AWS account ID               |
| `ECR_REPO_SERVICE_1`    | From [infra-terraform](https://github.com/your-org/infra-terraform) output             |
| `ECR_REPO_SERVICE_2`    | From [infra-terraform](https://github.com/your-org/infra-terraform) output             |

---

## 🔄 CI/CD Workflow
On every push to `main`:
- Builds Docker images for `service1` and `service2`
- Tags and pushes them to ECR
- (Optional) You can extend the workflow to update ECS services with the new image tags using `aws ecs update-service`

Example step to add at the end of `deploy.yml`:
```yaml
- name: Update ECS services (optional)
  run: |
    aws ecs update-service --cluster your-cluster-name \
      --service your-service1-name \
      --force-new-deployment

    aws ecs update-service --cluster your-cluster-name \
      --service your-service2-name \
      --force-new-deployment
```

---

## 🧪 Local Dev
You can test the services locally with Docker and LocalStack. See [testing guide](#) for steps.

---

## 📬 Endpoints
- `service1`: `POST /upload` – Uploads file to S3
- `service2`: `POST /message` – Sends message to SQS

