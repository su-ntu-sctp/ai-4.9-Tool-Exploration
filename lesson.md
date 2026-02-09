# Lesson 4.9: Tool Exploration - CI/CD Tools

## Learning Objectives

By the end of this lesson, you will be able to:
1. Identify different CI/CD tools available in the market
2. Compare and contrast CircleCI, GitLab CI/CD, Jenkins, and Azure DevOps
3. Understand when to use which CI/CD tool based on project requirements
4. Recognize the similarities and differences in CI/CD configurations across tools

---

## Prerequisites

Before starting this lesson, you should have:
- Completed Lesson 7 (Continuous Integration with CircleCI)
- Understanding of CI/CD concepts
- Familiarity with YAML configuration files
- Basic knowledge of Docker

---

## Introduction

In Lesson 7, you learned about Continuous Integration using CircleCI. But CircleCI is just one of many CI/CD tools available today. 

**Key Questions:**
- Why are there so many CI/CD tools?
- How do they differ?
- How do you choose the right tool for your project?

In this lesson, we'll explore alternative CI/CD tools and understand their strengths and trade-offs.

---

## Part 1 - Understanding the CI/CD Tool Landscape

### Why Multiple CI/CD Tools Exist

Different organizations have different needs:

**1. Company Size:**
- Startups need quick, easy setup (CircleCI, GitLab CI/CD)
- Large enterprises need control and customization (Jenkins, Azure DevOps)

**2. Infrastructure:**
- Cloud-first companies prefer SaaS tools (CircleCI, GitLab CI/CD)
- Companies with security requirements need on-premise (Jenkins)

**3. Existing Tech Stack:**
- GitHub users → GitHub Actions
- GitLab users → GitLab CI/CD
- Microsoft shops → Azure DevOps
- Java/Enterprise → Jenkins

**4. Budget:**
- Open-source/free → Jenkins, GitLab (self-hosted)
- Paid SaaS → CircleCI, Azure DevOps (premium features)

### Categories of CI/CD Tools

**1. Cloud-Based SaaS:**
- CircleCI, GitLab CI/CD, GitHub Actions, Travis CI
- Easy setup, no maintenance
- Pay per usage or monthly plans

**2. Self-Hosted / On-Premise:**
- Jenkins, GitLab (self-hosted), TeamCity
- Full control, host on your servers
- Require maintenance and setup

**3. Platform-Integrated:**
- GitLab CI/CD (integrated with GitLab)
- GitHub Actions (integrated with GitHub)
- Bitbucket Pipelines (integrated with Bitbucket)

---

## Part 2 - Comparing Four Popular CI/CD Tools

Let's compare four widely-used CI/CD tools:

### Tool Overview

| Tool | Type | Best For | Free Tier | Difficulty |
|------|------|----------|-----------|------------|
| **CircleCI** | Cloud SaaS | Startups, quick setup | 6,000 build minutes/month | Easy |
| **GitLab CI/CD** | Cloud/Self-hosted | Teams using GitLab | 400 minutes/month | Easy |
| **Jenkins** | Self-hosted | Enterprises, customization | Unlimited (self-hosted) | Medium-Hard |
| **Azure DevOps** | Cloud SaaS | Microsoft ecosystem | 1,800 minutes/month | Medium |

---

### Detailed Comparison

#### 1. CircleCI (What You're Using)

**✅ Pros:**
- Very easy to set up (you experienced this!)
- Great documentation and UI
- Fast build times with caching
- Docker support built-in
- Good free tier for small teams

**❌ Cons:**
- Can get expensive for larger teams
- Limited customization compared to Jenkins
- Depends on CircleCI's infrastructure

**Best Use Cases:**
- Startups and small-to-medium teams
- Projects needing quick CI/CD setup
- Teams wanting managed infrastructure

**Configuration File:** `.circleci/config.yml`

**Example (you've already used this):**
```yml
version: 2.1
jobs:
  build:
    docker:
      - image: cimg/openjdk:21.0
    steps:
      - checkout
      - run: mvn clean install
```

---

#### 2. GitLab CI/CD

**✅ Pros:**
- Integrated with GitLab (no account linking needed)
- Works with both cloud and self-hosted GitLab
- Built-in container registry
- Auto DevOps features
- Extensive free tier

**❌ Cons:**
- Requires GitLab as your Git platform
- Free tier minutes more limited than CircleCI
- Less popular than GitHub ecosystem

**Best Use Cases:**
- Teams already using GitLab for version control
- Organizations wanting all-in-one DevOps platform
- Projects needing integrated container registry

**Configuration File:** `.gitlab-ci.yml`

**Example (very similar to CircleCI):**
```yml
stages:
  - build
  - test

build-job:
  stage: build
  image: maven:3.9-eclipse-temurin-21
  script:
    - mvn clean install
```

**Key Difference from CircleCI:**
- Uses `stages` instead of `workflows`
- Jobs are grouped by stages
- Built-in to GitLab (no separate account)

---

#### 3. Jenkins

**✅ Pros:**
- Most popular CI/CD tool (huge community)
- Completely free and open-source
- Unlimited build minutes (you host it)
- Extremely customizable with 1,800+ plugins
- Can run on your own servers (security/compliance)

**❌ Cons:**
- Requires server setup and maintenance
- Steeper learning curve
- UI feels dated compared to modern tools
- You manage updates, security, backups

**Best Use Cases:**
- Large enterprises with dedicated DevOps teams
- Companies with strict security/compliance needs
- Organizations wanting full control
- Teams with existing Jenkins expertise

**Configuration File:** `Jenkinsfile`

**Example (more complex syntax):**
```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
```

**Key Differences:**
- Uses Groovy language (not YAML)
- Requires Jenkins server installation
- More verbose configuration

---

#### 4. Azure DevOps

**✅ Pros:**
- Integrated with Microsoft ecosystem (Azure, Visual Studio)
- Combines CI/CD, boards, repos, artifacts
- Good Windows/.NET support
- Generous free tier for small teams
- Enterprise features built-in

**❌ Cons:**
- Overkill if not using Microsoft stack
- More complex than CircleCI/GitLab
- Less popular for non-Microsoft projects

**Best Use Cases:**
- Companies using Azure cloud
- .NET/C# projects
- Organizations wanting comprehensive ALM tool
- Teams needing boards + CI/CD + repos together

**Configuration File:** `azure-pipelines.yml`

**Example:**
```yml
trigger:
  - main

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
    goals: 'clean install'
```

**Key Differences:**
- Different syntax (tasks instead of steps)
- Tight Azure integration
- More enterprise-focused features

---

### Quick Comparison Table

| Feature | CircleCI | GitLab CI/CD | Jenkins | Azure DevOps |
|---------|----------|--------------|---------|--------------|
| **Setup Time** | 5 minutes | 5 minutes | 1-2 hours | 15 minutes |
| **Hosting** | Cloud only | Cloud or self-hosted | Self-hosted | Cloud only |
| **Language** | YAML | YAML | Groovy | YAML |
| **Docker Support** | Excellent | Excellent | Via plugins | Good |
| **Learning Curve** | Easy | Easy | Steep | Medium |
| **Free for Students** | Yes | Yes | Yes (unlimited) | Yes |
| **Requires Server** | No | No | Yes | No |
| **Best For** | Startups | GitLab users | Enterprises | Microsoft shops |

---

## Part 3 - GitLab CI/CD Demo

**Note:** This is an **instructor-led demonstration**. You will observe and compare with your CircleCI experience. You don't need to set this up yourself.

### Demo Application

For this demo, we'll use a simple Spring Boot application called **hello-cicd-app**.

**Project Structure:**
```
hello-cicd-app/
├── src/
│   └── main/
│       └── java/
│           └── com/
│               └── example/
│                   └── hellocicdapp/
│                       ├── HelloCicdAppApplication.java
│                       └── controller/
│                           └── HelloController.java
├── Dockerfile
├── .dockerignore
├── pom.xml
└── .gitlab-ci.yml
```

---

### Step 1: Simple Spring Boot Application

**HelloCicdAppApplication.java (Main Application Class):**
```java
package com.example.hellocicdapp;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class HelloCicdAppApplication {

    public static void main(String[] args) {
        SpringApplication.run(HelloCicdAppApplication.class, args);
    }
}
```

**HelloController.java (REST Controller):**
```java
package com.example.hellocicdapp.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello from GitLab CI/CD Demo!";
    }
}
```

**Dockerfile:**
```dockerfile
# Build stage
FROM eclipse-temurin:21-jdk-alpine AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN apk add --no-cache maven
RUN mvn clean package -DskipTests

# Runtime stage
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
```

**.dockerignore:**
```
# Maven
target/
!target/*.jar
.mvn/
mvnw
mvnw.cmd

# Git
.git/
.gitignore

# IDE
.idea/
*.iml
.vscode/
.settings/
.classpath
.project

# OS
.DS_Store
Thumbs.db

# Logs
*.log

# Docker
Dockerfile
docker-compose.yml
.dockerignore
```

**pom.xml:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>hello-cicd-app</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <name>Hello CI/CD App</name>
    <description>Simple Spring Boot app for CI/CD tool demonstration</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
        <relativePath/>
    </parent>

    <properties>
        <java.version>21</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

---

### Step 2: Test Docker Build Locally (Before GitLab)

**Before setting up GitLab CI/CD, verify everything works locally:**

```bash
# Step 1: Build the JAR file
mvn clean package -DskipTests

# Step 2: Build Docker image
docker build -t hello-cicd-app .

# Step 3: Run container
docker run -d -p 8080:8080 --name hello-cicd hello-cicd-app

# Step 4: Test endpoint
curl http://localhost:8080/hello
# Should see: "Hello from GitLab CI/CD Demo!"

# Step 5: Cleanup
docker stop hello-cicd
docker rm hello-cicd
```

**If this works, proceed to GitLab setup. If not, fix issues before continuing.**

---

### Step 3: GitLab CI/CD Configuration

**`.gitlab-ci.yml` (compare with your CircleCI config):**

```yml
# GitLab CI/CD Pipeline Configuration
# This pipeline has 3 stages: build, test, publish

stages:
  - build
  - test
  - publish

# Variables used across all jobs
variables:
  MAVEN_OPTS: "-Dmaven.repo.local=$CI_PROJECT_DIR/.m2/repository"

# Cache Maven dependencies to speed up builds
cache:
  paths:
    - .m2/repository

# ==========================================
# BUILD JOB - Compile and package
# ==========================================
build:
  stage: build
  image: maven:3.9-eclipse-temurin-21
  script:
    - echo "Building the application..."
    - mvn clean install -DskipTests
  artifacts:
    paths:
      - target/*.jar
    expire_in: 1 hour

# ==========================================
# TEST JOB - Run automated tests
# ==========================================
test:
  stage: test
  image: maven:3.9-eclipse-temurin-21
  script:
    - echo "Running tests..."
    - mvn test
  artifacts:
    reports:
      junit: target/surefire-reports/TEST-*.xml

# ==========================================
# PUBLISH JOB - Build and push Docker image
# ==========================================
publish:
  stage: publish
  image: docker:latest
  services:
    - docker:dind
  before_script:
    - echo "Logging in to Docker Hub..."
    - docker login -u "$DOCKER_USERNAME" -p "$DOCKER_PASSWORD"
  script:
    - echo "Building Docker image..."
    - docker build -t $DOCKER_USERNAME/hello-cicd-app:latest .
    - echo "Pushing to Docker Hub..."
    - docker push $DOCKER_USERNAME/hello-cicd-app:latest
```

---

### Step 4: GitLab Setup Instructions

**A. Create GitLab Account and Repository**

1. Go to [https://gitlab.com](https://gitlab.com)
2. Click **"Register now"** and sign up
3. Verify your email address
4. Click **"Create a project"** → **"Create blank project"**
5. Enter:
   - **Project name:** `hello-cicd-app`
   - **Visibility:** Public
   - **Initialize with README:** UNCHECKED
6. Click **"Create project"**

---

**B. Add Environment Variables in GitLab**

1. In your GitLab project, go to **Settings** → **CI/CD**
2. Find **"Variables"** section → Click **"Expand"**
3. Click **"Add variable"**

**Variable 1:**
- **Key:** `DOCKER_USERNAME`
- **Value:** Your Docker Hub username
- **Environments:** All (default)
- **Protect variable:** UNCHECKED ❌
- **Mask variable:** UNCHECKED
- Click **"Add variable"**

**Variable 2:**
- **Key:** `DOCKER_PASSWORD`
- **Value:** Your Docker Hub password
- **Environments:** All (default)
- **Protect variable:** UNCHECKED ❌
- **Mask variable:** CHECKED ✅
- Click **"Add variable"**

**⚠️ CRITICAL:** Make sure **"Environments"** is set to **"All (default)"**, NOT a specific environment name. If set to a specific environment, variables won't be available and pipeline will fail with "username is empty" error.

---

**C. Push Code to GitLab**

```bash
# Initialize git (if not already done)
git init

# Add all files
git add .
git commit -m "Initial commit: Spring Boot app with GitLab CI/CD"

# Add GitLab remote (replace YOUR_USERNAME)
git remote add origin https://gitlab.com/YOUR_USERNAME/hello-cicd-app.git

# Push to GitLab
git branch -M main
git push -u origin main
```

**Enter your GitLab username and password when prompted.**

---

**D. Watch Pipeline Run**

1. Go to your GitLab project
2. Click **"Build"** → **"Pipelines"** (left sidebar)
3. You should see pipeline running:
   - 🔵 build (running/passed)
   - 🔵 test (running/passed)
   - 🔵 publish (running/passed)

**Timeline:**
- Build: ~2-3 minutes
- Test: ~1-2 minutes
- Publish: ~2-3 minutes
- **Total: ~5-8 minutes**

---

**E. Verify on Docker Hub**

1. Go to [https://hub.docker.com](https://hub.docker.com)
2. Login
3. Check your repositories
4. You should see `hello-cicd-app` with tag `latest`

---

### Step 5: Troubleshooting

**Issue 1: Pipeline fails at "publish" job with "username is empty"**

**Cause:** Environment variables not set correctly in GitLab.

**Solution:**
1. Go to Settings → CI/CD → Variables
2. Check both `DOCKER_USERNAME` and `DOCKER_PASSWORD` exist
3. **CRITICAL:** Click on each variable → Check **"Environments"** field
4. If it shows a specific environment name (like "DOCKER_USERNAME"), change it to **"All (default)"**
5. Make sure **"Protect variable"** is UNCHECKED
6. Save changes
7. Retry the pipeline

**Issue 2: Pipeline fails with "manifest for maven:3.9-openjdk-21 not found"**

**Cause:** Base image doesn't exist.

**Solution:** Use `maven:3.9-eclipse-temurin-21` instead in `.gitlab-ci.yml`

---

### Step 6: Comparing GitLab CI/CD with CircleCI

**Observe these differences:**

| Aspect | CircleCI | GitLab CI/CD |
|--------|----------|--------------|
| **Stages/Workflows** | `workflows:` section | `stages:` at top level |
| **Job Definition** | Under `jobs:` | Job name with stage |
| **Dependencies** | `requires:` | Automatic by stage order |
| **Docker Images** | `docker:` in job | `image:` in job |
| **Caching** | `save_cache` / `restore_cache` | `cache:` at top level |
| **Artifacts** | `persist_to_workspace` | `artifacts:` |
| **Secrets** | Environment variables in UI | CI/CD variables in UI |

---

### Step 7: Key Observations

**What's Similar:**
- YAML-based configuration
- Jobs run in Docker containers
- Build → Test → Publish workflow
- Environment variables for secrets
- Caching for dependencies

**What's Different:**
- GitLab uses "stages" vs CircleCI "workflows"
- GitLab integrated with Git repository (no separate account)
- Different syntax for artifacts and caching
- GitLab has built-in container registry

**Lesson:** Different tools, same concepts! Once you understand CI/CD, switching tools is just learning new syntax.

---

## Part 4 - Hands-On Activity: Tool Selection

**Time:** 15 minutes  
**Format:** Small groups (3-4 students)

### Instructions

You will receive 4 company scenario cards. For each scenario:
1. Read the company requirements
2. Discuss which CI/CD tool fits best
3. Justify your choice with 2-3 reasons

---

### Scenario 1: Early-Stage Startup

**Company:** MobileApp Inc.  
**Team Size:** 5 developers  
**Tech Stack:** React Native, Node.js  
**Requirements:**
- Need to launch MVP in 3 months
- Limited DevOps expertise
- Using GitHub for code
- Budget: $100/month

**Question:** Which CI/CD tool should they use?

<details>
<summary>Suggested Answer</summary>

**Best Choice:** CircleCI or GitHub Actions

**Reasons:**
1. Quick setup (5 minutes) - no time to manage Jenkins
2. Good free tier for small team
3. Integrates with GitHub (already using it)
4. Minimal DevOps knowledge needed
5. Cloud-based (no server maintenance)

</details>

---

### Scenario 2: Financial Services Company

**Company:** SecureBank Corp.  
**Team Size:** 50 developers  
**Tech Stack:** Java, Spring Boot, Oracle DB  
**Requirements:**
- Strict security and compliance rules
- Cannot use cloud-based CI/CD (data security)
- Need to host everything on-premise
- Have dedicated DevOps team (5 people)

**Question:** Which CI/CD tool should they use?

<details>
<summary>Suggested Answer</summary>

**Best Choice:** Jenkins (self-hosted)

**Reasons:**
1. Can host on their own secure servers
2. No data leaves their infrastructure
3. Free and open-source (no licensing costs)
4. Highly customizable for security requirements
5. Have DevOps team to manage and maintain it
6. Proven in enterprise/banking environments

</details>

---

### Scenario 3: Growing SaaS Company

**Company:** ProjectTool SaaS  
**Team Size:** 20 developers  
**Tech Stack:** Python, React, PostgreSQL  
**Requirements:**
- Using GitLab for version control
- Need integrated issue tracking + CI/CD
- Want one platform for everything
- Budget: $300/month

**Question:** Which CI/CD tool should they use?

<details>
<summary>Suggested Answer</summary>

**Best Choice:** GitLab CI/CD

**Reasons:**
1. Already using GitLab for version control
2. Integrated issue tracking, CI/CD, and container registry
3. No need to link multiple tools/accounts
4. One platform = easier management
5. Good free tier, can upgrade as they grow
6. Built-in features for the full DevOps lifecycle

</details>

---

### Scenario 4: Microsoft-Focused Enterprise

**Company:** EnterpriseApps Ltd.  
**Team Size:** 100 developers  
**Tech Stack:** C#, .NET, SQL Server  
**Requirements:**
- Hosting on Azure Cloud
- Using Azure services (Storage, SQL, etc.)
- Need enterprise project management features
- Budget: No constraint

**Question:** Which CI/CD tool should they use?

<details>
<summary>Suggested Answer</summary>

**Best Choice:** Azure DevOps

**Reasons:**
1. Native integration with Azure services
2. Best support for .NET/C# projects
3. Combines boards, repos, pipelines, artifacts
4. Tight integration with Visual Studio
5. Enterprise features built-in
6. Single Microsoft ecosystem

</details>

---

## Part 5 - When to Switch CI/CD Tools

### Signs You Might Need to Switch

**1. From Cloud to Self-Hosted (e.g., CircleCI → Jenkins):**
- Company grows and has security/compliance needs
- Costs become too high on cloud platform
- Need more customization and control

**2. From Self-Hosted to Cloud (e.g., Jenkins → GitLab CI/CD):**
- Small DevOps team can't maintain Jenkins
- Want to reduce maintenance overhead
- Need faster setup for new projects

**3. From One Cloud to Another (e.g., CircleCI → GitHub Actions):**
- Changing Git platform (GitLab → GitHub)
- Need tighter integration with repository
- Cost optimization

### Migration Considerations

**Before switching, consider:**
1. **Learning curve** - Team needs to learn new tool
2. **Migration effort** - Converting all pipelines
3. **Cost comparison** - Total cost of ownership
4. **Feature parity** - New tool supports all needs
5. **Integration** - Works with existing tools

**Rule of thumb:** Don't switch tools just for fun. Switch when clear benefits outweigh migration costs.

---

## Summary

### What You Learned

1. ✅ CI/CD tool landscape and categories
2. ✅ Comparison of CircleCI, GitLab CI/CD, Jenkins, Azure DevOps
3. ✅ When to use which tool based on requirements
4. ✅ Similarities and differences in configurations
5. ✅ How to evaluate and select CI/CD tools





