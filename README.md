# 🦾 Gemini YOLO GitHub Agent

An autonomous AI software engineering agent powered by Google Gemini and the **Model Context Protocol (MCP)**. Designed to automate GitHub workflows, from code reviews and issue triage to autonomous code generation and bug fixing.

## 🌟 Key Features

- **🔎 Auto-Review**: Meticulous analysis of Pull Requests. Checks for security, efficiency, maintainability, and standards compliance, posting actionable inline comments directly to the PR.
- **🔀 Scheduled & On-Demand Triage**: Analyzes issues and assigns appropriate labels based on semantic context. Can be triggered on-demand or automatically via an hourly schedule.
- **⚡ Autonomous Invoke (Planning)**: Proposes structured action plans for arbitrary tasks requested via `@yolo-cli` mentions.
- **📝 Plan & Execute**: Executes the proposed plan by creating branches, modifying files, and opening pull requests automatically upon user approval.

## 🏗 Architecture

- **Google Gemini**: The core LLM driving the reasoning and code generation.
- **Gemini CLI**: Runs in YOLO mode (`--yolo`) for fully autonomous, loop-driven tool execution.
- **GitHub MCP Server**: Provides the agent with a standardized set of tools to read repositories, manage issues, read pull requests, and securely push code changes.

## 🛠 Setup Guide

### 1. Local Usage (Gemini CLI)

You can run the agent on your local machine to analyze the current repository.

1. **Environment Setup**: Ensure you have the `gemini` CLI installed.
   * **Useful Link**: [Gemini CLI Documentation](https://geminicli.com/)
2. **Authentication**: Simply run:
   ```bash
   gemini login
   ```
3. **Configuration**: You will need to configure the GitHub MCP server in your `~/.gemini/settings.json` manually for local usage to match the CI environment.
4. **Execution**:
   Use the desired prompt from the `.github/commands/` folder. For example:
   ```bash
   gemini --yolo --prompt "$(cat .github/commands/yolo-review.md)"
   ```

### 2. Server / Self-hosted Version (GitHub Actions)

For continuous operation, the agent is designed to run via GitHub Actions on a self-hosted runner.

1. **Runner Setup**:
   * Install and start a GitHub Actions Runner on your server.
   * Ensure the runner matches the required labels (e.g., `self-hosted`, `WIN`).
   * The workflows automatically download and configure the official `github-mcp-server` binary for your OS (Windows, Linux, or macOS).

**Useful Link**: [About self-hosted runners - GitHub Docs](https://docs.github.com/en/actions/hosting-your-own-runners/about-self-hosted-runners)

2. **Secrets and Variables (Recommended)**:
   While the standard `GITHUB_TOKEN` works for some operations, configuring a GitHub App is highly recommended for full capabilities:
   - `APP_ID` (Variable): Your GitHub App ID.
   - `APP_PRIVATE_KEY` (Secret): Your application's Private Key.

   **Why use a GitHub App?**
   - **Identity**: The agent posts as your App (bot) instead of a generic action user.
   - **Permissions**: Ensures the agent has sufficient cross-repo or write permissions.
   - **Rate Limits**: Apps have significantly higher API rate limits.

3. **Activation**:
   - The primary dispatcher (`yolo-dispatch.yml`) automatically triggers on specific repository events (PR opens, comments, issues) and delegates tasks to the corresponding workflow.

## 🚀 How to Use

Interaction happens directly via GitHub issue and pull request comments. Use the following commands:

- `@yolo-cli /review` — Triggers a comprehensive code review on a Pull Request.
- `@yolo-cli /triage` — Forces a triage classification and labeling on an Issue.
- `@yolo-cli <your request>` — Instructs the agent to analyze your request and generate a detailed **Plan of Action**. (e.g., `@yolo-cli refactor the authentication module`).
- `@yolo-cli /approve` — Approves the most recently generated Plan of Action, authorizing the agent to execute code changes and create a PR.

---
*Developed to demonstrate the power of autonomous AI agents in CI/CD pipelines using the Model Context Protocol.*
