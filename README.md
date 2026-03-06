# 🦾 Gemini YOLO GitHub Agent

An autonomous AI agent powered by Google Gemini, designed to automate GitHub workflows: from code reviews to issue triage.

## 🌟 Key Features

- **🔎 Auto-Review**: Deep analysis of Pull Requests, checking for security, efficiency, and standards compliance, posting comments directly to the PR.
- **🔀 Auto-Triage**: Analysis of new Issues and PRs, suggesting appropriate labels and initial assessment.
- **⚡ Autonomous Invoke**: Execution of arbitrary instructions via `@yolo-cli` mentions in comments.
- **📝 Plan & Execute**: Ability to create a plan for changes and implement them after user confirmation.

## 🛠 Setup Guide

### 1. Local Usage (Gemini CLI)

You can run the agent on your local machine to analyze the current repository. The CLI handles most of the configuration automatically.

1. **Environment Setup**: Ensure you have the `gemini` CLI installed.
2. **Authentication**: Simply run:

   ```bash
   gemini login
   ```

   This will authenticate your session and set up the necessary environment for the CLI.
3. **Execution**:
   Use the desired command from the `.github/commands/` folder. For example, for a review:

   ```bash
   gemini --yolo --prompt "$(cat .github/commands/yolo-review.md)"
   ```

### 2. Server / Self-hosted Version (GitHub Actions)

For continuous operation in the repository, GitHub Actions with your own server (self-hosted runner) are used.

1. **Runner Setup**:
   - Install and start a GitHub Actions Runner on your server.
   - Ensure the runner has the `self-hosted` tag (as specified in the `.yml` files).
2. **Secrets and Variables (Settings -> Secrets and variables -> Actions)**:
   - `APP_ID`: Your GitHub App ID (for authorization).
   - `APP_PRIVATE_KEY`: Your application's Private Key.
   - These are used in `yolo-dispatch.yml` to generate access tokens.
3. **Activation**:
   - Place the files from `.github/workflows/` into your repository.
   - The main dispatcher (`yolo-dispatch.yml`) will automatically trigger on events (PR opens, comments) and delegate tasks to the appropriate workflows.

## 🚀 How to Use

Interaction happens via GitHub comments:

- Write `@yolo-cli /review` in a PR to trigger a check.
- Write `@yolo-cli /triage` in a new issue for classification.
- Simply mention `@yolo-cli` and describe the task (e.g., `@yolo-cli translate comments to English`), and the agent will attempt to perform it.

---
*Developed to demonstrate the power of autonomous agents in CI/CD processes.*
