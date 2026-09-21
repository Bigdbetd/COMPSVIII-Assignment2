# GitHub Actions Workflow Analysis

## 1. What triggers this workflow to run?

The workflow runs in two situations defined in the `on:` section of `.github/workflows/deploy.yml`:

- When code is pushed to the `main` branch.
- When a pull request targets the `main` branch.

The build-and-test job runs for both events. The deploy job runs only for a push to `main`, not for a pull request.

## 2. What are the four main steps this workflow performs?

The four steps in the `build-and-test` job are:

1. **Checkout code** — retrieves the repository contents.
2. **Validate HTML** — checks the HTML files for standards and syntax problems.
3. **Check links** — checks Markdown links for broken destinations.
4. **Upload artifact** — packages the website files so GitHub Pages can deploy them.

After those four steps succeed on a push to `main`, the separate `deploy` job performs **Deploy to GitHub Pages**.

## 3. What does the "Checkout code" step do and why is it necessary?

The Checkout code step uses `actions/checkout@v4` to copy the repository's files into the GitHub Actions runner. A runner begins without the project's source code, so later steps would have no HTML, Markdown, CSS, JavaScript, or configuration files to validate or upload without this step.

## 4. What is the purpose of the environment configuration?

The environment configuration associates the deployment with GitHub's `github-pages` environment. It also records the deployed page URL from the deployment step's output. This lets GitHub track the deployment, display its status and URL, and apply any environment-specific controls or protection rules.

## 5. How does automated deployment improve reliability compared with manual deployment?

Automated deployment follows the same repeatable process after every accepted change. It validates the HTML, checks links, and uploads a known artifact before deployment. This reduces human errors such as skipping validation, uploading the wrong files, or forgetting a step. Failed checks are visible in GitHub and prevent the deploy job from running successfully.

## 6. What happens if code is pushed to a branch other than main?

A normal push to another branch does not trigger this workflow because the push trigger is limited to `main`. However, opening a pull request from that branch into `main` triggers the build-and-test job. The site is not deployed during the pull request because the deploy job requires a push event whose reference is `refs/heads/main`.
