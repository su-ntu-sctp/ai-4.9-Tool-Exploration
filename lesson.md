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
  image: maven:3.9-openjdk-21
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
│                   └── HelloController.java
├── Dockerfile
├── pom.xml
└── .gitlab-ci.yml
```

---

### Step 1: Simple Spring Boot Application

**HelloController.java:**
```java
package com.example;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class HelloController {

    public static void main(String[] args) {
        SpringApplication.run(HelloController.class, args);
    }

    @GetMapping("/hello")
    public String hello() {
        return "Hello from GitLab CI/CD Demo!";
    }
}
```

**Dockerfile:**
```dockerfile
# Use Maven to build the application
FROM maven:3.9-openjdk-21 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

# Run the application
FROM openjdk:21-jdk-slim
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
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

### Step 2: GitLab CI/CD Configuration

**`.gitlab-ci.yml` (compare with your CircleCI config):**

```yml
# Define the stages of the pipeline
stages:
  - build
  - test
  - publish

# Define variables
variables:
  MAVEN_OPTS: "-Dmaven.repo.local=$CI_PROJECT_DIR/.m2/repository"

# Cache Maven dependencies
cache:
  paths:
    - .m2/repository

# Build job - compile and package the application
build-job:
  stage: build
  image: maven:3.9-openjdk-21
  script:
    - echo "Building the application..."
    - mvn clean install -DskipTests
  artifacts:
    paths:
      - target/*.jar
    expire_in: 1 hour

# Test job - run unit tests
test-job:
  stage: test
  image: maven:3.9-openjdk-21
  script:
    - echo "Running tests..."
    - mvn test
  artifacts:
    reports:
      junit: target/surefire-reports/TEST-*.xml

# Publish job - build and push Docker image
publish-job:
  stage: publish
  image: docker:latest
  services:
    - docker:dind
  before_script:
    - docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD
  script:
    - echo "Building Docker image..."
    - docker build -t $DOCKER_USERNAME/hello-cicd-app:latest .
    - echo "Pushing to Docker Hub..."
    - docker push $DOCKER_USERNAME/hello-cicd-app:latest
  only:
    - main
```

**Note:** The `$DOCKER_USERNAME` and `$DOCKER_PASSWORD` variables need to be configured in GitLab:
- Go to Settings → CI/CD → Variables
- Add `DOCKER_USERNAME` (your Docker Hub username)
- Add `DOCKER_PASSWORD` (your Docker Hub password, mark as Protected and Masked)

---

### Step 3: Comparing GitLab CI/CD with CircleCI

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

### Step 4: Running the Pipeline

**Instructor will demonstrate:**

1. **Push code to GitLab**
   ```bash
   git remote add gitlab git@gitlab.com:username/hello-cicd-app.git
   git push gitlab main
   ```

2. **Pipeline triggers automatically**
   - GitLab detects `.gitlab-ci.yml`
   - Starts running the pipeline

3. **Watch the pipeline**
   - Navigate to: CI/CD → Pipelines
   - See build → test → publish running in sequence

4. **View logs**
   - Click on each job to see output
   - Compare with CircleCI logs you saw in Lesson 7

---

### Step 5: Key Observations

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

### Key Takeaways

**1. Core concepts are universal:**
- Build → Test → Publish pipeline
- YAML/config-based definitions
- Jobs, stages, workflows
- Environment variables for secrets

**2. Choose tools based on context:**
- Team size and expertise
- Infrastructure (cloud vs on-premise)
- Budget and licensing
- Existing tech stack

**3. Don't overthink it:**
- For learning: Any popular tool works (CircleCI, GitLab, GitHub Actions)
- For startups: Choose easy cloud-based tools
- For enterprises: Consider security and customization needs

**4. You can always switch:**
- Core skills transfer between tools
- Migration is possible (though has cost)
- Many companies use multiple tools

---

## Next Steps

In the next lesson (Lesson 10: Hosting Options), we'll explore:
- Different platforms for hosting applications (like Render, Railway, Fly.io)
- Hosting databases (PostgreSQL options)
- Comparing hosting platforms

Then in Lesson 12, you'll set up Continuous Deployment (CD) using CircleCI to automatically deploy your devops-demo application!

---

## Additional Resources

### Official Documentation
- [CircleCI Docs](https://circleci.com/docs/)
- [GitLab CI/CD Docs](https://docs.gitlab.com/ee/ci/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
- [Azure DevOps Docs](https://learn.microsoft.com/en-us/azure/devops/)

### Comparison Articles
- [CI/CD Tools Comparison 2024](https://www.jetbrains.com/teamcity/ci-cd-guide/ci-cd-tools/)
- [Choosing the Right CI/CD Tool](https://www.digitalocean.com/community/tutorials/ci-cd-tools-comparison-jenkins-gitlab-ci-circle-ci-travis-ci-codeship-teamcity-bamboo)

### Video Tutorials
- [GitLab CI/CD in 20 Minutes](https://www.youtube.com/watch?v=qP8kir2GUgo)
- [Jenkins vs GitLab CI vs GitHub Actions](https://www.youtube.com/watch?v=3a8KsB5wJDE)

---

## Glossary

**Pipeline:** A series of automated steps that code goes through from commit to deployment

**Stage:** A logical grouping of jobs (e.g., build stage, test stage)

**Job:** A single task within a pipeline (e.g., compile code, run tests)

**Runner:** A server that executes CI/CD jobs

**Artifact:** Output from a job that's saved and passed to other jobs

**Self-Hosted:** Software that you install and run on your own servers

**SaaS (Software as a Service):** Cloud-based software that you access via internet

**On-Premise:** Software installed and run on your company's own infrastructure

---



**Great job!** You now understand the CI/CD tool landscape and can make informed decisions about which tool to use for different projects.