# 4.9 Self Studies

**Estimated Preparation Time:** 65 minutes

---

> **📌 Note:** The videos in these self studies are selected to help you build familiarity with the tools and concepts before class. The approach, project structure, and commands used in the videos may differ from what we do in the lesson. Focus on understanding the **concepts and the why** — not on replicating the video step by step. The lesson content is what you should follow during class.

---

## Task 1 — The CI/CD Tool Landscape (25 minutes)

Watch the following video on CI/CD tools:

- 📹 [CI/CD Tools Comparison — https://www.youtube.com/watch?v=_DRtZVE9y0A]

While watching, refer to **lesson.md Part 1** and sort every tool you hear about into one of the three types:
- **Platform-integrated** — CI/CD built into the Git platform (e.g. GitHub Actions, GitLab CI/CD)
- **Managed CI service (SaaS)** — a separate CI company runs it for you (e.g. CircleCI, Travis CI)
- **Self-hosted** — you install and run the CI server yourself (e.g. Jenkins, TeamCity)

> Don't worry about pricing, free-tier limits or feature lists — they change often. Focus on **who runs the tool** and **when you would choose it**.

**Guiding Questions:**
1. What is the main difference between a managed CI service and a self-hosted one?
2. Which type of tool would you recommend to a team with strict data security requirements, and why?
3. What do all CI/CD tools have in common, regardless of their syntax or how they are hosted?

---

## Task 2 — Get Ready for GitHub Actions (25 minutes)

In class you will add a GitHub Actions workflow to the devops-demo project and compare it with your CircleCI pipeline from Lesson 4.7.

**Read:**
- [Understanding GitHub Actions — GitHub Docs](https://docs.github.com/en/actions/get-started/understand-github-actions) — focus on the components: workflows, events, jobs, actions and runners

**Review:**
- Your Lesson 4.7 `.circleci/config.yml` — make sure you can explain what each of the three jobs (build, test, publish) does

Then answer the following:

1. In GitHub Actions, where must a workflow file be saved?
2. What is an **event**? Which event will start our workflow in class?
3. What is a **runner**? What does it correspond to in CircleCI?
4. What is an **action** (`uses:`)? Which CircleCI step does `actions/checkout` replace?

**Before class, make sure you:**
- Can log in to your personal **GitHub** account and your **Docker Hub** account
- Know how to create a Docker Hub personal access token (you did this in Lesson 4.7)
- Have Java 21, Maven and Git working in your terminal (Windows users: WSL)

---

## Task 3 — Tool Selection Practice (15 minutes)

No video for this task. Read through the four company scenarios in **lesson.md Part 4** before class and for each scenario write down:

1. Which **type** of CI/CD tool you would recommend, and one tool of that type
2. Two reasons that justify your choice
3. One trade-off of your chosen type for that scenario

Come to class with your answers ready to discuss — the activity will be done in groups, so having your own thoughts prepared first will help the discussion.

**Guiding Questions:**
1. Is there ever a scenario where more than one type could be a valid choice? How would you decide?
2. What questions would you ask a team before recommending a CI/CD tool?
3. As a DevOps engineer, why is it useful to understand the concepts behind CI/CD rather than just one tool?

---

## Active Engagement Strategies

- Pause the video each time a new tool is mentioned and decide which of the three types it belongs to
- After Task 2, try to sketch the GitHub Actions version of one CircleCI job from memory — you'll check your guess in class
- For Tasks 2 and 3, write your answers before class — do not just read through the content

---

## Additional Reading Material

- [Understanding GitHub Actions — GitHub Docs](https://docs.github.com/en/actions/get-started/understand-github-actions)
- [Using secrets in GitHub Actions — GitHub Docs](https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-secrets)
- [CircleCI Documentation](https://circleci.com/docs/)
- [GitLab CI/CD Documentation](https://docs.gitlab.com/ci/)
- [Jenkins Documentation](https://www.jenkins.io/doc/)
