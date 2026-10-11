# Lesson 4.9: Tool Exploration - CI/CD Tools

## Learning Objectives

By the end of this lesson, you will be able to:
1. **Identify** the three main types of CI/CD tools: platform-integrated, managed CI services and self-hosted
2. **Explain** the trade-offs of each type of tool
3. **Select** the right type of tool, and a tool within it, for a given use case
4. **Build** a GitHub Actions workflow for the devops-demo project and **compare** it with your CircleCI workflow

---

## Prerequisites

Before starting this lesson, you should have:
- Completed Lesson 4.7 (Continuous Integration with CircleCI)
- A personal GitHub account
- A Docker Hub account
- Java 21, Maven and Git installed

---

## Introduction

In Lesson 4.7, you built a CI pipeline with CircleCI. But CircleCI is just one of many CI/CD tools available today.

**Key Questions:**
- Why are there so many CI/CD tools?
- How are they different from each other?
- How do you choose the right tool for your project?

The good news: every CI/CD tool is built on the same ideas you already know. Once you understand **what type** of tool you need, picking a specific tool becomes much easier.

---

## Part 1 - Understanding the CI/CD Tool Landscape

### Same Concepts, Different Tools

Every CI/CD tool does the same basic job:

```
Code pushed to Git
   ↓
Tool detects the change (trigger)
   ↓
Jobs run on a machine called a runner (build → test → publish)
   ↓
Results reported back (pass / fail)
```

They all use the same building blocks: **triggers**, **jobs**, **steps**, **runners** and **secrets**. What changes from tool to tool is:
- **Who runs the tool** - your Git platform, a separate company, or you
- **The configuration syntax** - the file name and the keywords

### Three Types of CI/CD Tools

The most useful way to compare CI/CD tools is by **who runs the tool and where it lives**.

```
┌──────────────────────────┐  ┌──────────────────────────┐  ┌──────────────────────────┐
│   PLATFORM-INTEGRATED    │  │   MANAGED CI SERVICE     │  │       SELF-HOSTED        │
│                          │  │         (SaaS)           │  │                          │
│  CI is built into your   │  │  A separate CI company   │  │  You install and run     │
│  Git platform            │  │  runs it for you         │  │  the CI server yourself  │
│                          │  │                          │  │                          │
│  • GitHub Actions        │  │  • CircleCI              │  │  • Jenkins               │
│  • GitLab CI/CD          │  │  • Travis CI             │  │  • TeamCity              │
│  • Bitbucket Pipelines   │  │                          │  │                          │
└──────────────────────────┘  └──────────────────────────┘  └──────────────────────────┘
   Easiest to start              Works with any Git host       Most control, most work
```

---

#### 1. Platform-Integrated

CI/CD is a feature of the platform that already hosts your code.

**Examples:** GitHub Actions (GitHub), GitLab CI/CD (GitLab), Bitbucket Pipelines (Bitbucket)

**✅ Why choose it:**
- Nothing extra to sign up for or connect - it lives next to your code
- Pipeline results show up directly in commits and pull requests
- Large libraries of ready-made steps (for example, GitHub Actions has the Actions Marketplace)

**❌ Trade-offs:**
- Tied to that platform - move from GitHub to GitLab and you rewrite your pipelines

**Choose it when:** you want the simplest setup and you are happy to stay on your current Git platform. For most teams, this is the default starting point.

---

#### 2. Managed CI Service (SaaS)

A company whose main product is CI/CD. It connects to your Git platform and runs your pipelines on its own machines.

**Examples:** CircleCI (what you used in Lesson 4.7), Travis CI

**✅ Why choose it:**
- Works with more than one Git platform (GitHub, GitLab, Bitbucket)
- Focused on CI/CD, with features such as fast caching, test insights and a dedicated dashboard
- No servers to maintain

**❌ Trade-offs:**
- One more account, vendor and set of permissions to manage
- Your code and secrets are sent to a third party

**Choose it when:** you need one CI tool across different Git platforms, or you need CI-specific features that your Git platform does not offer.

---

#### 3. Self-Hosted

You install the CI server on your own machines (on-premise or in your own cloud account) and look after it yourself.

**Examples:** Jenkins, TeamCity

**✅ Why choose it:**
- Full control over where code, data and builds live
- Highly customisable (Jenkins has a very large plugin ecosystem)
- Can run on special hardware or inside a private network

**❌ Trade-offs:**
- You maintain everything: servers, upgrades, plugins, security patches and backups
- Needs people with the skills and time to run it

**Choose it when:** rules require code and builds to stay on your own infrastructure, and you have a team to maintain it.

---

> **Note - the lines can blur:** some tools offer more than one option. GitLab can be installed on your own servers (self-managed), and GitHub Actions and CircleCI both let you run jobs on your own machines (self-hosted runners). The type describes the tool's **usual** model - who runs it by default.

### How to Choose

Ask these questions in order:

```
1. Must code and builds stay on our own servers (security / compliance)?
      └─ Yes → Self-hosted (Jenkins, TeamCity)
      └─ No  ↓

2. Is all our code on one Git platform, and are we happy to stay there?
      └─ Yes → Platform-integrated (GitHub Actions on GitHub, GitLab CI/CD on GitLab)
      └─ No  ↓

3. Do we need one CI tool across several Git platforms, or CI-specific features?
      └─ Yes → Managed CI service (CircleCI, Travis CI)
```

One more question applies to every choice: **who will look after it?** A team with no DevOps engineers should avoid running its own CI server.

---

### 🏃 Activity 1 — Tool Matching Discussion (10 min)

Based on what you've just read, discuss the following with a partner:

1. You're joining a startup that keeps all its code on GitHub and has no DevOps team. Which type of tool would you recommend, and which tool?
2. A large bank tells you no code or build data can be sent to third-party cloud servers. Which type of tool is the only viable option?
3. A digital agency hosts client projects on GitHub, GitLab and Bitbucket. Why might a managed CI service suit them better than a platform-integrated tool?

> **Key takeaway:** There's no single "best" tool. Pick the **type** that fits your team, your Git platform and your rules, then pick a tool within that type.

---

## Part 2 - Hands-On: Build a GitHub Actions Workflow

In Lesson 4.7 you built a **build → test → publish** pipeline for devops-demo with CircleCI (a managed CI service). Now you will build the **same pipeline** with GitHub Actions (a platform-integrated tool), so you can compare the two side by side.

```
git push to main
   ↓
GitHub Actions starts the workflow
   ↓
build (package JAR) → test (mvn test) → publish (Docker image to Docker Hub)
```

### Step 1: Download the Project

Download the project file for this lesson. Version `v4.9.0` contains the devops-demo application and its tests, but **no CI configuration files**.

For Windows users, use the WSL terminal only. For macOS users, use your preferred terminal. Run the following commands to set up your project.

```sh
cd ~
cd playground
curl -L -O https://github.com/aie-devops/devops-demo/archive/refs/tags/v4.9.0.zip
unzip v4.9.0.zip
cd devops-demo-4.9.0
```

Verify that the tests pass:

```sh
mvn test
```

You should see `BUILD SUCCESS` and `Tests run: ... Failures: 0, Errors: 0`.

---

### Step 2: Create a New GitHub Repository

1. Go to [GitHub](https://github.com) and sign in to your **personal account**
2. Click the **+** icon in the top-right corner → **New repository**
3. Fill in the details:
   - **Owner:** your **personal username** (not an organisation)
   - **Repository name:** `devops-demo-lesson-4.9`
   - **Visibility:** Public
   - **Do NOT** add a README, .gitignore or license
4. Click **Create repository**

Keep this page open - you will need the repository URL in the next step.

---

### Step 3: Initialise Git and Push the Code

The project already includes a `.gitignore` file, so you can commit straight away.

```bash
# Initialise a new Git repository
git init

# Add and commit all files
git add .
git commit -m "Initial commit: devops-demo for Lesson 4.9"

# Connect to your GitHub repository (replace YOUR_USERNAME)
git remote add origin https://github.com/YOUR_USERNAME/devops-demo-lesson-4.9.git

# Push to the main branch
git branch -M main
git push -u origin main
```

Refresh your repository page on GitHub - you should see your code. Click the **Actions** tab: nothing has run yet, because there is no workflow file.

---

### Step 4: Create the GitHub Actions Workflow File

GitHub Actions looks for workflow files in a fixed folder: `.github/workflows/`. Every `.yml` file in that folder is a workflow.

```bash
mkdir -p .github/workflows
touch .github/workflows/ci.yml
```

Open `.github/workflows/ci.yml` in your editor. We'll build it in four pieces, matching your CircleCI config.

#### Name and Trigger

```yml
name: build_test_publish

on:
  push:
    branches:
      - main

jobs:
```

- `name:` - the workflow name shown in the Actions tab (same name as your CircleCI workflow)
- `on:` - the **trigger**: run this workflow on every push to `main`
- `jobs:` - the jobs to run (added below)

#### Job 1: Build

```yml
  # ==========================================
  # BUILD JOB - Compile and package
  # ==========================================
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v7

      - name: Set up Java 21 and cache Maven dependencies
        uses: actions/setup-java@v6
        with:
          distribution: temurin
          java-version: '21'
          cache: maven

      - name: Install dependencies and build
        run: |
          echo "Building the application..."
          mvn clean install -DskipTests

      - name: Save JAR for the publish job
        uses: actions/upload-artifact@v7
        with:
          name: app-jar
          path: target/*.jar
```

**Understanding the build job:**
- `runs-on: ubuntu-latest` - the **runner**: a fresh Ubuntu virtual machine provided by GitHub
- `uses:` - runs an **action**, a ready-made step published by someone else
  - `actions/checkout` - downloads your code (same as CircleCI's `checkout`)
  - `actions/setup-java` - installs Java 21 and caches Maven dependencies (replaces CircleCI's `cimg/openjdk:21.0` image plus `restore_cache` / `save_cache`)
  - `actions/upload-artifact` - saves the JAR so a later job can use it (replaces `persist_to_workspace`)
- `run:` - runs a shell command (same as CircleCI's `run`)

#### Job 2: Test

```yml
  # ==========================================
  # TEST JOB - Run automated tests
  # ==========================================
  test:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Checkout code
        uses: actions/checkout@v7

      - name: Set up Java 21 and cache Maven dependencies
        uses: actions/setup-java@v6
        with:
          distribution: temurin
          java-version: '21'
          cache: maven

      - name: Run tests
        run: |
          echo "Running tests..."
          mvn test

      - name: Save test reports
        if: always()
        uses: actions/upload-artifact@v7
        with:
          name: test-reports
          path: target/surefire-reports
```

**Understanding the test job:**
- `needs: build` - only start after the build job succeeds (same as CircleCI's `requires: - build`)
- Each job runs on a **new** runner, so we check out the code and set up Java again - just like CircleCI
- `if: always()` - save the test reports even when tests fail, so you can see what went wrong (similar to CircleCI's `store_test_results` / `store_artifacts`)

#### Job 3: Publish

```yml
  # ==========================================
  # PUBLISH JOB - Build and push Docker image
  # ==========================================
  publish:
    runs-on: ubuntu-latest
    needs: test
    env:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_TOKEN: ${{ secrets.DOCKER_TOKEN }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v7

      - name: Get JAR from the build job
        uses: actions/download-artifact@v8
        with:
          name: app-jar
          path: target

      - name: Build Docker image
        run: |
          echo "Building Docker image..."
          docker build -t $DOCKER_USERNAME/devops-demo:latest .

      - name: Push to Docker Hub
        run: |
          echo "Logging in to Docker Hub..."
          echo $DOCKER_TOKEN | docker login -u $DOCKER_USERNAME --password-stdin
          echo "Pushing image to Docker Hub..."
          docker push $DOCKER_USERNAME/devops-demo:latest
```

**Understanding the publish job:**
- `needs: test` - only publish if the tests pass
- `env:` - reads the Docker Hub credentials from **GitHub secrets** (you'll add these in Step 5) and makes them available as `$DOCKER_USERNAME` and `$DOCKER_TOKEN`, so the commands are exactly the same as in CircleCI
- `actions/download-artifact` - puts the JAR back into `target/`, where the Dockerfile expects it (replaces `attach_workspace`)
- No `setup_remote_docker` needed - Docker is already installed on GitHub's Ubuntu runners

---

### Complete ci.yml File

Here's your complete `.github/workflows/ci.yml`:

```yml
name: build_test_publish

on:
  push:
    branches:
      - main

jobs:
  # ==========================================
  # BUILD JOB - Compile and package
  # ==========================================
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v7

      - name: Set up Java 21 and cache Maven dependencies
        uses: actions/setup-java@v6
        with:
          distribution: temurin
          java-version: '21'
          cache: maven

      - name: Install dependencies and build
        run: |
          echo "Building the application..."
          mvn clean install -DskipTests

      - name: Save JAR for the publish job
        uses: actions/upload-artifact@v7
        with:
          name: app-jar
          path: target/*.jar

  # ==========================================
  # TEST JOB - Run automated tests
  # ==========================================
  test:
    runs-on: ubuntu-latest
    needs: build

    steps:
      - name: Checkout code
        uses: actions/checkout@v7

      - name: Set up Java 21 and cache Maven dependencies
        uses: actions/setup-java@v6
        with:
          distribution: temurin
          java-version: '21'
          cache: maven

      - name: Run tests
        run: |
          echo "Running tests..."
          mvn test

      - name: Save test reports
        if: always()
        uses: actions/upload-artifact@v7
        with:
          name: test-reports
          path: target/surefire-reports

  # ==========================================
  # PUBLISH JOB - Build and push Docker image
  # ==========================================
  publish:
    runs-on: ubuntu-latest
    needs: test
    env:
      DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
      DOCKER_TOKEN: ${{ secrets.DOCKER_TOKEN }}

    steps:
      - name: Checkout code
        uses: actions/checkout@v7

      - name: Get JAR from the build job
        uses: actions/download-artifact@v8
        with:
          name: app-jar
          path: target

      - name: Build Docker image
        run: |
          echo "Building Docker image..."
          docker build -t $DOCKER_USERNAME/devops-demo:latest .

      - name: Push to Docker Hub
        run: |
          echo "Logging in to Docker Hub..."
          echo $DOCKER_TOKEN | docker login -u $DOCKER_USERNAME --password-stdin
          echo "Pushing image to Docker Hub..."
          docker push $DOCKER_USERNAME/devops-demo:latest
```

> **Tip:** YAML is sensitive to indentation. Use spaces, not tabs, and make sure `build:`, `test:` and `publish:` line up under `jobs:`.

---

### Step 5: Add Docker Hub Secrets in GitHub

The publish job needs your Docker Hub username and a personal access token. In CircleCI you stored these in the `docker-hub` **context**. In GitHub Actions you store them as **repository secrets**.

**A. Create a Docker Hub personal access token**

Create a new token for GitHub Actions rather than reusing your CircleCI token - one token per tool means you can revoke one without breaking the other.

1. Log in to [Docker Hub](https://hub.docker.com/) and open **Account settings**
2. Click **Personal access tokens** → **Generate new token**
3. Description: `github-actions-devops-demo`
4. Expiration date: `90 days`
5. Access permissions: `Repo Read & Write`
6. Click **Generate** and copy the token (you won't see it again)

**B. Add the secrets to your GitHub repository**

1. Open your `devops-demo-lesson-4.9` repository on GitHub
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret** and add:

| Name | Value |
|------|-------|
| `DOCKER_USERNAME` | Your Docker Hub username (e.g. `john123`) |
| `DOCKER_TOKEN` | The personal access token you just created |

Secret names are case-sensitive and must match the names used in `ci.yml` exactly.

---

### Step 6: Push the Workflow and Watch It Run

```bash
git add .github/workflows/ci.yml
git commit -m "Add GitHub Actions workflow"
git push
```

Pushing to `main` triggers the workflow:

1. Open your repository on GitHub and click the **Actions** tab
2. Click the running workflow **build_test_publish**
3. Watch the three jobs run in order: **build** → **test** → **publish**
4. Click any job to see the log output of each step

**All three jobs should show green checkmarks!** ✅

Scroll to the bottom of the workflow summary page - under **Artifacts** you'll find `app-jar` and `test-reports`.

---

### Step 7: Verify on Docker Hub

1. Go to [Docker Hub](https://hub.docker.com/) and log in
2. Open your `devops-demo` repository
3. You should see a new push of the `latest` tag

> **Note:** This is the same image name your CircleCI pipeline pushed in Lesson 4.7, so this push replaces it. That's expected - both pipelines build the same application.

**Success!** You now have the same pipeline running on two different CI tools. 🎉

---

### Troubleshooting

**Issue 1: Nothing appears in the Actions tab**

- The file must be at exactly `.github/workflows/ci.yml` (note the dot in `.github` and the `s` in `workflows`)
- Check you pushed to `main` (`git branch` should show `* main`)
- Check the YAML is valid - GitHub shows an error at the top of the Actions tab if it can't read the file

**Issue 2: `git push` is rejected with "refusing to allow a Personal Access Token to create or update workflow ... without `workflow` scope"**

Your GitHub personal access token can push code but not workflow files. Create a new token with the **workflow** scope (classic token) or **Workflows: Read and write** permission (fine-grained token), then push again.

**Issue 3: Publish job fails at Docker login (`unauthorized` or `username is empty`)**

- Check both secrets exist under **Settings** → **Secrets and variables** → **Actions**
- Check the names are exactly `DOCKER_USERNAME` and `DOCKER_TOKEN`
- Check the token has not expired and has `Repo Read & Write` permission
- After fixing, click **Re-run jobs** → **Re-run failed jobs** on the workflow page

**Issue 4: Publish job fails at `docker build` with "no source files were specified"**

The JAR wasn't downloaded into `target/`. Check that the `name:` in `upload-artifact` and `download-artifact` are both `app-jar`, and that `download-artifact` has `path: target`.

**Issue 5: Test job fails**

Run `mvn test` locally, fix the failing test, then commit and push again.

---

## Part 3 - Comparing CircleCI and GitHub Actions

Put your Lesson 4.7 `.circleci/config.yml` next to your new `.github/workflows/ci.yml`. The pipeline is the same - only the syntax and where it runs are different.

| Concept | CircleCI (managed CI service) | GitHub Actions (platform-integrated) |
|---------|-------------------------------|--------------------------------------|
| Config file | `.circleci/config.yml` | `.github/workflows/ci.yml` |
| Trigger | Set up when you connect the project in CircleCI | `on: push` in the workflow file |
| Runner / environment | `docker: - image: cimg/openjdk:21.0` | `runs-on: ubuntu-latest` + `actions/setup-java` |
| Get the code | `- checkout` | `uses: actions/checkout` |
| Run a command | `- run:` | `- run:` |
| Cache dependencies | `restore_cache` / `save_cache` | `cache: maven` |
| Share files between jobs | `persist_to_workspace` / `attach_workspace` | `upload-artifact` / `download-artifact` |
| Docker | `setup_remote_docker` | Already on the runner |
| Job order | `workflows:` → `requires:` | `needs:` on each job |
| Secrets | Context `docker-hub` | Repository secrets |
| Where you watch it | CircleCI dashboard (separate site) | **Actions** tab in your repository |

**What's the same:**
- YAML configuration stored in the repository
- Build → test → publish jobs, each on a fresh machine
- Jobs depend on earlier jobs, so a failed test stops the publish
- Secrets are stored in the tool, never in the code

**What's different:**
- GitHub Actions lives inside GitHub - no separate account or project to connect
- GitHub Actions builds pipelines from reusable **actions** (`uses:`)
- CircleCI keeps the job order in one `workflows:` block; GitHub Actions puts `needs:` on each job
- CircleCI also works with GitLab and Bitbucket; GitHub Actions only runs for GitHub repositories

---

### 🏃 Activity 2 — Compare Your Two Pipelines (15 min)

**Part A - Break the pipeline on purpose**

1. In `DemoController.java`, change the message to `"DevOps demo - GitHub Actions is working!"` but **do not** update the test
2. Commit and push to `main`
3. In the Actions tab, watch what happens to the **test** and **publish** jobs
4. Download the `test-reports` artifact and find the failure message
5. Update `DemoControllerTest.java` to expect the new message, run `mvn test` locally, then commit and push again - all three jobs should pass

**Part B - Discuss with a partner**

1. Which config file was easier to read? Which keywords map directly between the two?
2. Why does CircleCI need `setup_remote_docker` but GitHub Actions does not?
3. Your company moves its code from GitHub to GitLab. Which pipeline still works, and which would you have to rewrite? What does this tell you about the trade-off of platform-integrated tools?

> **Key takeaway:** Different tools, same concepts. Once you understand CI/CD, switching tools is mostly learning new syntax.

---

## Part 4 - Hands-On Activity: Tool Selection

**Format:** Small groups (3-4 students)

### Instructions

You will receive 4 company scenario cards. For each scenario:
1. Read the company requirements
2. Decide which **type** of CI/CD tool fits best, then pick a tool
3. Justify your choice with 2-3 reasons

---

### Scenario 1: Early-Stage Startup

**Company:** MobileApp Inc.
**Team Size:** 5 developers
**Tech Stack:** React Native, Node.js
**Requirements:**
- Need to launch an MVP in 3 months
- No DevOps engineers
- All code is on GitHub

**Question:** Which type of CI/CD tool should they use, and which tool?

<details>
<summary>Suggested Answer</summary>

**Best Choice:** Platform-integrated - GitHub Actions

**Reasons:**
1. Their code is already on GitHub - nothing extra to set up
2. No servers to maintain, which suits a team with no DevOps engineers
3. Fast to get started, so the team can focus on the MVP

</details>

---

### Scenario 2: Financial Services Company

**Company:** SecureBank Corp.
**Team Size:** 50 developers
**Tech Stack:** Java, Spring Boot, Oracle DB
**Requirements:**
- Strict security and compliance rules
- Code and build data cannot leave the company's own servers
- Dedicated DevOps team (5 people)

**Question:** Which type of CI/CD tool should they use, and which tool?

<details>
<summary>Suggested Answer</summary>

**Best Choice:** Self-hosted - Jenkins (or TeamCity)

**Reasons:**
1. Runs on their own secure servers, so no data leaves their infrastructure
2. Highly customisable to meet security and compliance requirements
3. They have a DevOps team to install, maintain and secure it

</details>

---

### Scenario 3: Growing SaaS Company

**Company:** ProjectTool SaaS
**Team Size:** 20 developers
**Tech Stack:** Python, React, PostgreSQL
**Requirements:**
- All code is on GitLab
- Want issue tracking, code and CI/CD in one platform
- Want as few tools to manage as possible

**Question:** Which type of CI/CD tool should they use, and which tool?

<details>
<summary>Suggested Answer</summary>

**Best Choice:** Platform-integrated - GitLab CI/CD

**Reasons:**
1. Already using GitLab - CI/CD is built in
2. One platform for issues, code, pipelines and container registry
3. No extra accounts or integrations to look after

</details>

---

### Scenario 4: Digital Agency

**Company:** PixelWorks Agency
**Team Size:** 15 developers
**Tech Stack:** Java, Node.js, mobile apps
**Requirements:**
- Client projects are spread across GitHub, GitLab and Bitbucket (each client chooses)
- Want one CI tool and one dashboard for every project
- No time to maintain their own servers

**Question:** Which type of CI/CD tool should they use, and which tool?

<details>
<summary>Suggested Answer</summary>

**Best Choice:** Managed CI service - CircleCI

**Reasons:**
1. Connects to GitHub, GitLab and Bitbucket, so every project uses the same tool
2. One dashboard and one config style across all clients
3. Runs on the provider's machines - no servers to maintain

</details>

---

## Part 5 - When to Switch CI/CD Tools

### Signs You Might Need to Switch

**1. Managed or platform-integrated → Self-hosted (e.g. CircleCI → Jenkins):**
- New security or compliance rules require builds on your own servers
- You need full control or special hardware

**2. Self-hosted → Managed or platform-integrated (e.g. Jenkins → GitHub Actions):**
- The team can no longer keep up with maintaining the CI server
- You want to spend less time on upkeep and more on delivery

**3. One platform to another (e.g. GitLab CI/CD → GitHub Actions):**
- The company moves its code to a different Git platform
- You want CI/CD that lives right next to your code

### Migration Considerations

**Before switching, consider:**
1. **Learning curve** - the team needs to learn the new tool
2. **Migration effort** - every pipeline must be rewritten
3. **Features** - the new tool supports everything you rely on
4. **Integrations** - it works with your existing tools

**Rule of thumb:** Don't switch tools just for fun. Switch when the benefits clearly outweigh the cost of migrating.

---

## Summary

### What You Learned

1. ✅ The three types of CI/CD tools: platform-integrated, managed CI services and self-hosted
2. ✅ How to choose a type of tool, and a tool, for a given use case
3. ✅ How to build a GitHub Actions workflow for devops-demo
4. ✅ How CircleCI and GitHub Actions express the same pipeline in different syntax

### Key Takeaways

- **Pick the type first, then the tool.** Who should run it, and where does your code live?
- **Platform-integrated** (GitHub Actions, GitLab CI/CD) - simplest; tied to your Git platform
- **Managed CI service** (CircleCI, Travis CI) - works across Git platforms; one more vendor
- **Self-hosted** (Jenkins, TeamCity) - full control; you maintain everything
- **The concepts transfer.** Triggers, jobs, steps, runners and secrets exist in every tool

---

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [CircleCI Documentation](https://circleci.com/docs/)
- [GitLab CI/CD Documentation](https://docs.gitlab.com/ci/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
