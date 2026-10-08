# 🧹 GitHub Enterprise Garbage Collector

An automated, serverless FinOps and governance utility designed for **GitHub Enterprise** organizations. This tool systematically scans your organization to identify stale or inactive repositories, alerts your engineering team via Slack, and automatically archives them to eliminate hidden cloud storage costs and reduce security exposure.
I had the idea to create this repository form random reasons - so I decided to create something "impossible" that even big companies have trouble to create, and I found this idea. But you now what is not impossible? Hitting that "Follow user" button.

I'm sorry. Let's move on.

## ✨ Features

- **Automated Lifecycle Scanning:** Automatically checks the last commit dates across all repositories in your organization using the GitHub REST API.
- **Enterprise-Grade Slack Reporting:** Dispatches rich, beautifully formatted summaries directly to your Slack channel before taking destructive actions.
- **Fail-Safe Dry Run:** Includes a default safety-first simulation mode (`DRY_RUN=true`) that logs findings and sends alerts without altering production code.
- **Serverless Automation:** Built to run natively and out-of-the-box via GitHub Actions schedules (*cron*).

---

## 🚀 Setup Instructions

### 1. Configure GitHub Repository Secrets
To securely run this engine without hardcoding credentials, add the following variables to your repository under **Settings** > **Secrets and variables** > **Actions** > **New repository secret**:

| Secret Name | Description | Required |
| :--- | :--- | :--- |
| `ORGANIZATION_ADMIN_PAT` | A Personal Access Token (Classic) with full `repo` and `admin:org` scopes. | **Yes** |
| `SLACK_ALERT_WEBHOOK_URL` | The Incoming Webhook URL generated from your Slack App management portal. | **Yes** |
| `ORGANIZATION_GITHUB_API_URL`| Custom endpoint URL if using GitHub Enterprise Server (e.g., `https://company.com`). Defaults to public GitHub API if omitted. | *Optional* |

---

## 🛠️ Local Development & Manual Testing

If you want to pull down the repository and test the engine locally on your workstation, follow these steps:

### Prerequisites
- Node.js (v20 or higher)
- npm

### Installation
Clone the repository and install the development dependencies:
```bash
npm install
```

### Environment Configuration
Create a `.env` file in the root directory of the project:
```env
GITHUB_API_URL=https://github.com
GITHUB_PAT=your_copied_github_personal_access_token
GITHUB_ORG=your_target_github_organization_name
SLACK_WEBHOOK_URL=https://slack.com
INACTIVITY_DAYS_THRESHOLD=180
DRY_RUN=true
```
> ⚠️ **IMPORTANT:** Leave `DRY_RUN=true` during initial local testing to safely preview matches without archiving production assets.

### Running the Engine
Execute the TypeScript execution script directly:
```bash
npm run dev
```

---

## 🤖 GitHub Actions Automated Execution

The project comes pre-configured with a continuous integration workflow located at `.github/workflows/cleanup.yml`. 

### Automated Schedule
The workflow runs automatically **every Monday at 00:00 UTC** completely serverless on GitHub-hosted runners.

### Manual Dispatch (UI Trigger)
You can manually force a cleanup assessment run at any time via the GitHub web portal:
1. Navigate to the **Actions** tab at the top of your GitHub repository.
2. Select **Automated GitHub Garbage Collector** from the left sidebar workflow listing.
3. Click the **Run workflow** dropdown interface on the right.
4. Set the `dry_run_override` value (`true` for safety log auditing, `false` to actively archive matched items) and click the green **Run workflow** button.

---

## 📄 License
This project is open-source software licensed under the [MIT License](LICENSE).
