# DevSecOps Automation Lab

A hands-on DevSecOps project that demonstrates how code quality, testing, security checks, CI pipelines, and infrastructure validation can be automated on a Windows development machine.

## Project objective

The goal was to build a small but realistic DevSecOps workflow around a Python application and learn how three core automation tools work together:

- **GitHub Actions** for cloud-based continuous integration and pull-request checks
- **Jenkins** for a self-hosted CI pipeline running locally in Docker
- **Terraform** for infrastructure as code

The project intentionally uses a small Python application so the focus remains on the automation practices rather than application complexity.

## What was built

### Python application and tests

The repository contains a simple Python `add` function and unit tests that verify normal and negative-number inputs.

```text
app/
├── __init__.py
└── app.py

tests/
└── test_app.py
```

## GitHub Actions implementation

GitHub Actions runs automatically on pushes and pull requests targeting `main`.

The CI workflow performs:

| Check | Tool | Purpose |
| --- | --- | --- |
| Lint | Ruff | Checks Python code quality and formatting |
| Unit tests | pytest | Verifies application behavior |
| Security scan | Bandit | Scans Python code for common security issues |
| Terraform validation | Terraform | Checks formatting and validates infrastructure configuration |

Additional repository security controls include:

- **Dependency Review** to identify vulnerable dependencies introduced in pull requests
- **Dependabot** to check GitHub Actions dependencies weekly and open update pull requests

This means a change is reviewed automatically before it is merged into `main`.

## Jenkins implementation

Jenkins runs locally in Docker Desktop and provides a second, self-hosted CI system.

The Jenkins controller uses a custom Docker image that includes:

- Git
- Python
- pytest
- Ruff
- Bandit
- Terraform

The Jenkins pipeline is defined in the repository’s `Jenkinsfile` and includes these stages:

```text
Checkout
  ↓
Lint
  ↓
Unit tests
  ↓
Security scan
  ↓
Terraform validation
```

Jenkins checks the repository for changes on a schedule and can automatically build when it detects a new change on `main`.

Jenkins is available locally at:

```text
http://localhost:8080
```

## Terraform implementation

Terraform manages local Docker infrastructure rather than cloud resources. This keeps the lab safe, free, and easy to run on a personal PC.

Terraform creates and manages:

- An Nginx Docker image
- A Docker container named `terraform-nginx-lab`
- An Nginx service available at `http://localhost:8082`

```text
terraform/
├── main.tf
└── .terraform.lock.hcl
```

Terraform state remains local and is excluded from Git. The provider lock file is committed so other environments use the same provider version.

CI validates Terraform configuration with:

```text
terraform fmt -check
terraform init -backend=false -input=false
terraform validate -no-color
```

Infrastructure changes are not applied automatically by CI. Running `terraform apply` remains a deliberate local action.

## Project structure

```text
github-actions-lab/
├── app/
│   ├── __init__.py
│   └── app.py
├── tests/
│   └── test_app.py
├── terraform/
│   ├── main.tf
│   └── .terraform.lock.hcl
├── jenkins/
│   └── Dockerfile
├── .github/
│   ├── workflows/
│   │   ├── ci.yml
│   │   └── dependency-review.yml
│   └── dependabot.yml
├── Jenkinsfile
├── .gitignore
└── README.md
```

## Key outcomes

This project successfully demonstrates:

- Version-controlled application code and infrastructure code
- Automated pull-request checks
- Python linting, testing, and basic security scanning
- Dependency review and automated update monitoring
- A local Jenkins pipeline running inside Docker
- Terraform-managed Docker infrastructure
- Safe infrastructure validation in CI without automatic deployment

## Local commands

Run Python tests:

```powershell
py -m pytest
```

View Terraform-managed resources:

```powershell
cd terraform
terraform state list
```

Preview Terraform infrastructure changes:

```powershell
terraform plan
```

Apply an approved infrastructure change:

```powershell
terraform apply
```

## Future improvements

Possible next steps include:

- Configure a secure GitHub webhook for immediate Jenkins builds
- Add Terraform plan output to pull-request comments
- Use a remote Terraform state backend for team collaboration
- Add container image scanning
- Add deployment approval stages
- Provision cloud infrastructure with Terraform

## Status

This project is complete as a local DevSecOps automation lab. The Nginx service remains managed by Terraform on port `8082`, and Jenkins remains available on port `8080`.