#step10
# GitHub Actions Workflow Analysis

## 1. What triggers this workflow to run?

The workflow is triggered by two events:

- A **push to the `main` branch**
- A **pull request targeting the `main` branch**

These triggers are defined in the `on:` section of the `deploy.yml` file.

## 2. What are the four main steps this workflow performs?

The four main steps in the `build-and-test` job are:

1. **Checkout code**
2. **Validate HTML**
3. **Check links**
4. **Upload artifact**

After these steps are completed successfully, a separate deployment job runs to deploy the website to GitHub Pages when the changes were pushed directly to the `main` branch.

## 3. What does the "Checkout code" step do and why is it necessary?

The **Checkout code** step uses the `actions/checkout@v4` action to download the repository's current files into the GitHub Actions runner.

This step is necessary because the following workflow steps need access to the website files. For example, the HTML validator needs to inspect the HTML files, the link checker needs to check links, and the deployment process needs the website files to publish the site.

## 4. What is the purpose of the environment configuration?

The environment configuration identifies the **GitHub Pages environment** where the website will be deployed.

The workflow uses:

- `name: github-pages`
- `url: ${{ steps.deployment.outputs.page_url }}`

This connects the deployment job to the GitHub Pages environment and allows GitHub to associate the deployed website with its published URL. The environment also works with the permissions required for GitHub Pages deployment.

## 5. How does this automated deployment improve reliability compared to manual deployment?

Automated deployment improves reliability because the same defined process is performed every time the workflow runs. The workflow automatically checks the HTML, checks links, uploads the website artifact, and deploys the site to GitHub Pages.

This reduces the possibility of human mistakes, such as forgetting to upload a file, deploying an incorrect version, or skipping a validation step. It also makes the deployment process faster, consistent, and easier to repeat.

## 6. What would happen if you pushed code to a different branch (not main)?

If code is pushed to a branch other than `main`, this workflow will **not run for that push** because the workflow is configured to trigger pushes only on the `main` branch.

If the different branch is used to create a pull request targeting `main`, the workflow can run for that pull request. However, the deployment job will not deploy the website because the workflow specifically requires a push to `main`:

```yaml
if: github.event_name == 'push' && github.ref == 'refs/heads/main'
