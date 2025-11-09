# Modernized-GitOps
This project demonstrates a complete GitOps workflow where both infrastructure and application lifecycle are managed declaratively using Git, enabling automated, consistent, and secure deployments.

## Key Components
### Infrastructure as Code (IaC) with Terraform
- AWS infrastructure is fully provisioned using Terraform.
- Resources include VPC, subnets, IAM roles, EKS cluster, node groups, and networking components.
- Terraform state is securely stored in a remote backend (S3 + DynamoDB or Terraform Cloud).
- Any infrastructure change is done through Pull Requests → Plan → Review → Apply using GitHub Actions.

### CI/CD with GitHub Actions
- CI Pipeline:
  Runs on code push or PR.
  Validates Terraform code (fmt, validate, tflint) and runs terraform plan.
  Builds application Docker images and pushes them to Amazon ECR.

- CD Pipeline (GitOps):
  Once the PR is approved and merged, GitHub Actions executes terraform apply.
  Application Kubernetes manifests (Helm or Kustomize) are updated in a GitOps config repository.

### GitOps Deployment to EKS
- Application deployment manifests are stored in a separate GitOps repository.
- Argo CD or Flux watches this repository and automatically syncs the desired state to the EKS cluster.
- Any change committed to the Git repository (infra or app) triggers reconciliation rather than manual kubectl apply commands.
