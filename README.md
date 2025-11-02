# Lab #4: GitHub Actions

This repository contains the three workflow files required for the GitHub Actions lab, each demonstrating a core concept of the CI/CD platform.

## ⚙️ Workflows Summary

### 1. 01-dependent-jobs.yml (Job Dependencies)

* **Purpose:** To create a sequential CI/CD pipeline 
* **Key Concepts Demonstrated:**
    * **`needs`**: Used to create explicit dependencies between jobs. The `test` job depends on `build`, and `deploy` depends on `test`, ensuring the required execution sequence.
    * **Trigger**: Configured to run automatically `on: push` to the `main` branch.

### 2. 02-env-and-secrets.yml (Environment Variables & Secrets)

* **Purpose:** To demonstrate how to define and use environment variables at different levels of scope, and how to securely handle sensitive data using secrets.
* **Key Concepts Demonstrated:**
    * **`workflow_dispatch`**: Allows the workflow to be triggered **manually** from the GitHub Actions tab.
    * **`env` Scope**: Variables are defined at the workflow, job, and step levels, showing that variables are inherited downwards but not upwards.
    * **`secrets`**: Repository secrets (e.g., `AWS_ACCESS_KEY_ID`) are accessed using the `${{ secrets.SECRET_NAME }}` syntax. The values are automatically **masked (\*\*\*)** in the workflow logs for security.

### 3. 03-multi-platform.yml (Multi-Platform Testing)

* **Purpose:** To run a set of tasks simultaneously across different operating systems to verify cross-platform compatibility.
* **Key Concepts Demonstrated:**
    * **Parallel Execution**: All three jobs (`ubuntu-job`, `windows-job`, `macos-job`) run **in parallel** because none of them use the `needs` key.
    * **`runs-on`**: Used to specify different virtual runners for each job: `ubuntu-latest`, `windows-latest`, and `macos-latest`.
    * **Trigger**: Configured to run only `on: pull_request` targeting the `main` branch, a standard practice for pre-merge testing.
    * **Shell Differentiation**: Used the standard shell for Linux/macOS and explicitly set `shell: pwsh` for Windows to run platform-specific commands.

## 💡 Challenges Faced and Resolutions
* **Challenge 1: Triggering the Multi-Platform Workflow (Part C)**
    * **The Challenge:** After committing and pushing the `03-multi-platform.yml` file, I initially expected the workflow to run automatically. However, the workflow did not appear in the Actions tab.
    * **Resolution:** I realized the workflow was intentionally configured to run only `on: pull_request`. The system prevents it from running on a simple `push`. To correctly trigger the workflow, I had to create a new branch (`test-workflow-3`), commit the file to that branch, and then **open a Pull Request** targeting the `main` branch, which successfully triggered all three parallel jobs as required.
