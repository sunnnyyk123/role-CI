# Ansible Roles CI Workflow Guide

<div align="center">
  <img src="https://technovids.com/wp-content/uploads/2020/04/Ansible-DevOps-Automation.jpg.webp" alt="Ansible Logo" width="40%"/>
</div>

---

## Table of Contents

1. [Introduction](#introduction)  
2. [What is the CI Workflow?](#what-is-the-ci-workflow)  
3. [CI Workflow Diagram](#ci-workflow-diagram)  
4. [CI Workflow Explanation](#ci-workflow-explanation)  
5. [Best Practices](#best-practices)  
6. [Troubleshooting](#troubleshooting)  
7. [Conclusion](#conclusion)  
8. [References](#references)  

---

## Introduction

This document outlines the Continuous Integration (CI) workflow used to automate testing and validation of Ansible roles in CI/CD pipelines like GitLab CI. It helps ensure that your infrastructure code is secure, clean, and production-ready.

---

## What is the CI Workflow?

A typical CI workflow for Ansible roles includes:

- Git Checkout  
- Clean Workspace  
- Credential Scanning  
- Syntax Check  
- Ansible Linting  
- Dry Run (Check Mode)  
- Send Notification  

---

## CI Workflow Diagram

<p align="center">
  <img src="https://github.com/vardaan412/Images/blob/4fca1663dc02a588cf2f33a5e9f21890d5b6e4f7/CI.png" alt="CI Workflow" />
</p>

---

## CI Workflow Explanation

### 1. Git Checkout

Pull the latest code from the target branch:

```
git checkout <branch-name>
```

### 2. Clean Workspace

Clean all untracked and temporary files to start fresh:

```
git clean -fdx
```

### 3. Credential Scanning

Scan the repository for hardcoded secrets using gitleaks:



```
gitleaks detect --source
```
### 4. Syntax Check

Validate the syntax of your Ansible playbook:



```
ansible-playbook playbook.yml --syntax-check
```

### 5. Ansible Linting

Lint your Ansible role or playbook to ensure it follows best practices:



```
ansible-lint
```

### 6. Dry Run (Check Mode)

Simulate the playbook run without making actual changes:



```
ansible-playbook playbook.yml --check
```

### 7. Send Notification
Notify the team after the CI process is complete:



```
echo "CI Workflow completed!" | mail -s "CI Report" dev-team@example.com
```

---
## Best Practices
|  No. | Best Practice                                                                                   |
|-------|--------------------------------------------------------------------------------------------------|
| 1.  | Use git clean -fdx to avoid any untracked file issues before builds.                          |
| 2.   | Perform secret scanning early to prevent sensitive data leaks.                                   |
| 3.   | Keep ansible-lint updated to use the latest linting rules.                                     |
| 4.   | Use ansible-playbook --syntax-check and --check to validate without executing.              |
| 5.   | Send notifications at the end of the pipeline (both success and failure) for visibility.         |
| 6.   | Use proper branching strategies (feature/dev/main) with protected main branches.                 |

---
## Troubleshooting
|  Issue                                                                 |  Resolution                                                                 |
|-------------------------------------------------------------------------|------------------------------------------------------------------------------|
| Git checkout step fails                                                 | Ensure the repository and branch name are correct and accessible.            |
| git clean -fdx not working                                            | Make sure you're in the correct repo directory and have the right permissions.|
| Credential scanning (gitleaks) shows false positives                  | Update the rules or exclude false-positive files via config.                 |
| ansible-lint throwing unexpected errors                               | Update ansible-lint, check rules, and fix YAML formatting.                 |
| Syntax check fails                                                      | Ensure your playbook.yml is valid YAML and all included files exist.       |
| Dry run fails or behaves unexpectedly                                   | Check if variables are defined and inventory/configuration is correct.       |
| Notifications not sent                                                  | Verify webhook/SMTP config, secrets, and internet access.                    |

---
## Conclusion
Implementing CI workflows for Ansible roles ensures quality, reliability, and rapid feedback during role development. This SOP provides a comprehensive guide to setting up CI with ansible-lint, molecule, and GitLab pipelines to streamline the role testing process. By following these best practices, teams can maintain high standards across infrastructure codebases.

---
## References
|  Resource        | Link                                                                 | Description                                      |
|-------------------|----------------------------------------------------------------------|--------------------------------------------------|
| Ansible Lint      | [Ansible Lint](https://ansible-lint.readthedocs.io)                 | Best practices and checks for Ansible playbooks  |
| Molecule          | [Molecule](https://molecule.readthedocs.io)                         | Framework for testing Ansible roles              |
| GitLab CI Docs    | [GitLab CI Docs](https://docs.gitlab.com/ee/ci/)                    | Official GitLab documentation for CI/CD setup    |

---
