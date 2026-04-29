# 4.9 Self Studies

**Estimated Preparation Time:** 65 minutes

---

> **📌 Note:** The videos in these self studies are selected to help you build familiarity with the tools and concepts before class. The approach, project structure, and commands used in the videos may differ from what we do in the lesson. Focus on understanding the **concepts and the why** — not on replicating the video step by step. The lesson content is what you should follow during class.

---

## Task 1 — CI/CD Tools Comparison (25 minutes)

Watch the following video on CI/CD tools:

- 📹 [CI/CD Tools Comparison — https://www.youtube.com/watch?v=_DRtZVE9y0A]

While watching, refer to **lesson.md Part 1 and Part 2** and pay attention to:
- Why different CI/CD tools exist and what problems they solve
- How cloud-based tools differ from self-hosted tools
- What factors teams consider when choosing a CI/CD tool
- How the pipeline configuration differs across tools

**Guiding Questions:**
1. What is the main difference between a cloud-based CI/CD tool and a self-hosted one?
2. Which tool would you recommend to a team that has strict data security requirements and why?
3. What do all CI/CD tools have in common regardless of their syntax or hosting model?

---

## Task 2 — Compare Configuration Syntax (20 minutes)

No video for this task. Refer to **lesson.md Part 2** and read through the four configuration examples — CircleCI, GitLab CI/CD, Jenkins, and Azure DevOps — then answer the following:

1. Which two tools use the most similar YAML structure? What are the key differences between them?
2. Jenkins uses Groovy instead of YAML — what do you think are the advantages and disadvantages of this?
3. Looking at the GitLab CI/CD example, how does the `stages:` concept differ from CircleCI's `workflows:`?
4. If you had to switch your devops-demo project from CircleCI to GitLab CI/CD today, what parts of the config would need to change?

**Guiding Questions:**
1. Why do you think most modern CI/CD tools chose YAML over other formats?
2. What does `image:` in GitLab CI/CD correspond to in CircleCI?
3. How does GitLab CI/CD know the order jobs should run without an explicit `requires:` like CircleCI?

---

## Task 3 — Tool Selection Practice (20 minutes)

No video for this task. Read through the four company scenarios in **lesson.md Part 4** before class and for each scenario write down:

1. Which CI/CD tool you would recommend
2. Two reasons that justify your choice
3. One potential disadvantage of your chosen tool for that scenario

Come to class with your answers ready to discuss — the activity will be done in groups, so having your own thoughts prepared first will help the discussion.

**Guiding Questions:**
1. Is there ever a scenario where more than one tool could be a valid choice? How would you decide?
2. What questions would you ask a client before recommending a CI/CD tool?
3. As a DevOps engineer, why is it important to understand multiple tools rather than just one?

---

## Active Engagement Strategies

- Pause the video when a tool is being compared and try to recall what you already know about it from Lesson 4.7
- After watching, try to list the four tools and one key strength of each from memory
- For Tasks 2 and 3, write your answers before class — do not just read through the content

---

## Additional Reading Material

- [CI/CD Tools Comparison — Atlassian](https://www.atlassian.com/continuous-delivery/principles/continuous-integration-vs-delivery-vs-deployment)
- [Jenkins Overview — Jenkins Docs](https://www.jenkins.io/doc/)
- [GitLab CI/CD Overview — GitLab Docs](https://docs.gitlab.com/ee/ci/)
- [Azure DevOps Overview — Microsoft Docs](https://learn.microsoft.com/en-us/azure/devops/pipelines/get-started/what-is-azure-pipelines)