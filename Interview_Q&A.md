1) You are given a GitHub Actions workflow snippet. How would you identify incorrect steps and suggest improvements or missing steps for a robust CI/CD pipeline

  Ans) a) **Validate Workflow Structure**:
  
          A valid workflow should contain:

name (optional but recommended)

on (event trigger)

jobs

runs-on

steps

Example:

name: **CI Pipeline**
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v4
        
**Common mistakes**

Missing runs-on

Incorrect indentation (YAML is indentation-sensitive)

Jobs without steps

Steps missing run or uses

b) **Check for Missing Essential Steps**

Many workflows fail because important CI steps are missing.

Typical CI pipeline steps should include:

1️⃣ Checkout repository

Without this step, the runner cannot access the code.

- name: Checkout repository
  uses: actions/checkout@v4
  
3️⃣ Install dependencies

Example:

- name: Install dependencies
  run: npm install
4️⃣ Run tests
- name: Run tests
  run: npm test

Without tests → CI is incomplete.

5️⃣ Build step
- name: Build project
  run: npm run build


c) **Verify Use of Secrets**

run: docker login -u ${{ secrets.DOCKER_USER }} -p ${{ secrets.DOCKER_PASS }}


d) **Check Reusability**

Large workflows should use reusable workflows.

Example:

jobs:
  call-workflow:
    uses: org/repo/.github/workflows/build.yml@main

Benefits:

Avoid duplicate pipelines

Standardized CI


e) **Validate Workflow with Tools**

Use:

GitHub Actions Workflow Validator

actionlint

GitHub workflow syntax check

Example tool:

npm install -g actionlint
actionlint

========================================================

2) - name: Checkout repository
  uses: actions/checkout@v4

what does this mean ?

It is a GitHub Actions step that downloads your repository code into the runner environment so the workflow can use it.


**uses: actions/checkout@v4**

This tells GitHub Actions to use a pre-built action created by GitHub.

actions/checkout → The official action that checks out your repository.

@v4 → Version 4 of that action.

So GitHub runs the action which performs something similar to:

git clone https://github.com/your-repo/project.git

In simple terms:
actions/checkout@v4 copies your GitHub repository code into the workflow runner so the pipeline can work with it.

===================================================================
