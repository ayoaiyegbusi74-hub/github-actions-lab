# DevSecOps Automation Lab

A hands-on DevSecOps project that builds and improves an automated software-delivery pipeline using GitHub Actions, Jenkins, and Terraform.

## Project goals

This project is designed to practice how modern DevSecOps automation works:

- Validate code automatically on every push and pull request
- Enforce code-quality checks before merging changes
- Run unit tests automatically
- Scan Python code for common security issues
- Review dependency changes for known vulnerabilities
- Keep GitHub Actions dependencies up to date
- Later, build a Jenkins pipeline and provision infrastructure with Terraform

## Current implementation

The project currently uses GitHub Actions for continuous integration.

### CI workflow

The `DevSecOps CI` workflow runs when code is pushed or when a pull request targets the `main` branch.

It includes three parallel checks:

| Check | Tool | Purpose |
| --- | --- | --- |
| Lint | Ruff | Checks code quality and formatting |
| Unit tests | pytest | Verifies application behavior |
| Security scan | Bandit | Scans Python code for common security issues |

A pull request should only be merged after all checks pass.

### Dependency security

This project also includes supply-chain security controls:

- **Dependency Review** checks pull requests for newly introduced vulnerable dependencies.
- **Dependabot** checks GitHub Actions dependencies weekly and opens pull requests when updates are available.

## Project structure

```text
github-actions-lab/
├── app/
│   ├── __init__.py
│   └── app.py
├── tests/
│   └── test_app.py
├── .github/
│   ├── workflows/
│   │   ├── ci.yml
│   │   └── dependency-review.yml
│   └── dependabot.yml
└── README.md
```

## Local application

The sample Python application provides a simple `add` function.

Run it locally:

```powershell
py app\app.py
```

Run the tests locally:

```powershell
py -m pytest
```

## CI/CD workflow

```text
Developer creates a branch
        ↓
Developer pushes changes
        ↓
Pull request is opened
        ↓
GitHub Actions runs linting, tests, and security scanning
        ↓
Dependency Review checks dependency changes
        ↓
All checks pass
        ↓
Pull request is merged into main
```

## Planned work

The next phases of this lab are:

1. **Jenkins**
   - Run Jenkins locally on Windows using Docker
   - Create a Jenkins pipeline for the same application
   - Compare Jenkins pipelines with GitHub Actions workflows

2. **Terraform**
   - Learn Terraform fundamentals
   - Define cloud or local infrastructure as code
   - Integrate infrastructure validation into CI

3. **Pipeline improvements**
   - Add dependency manifests and dependency auditing
   - Add artifact handling
   - Add branch-protection rules
   - Add deployment stages

## Notes

This README is a living document. It will be updated as the project evolves and as new DevSecOps tools are added.

