# Assignment (Optional)

## Brief

Decide which type of CI/CD tool fits a given situation, and extend the GitHub Actions workflow you built in class.

1. **CI/CD Tool Types**
   - Create a `TOOLS.md` file in your `devops-demo-lesson-4.9` repository
   - For each of the three types of CI/CD tools covered in the lesson — **platform-integrated**, **managed CI service (SaaS)** and **self-hosted** — describe:
     - **Who runs it:** your Git platform, a separate CI company, or your own team
     - **Two example tools** (include GitHub Actions under platform-integrated)
     - **When to choose it:** 2-3 situations where this type is the best fit
     - **Trade-offs:** 2-3 drawbacks or constraints
   - Add a simple diagram or decision flow that shows how you would choose between the three types
   - Keep it high level: do **not** include pricing, free-tier limits or feature checklists — these change often and are not what drives the decision

2. **Tool Selection Scenarios**
   - In `TOOLS.md`, write a brief analysis (1-2 paragraphs each) for THREE of the following scenarios, recommending a type of CI/CD tool and a tool of that type:

   **Scenario A - Startup:**
   - 5-person team building a mobile app
   - All code is on GitHub
   - Need to launch an MVP in 3 months
   - No dedicated DevOps person

   **Scenario B - Healthcare Company:**
   - 30-person development team
   - Strict healthcare data-protection requirements
   - Cannot store code or build artifacts in a public cloud
   - Have 2 dedicated DevOps engineers

   **Scenario C - Growing SaaS:**
   - 20-person team with all code on GitLab
   - Need integrated issue tracking and CI/CD
   - Want one platform for everything
   - Currently spending 10+ hours a week managing separate tools

   **Scenario D - Software Consultancy:**
   - 40 developers building projects for many clients
   - Some clients use GitHub, others use Bitbucket
   - Want one CI tool and one dashboard across all projects
   - Do not want to run their own servers

   - For each scenario you analyse, include:
     - The recommended **type** of CI/CD tool and a tool of that type
     - 2-3 specific reasons supporting your choice
     - 1-2 trade-offs or things to watch out for
     - Why the other two types would NOT be as good a fit

3. **Extend Your GitHub Actions Workflow**
   - Update `.github/workflows/ci.yml` in your `devops-demo-lesson-4.9` repository:
     - Also run the workflow on **pull requests** to `main`
     - Make sure the **publish** job only runs for pushes to `main`, not for pull requests (hint: look up the `if:` condition and the `github.event_name` context)
     - Tag the Docker image with the commit SHA as well as `latest` (hint: `${{ github.sha }}`)
   - Open a pull request to show that **build** and **test** run but **publish** is skipped, then merge it and show that all three jobs run
   - In `TOOLS.md`, write 2-3 sentences on how you would make the same changes in your CircleCI `config.yml` from Lesson 4.7

## Submission (Optional)

- Submit the URL of your `devops-demo-lesson-4.9` GitHub repository to NTU Blackboard.
- Should you reference the work of your classmate(s) or online resources, give them credit by adding either the name of your classmate or URL.

## References
- GitHub Actions: https://docs.github.com/en/actions
- Workflow syntax for GitHub Actions: https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax
- CircleCI: https://circleci.com/docs/
- Docker Hub: https://docs.docker.com/docker-hub/
