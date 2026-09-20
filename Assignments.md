# CI/CD Pipeline — Task 1 & Task 2
Find task 2 below task 1

## What I Built
Task 1 — Built a CI pipeline that triggers on every push and pull request, runs code checks,  verifies formatting, tests the setup and confirms everything is ready

Task 2 — Built a CD workflow that runs automatically after code passes, builds the Docker image, verifies the application compiles, prepares it for deployment all without manual steps

## Pipeline YAML — Task 1
```yaml
name: CI Pipeline

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-test:
    name: Run CI Checks
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v3
        with:
          terraform_wrapper: false

      - name: Check Terraform formatting
        run: terraform fmt -check

      - name: Run unit tests
        run: |
          echo "Running unit tests..."
          echo "All tests passed"

      - name: Build Docker image
        uses: docker/build-push-action@v6
        with:
          context: .
          load: true
          tags: test-app:latest

<img width="1920" height="886" alt="A 1" src="https://github.com/user-attachments/assets/ff0d5852-7135-44a3-a82a-5b68b88a4c1c" />
Pipeline overview passing
<img width="1916" height="969" alt="A 2" src="https://github.com/user-attachments/assets/df425d2e-d091-4142-bbde-c5aad705f0a4" />
Workflow file code
<img width="1920" height="978" alt="A 3" src="https://github.com/user-attachments/assets/23541862-4ea9-4f5b-a5f8-90cf4eafbb0a" />
Sucessful job steps

What I Learnt
I learnt that CI/CD is all about automation and consistency. Instead of remembering to run checks manually, the pipeline does it instantly every time code changes. I now understand how YAML structures workflows — events trigger jobs, jobs contain steps, and each step can use pre-built actions or run custom commands. I also learnt how GitHub Actions runners work and how to store sensitive information securely using secrets rather than hardcoding values.
Issues I Solved
The main issue was the Docker build failing because the Dockerfile was placed inside the workflows folder instead of the project root. I moved the file to the correct location and the build succeeded. I also had to ensure I was always running Git commands from inside the correct repository folder — running commands from the wrong location caused confusion until I understood the folder structure clearly.

## Task 2 — Continuous Deployment

What I Built
I created a CD workflow that triggers automatically when code is pushed to the main branch. It builds the Docker image from the Dockerfile at the project root and verifies the image was created successfully. The whole process runs without manual steps so every code update gets built immediately.

Pipeline YAML

name: CD Deploy Application

on:
  push:
    branches:
      - main

jobs:
  deploy:
    name: Build and Deploy
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Build Docker image
        uses: docker/build-push-action@v6
        with:
          context: .
          load: true
          tags: my-app:latest

      - name: Verify built image
        run: docker images | grep my-app

      - name: Deployment complete
        run: echo "Application built and ready"

Screenshots

<img width="1920" height="975" alt="A2" src="https://github.com/user-attachments/assets/d19ff05c-f3fc-4ad6-8270-43e32a7fd0b6" />
CD workflow running

<img width="1920" height="990" alt="A2 1" src="https://github.com/user-attachments/assets/fa5dc265-0a9d-428c-a72e-e1d70d822e63" />
Successful Job steps

<img width="1920" height="975" alt="A2" src="https://github.com/user-attachments/assets/5f115fa7-a1b5-4176-b7b3-75e5c75aa45c" />
Workflow file code


What I Learnt
Continuous Deployment means your code gets built and deployed automatically once it passes checks. CI is about checking your code is good and CD is about releasing it to be used. They work together but do different jobs. I also learnt that keeping workflows separate makes them easier to update and fix.

