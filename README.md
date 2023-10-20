# GitHub Secrets, Actions & Workflows: A Step-by-Step Guide

This repository provides a comprehensive guide on how to use GitHub features like secrets, actions, and workflows. Whether you're a beginner trying to integrate CI/CD pipelines or an advanced user looking to optimize your workflow, this guide has something for you. With this step-by-step instruction set, you'll learn how to securely manage sensitive information using GitHub Secrets, automate tasks using GitHub Actions, and streamline your software development lifecycle with GitHub Workflows. The guide is complete with code samples, configuration files, and expert tips to assist you through the process.

## Table of Contents

- [Understanding Secrets and Tokens in GitHub](#Understanding-Secrets-and-Tokens-in-GitHub)
- [Prerequisites](#prerequisites)
- [GitHub Secrets](#github-secrets)
  - [Creating a Secret](#creating-a-secret)
  - [Using Secrets in Workflows](#using-secrets-in-workflows)
- [GitHub Actions](#github-actions)
  - [Basic Workflow](#basic-workflow)
  - [Advanced Workflow](#advanced-workflow)
- [GitHub Workflows](#github-workflows)
  - [Simple CI/CD Pipeline](#simple-cicd-pipeline)
  - [Matrix Strategy](#matrix-strategy)
- [Troubleshooting](#troubleshooting)


## Understanding Secrets and Tokens in GitHub

In GitHub, managing sensitive information securely is crucial. GitHub provides different ways to manage such data: **Repository Secrets** and **Personal Access Tokens (PAT)**. Understanding the differences between these two can help you choose the right approach for your workflow.

### Repository Secrets

#### Scope
- Restricted to the specific repository where they are defined.

#### Usage
- Mainly used to securely manage sensitive information required by GitHub Actions in that specific repository.

#### UI Path
- GitHub Repo > `Settings` > `Secrets & variables` > `Actions` > `Repository secrets`

### Personal Access Tokens (PAT)

#### Scope
- Can have broader permissions depending on how they are configured, potentially across all repositories owned by a user.

#### Usage
- Used for various tasks, such as cloning repositories, GitHub API calls, and more.

#### UI Path
- GitHub > `Settings` > `Developer settings` > `Personal access tokens`

### Key Differences

1. **Scope**: 
    - Repository Secrets are limited to the repository they are set in.
    - Personal Access Tokens can have a much wider scope, depending on your settings.

2. **Permissions**:
    - Repository Secrets do not have permissions; they are just values that are encrypted and stored, to be used in GitHub Actions.
    - Personal Access Tokens have specific permissions like `repo`, `admin`, `user`, etc., which define what actions the token can perform.

3. **Use Case**:
    - Repository Secrets are generally used for GitHub Actions in the repository they belong to.
    - Personal Access Tokens can be used for various tasks like accessing the GitHub API, cloning private repositories, etc., and are not tied to a specific repository or Action.

4. **Visibility**:
    - Repository Secrets are not exposed in build logs but can be exposed if you explicitly print them.
    - Personal Access Tokens can be embedded in code (though this is not recommended) and have the risk of being exposed if not handled securely.

To summarize, if you're managing secrets specifically for GitHub Actions in a single repository, then using **Repository Secrets** is the way to go. If you need a token that can perform a variety of tasks and possibly across different repositories, then **Personal Access Tokens** would be more suitable.


## Prerequisites

- A GitHub Account
- Basic familiarity with Git commands
- A repository to work with

## GitHub Secrets

### Creating a Secret

1. Navigate to your GitHub repository and go to `Settings` > `Secrets`.
2. Click `New repository secret`.
3. Enter a `Name` for your secret and the `Value`.
4. Click `Add secret` to save.

### Using Secrets in Workflows

To use the secret in a workflow, refer to it using the `secrets` context:

```yaml
jobs:
  example_job:
    runs-on: ubuntu-latest
    steps:
    - name: Use secret
      run: echo "API_TOKEN=${{ secrets.API_TOKEN }}"
```

## GitHub Actions

### Basic Workflow

Create a `.github/workflows/main.yml` file in your repository with the following content:

```yaml
name: Basic CI Workflow

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2
    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '16'
    - run: npm install
    - run: npm test
```

### Advanced Workflow

For an advanced setup, use `if` conditions, matrix strategies, and other advanced features:

```yaml
name: Advanced CI Workflow

on: [push, pull_request]

jobs:
  build:
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest, windows-latest]
    steps:
    - uses: actions/checkout@v2
    - name: Set up Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '16'
    - run: npm install
    - run: npm test
```

## GitHub Workflows

### Simple CI/CD Pipeline

For a basic CI/CD Pipeline, combine building, testing, and deploying steps:

```yaml
name: CI/CD Pipeline

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - run: npm install
    - run: npm test
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
    - name: Deploy
      run: ./deploy.sh
```

### Matrix Strategy

To test on multiple operating systems or versions, use the matrix strategy:

```yaml
jobs:
  test:
    strategy:
      matrix:
        os: [ubuntu-latest, macos-latest]
        node: [14, 16]
    runs-on: ${{ matrix.os }}
    steps:
    - name: Checkout Code
      uses: actions/checkout@v2
    - name: Setup Node
      uses: actions/setup-node@v2
      with:
        node-version: ${{ matrix.node }}
    - run: npm install
    - run: npm test
```

## Troubleshooting

1. **Action Failed**: Check the logs for errors.
2. **Secrets not Available**: Ensure they're available in the current context.
3. **Workflow not Triggering**: Check the `on` section of your YAML file.

For more help, refer to the [official documentation](https://docs.github.com/en/actions).

Feel free to clone, fork, and contribute to this repository. Happy Coding!
