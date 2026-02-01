<p align="center">
  <img src="clawdbot-awesome-logo.png" alt="Clawdbot Awesome Logo" width="400">
</p>

# Awesome Clawdbot [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated collection of use cases organized by role. Find your job, copy the prompt, done.

<p align="center">
  <a href="#-getting-started">Getting Started</a> •
  <a href="#-engineering-manager">Eng Manager</a> •
  <a href="#-developer">Developer</a> •
  <a href="#-product-manager">Product</a> •
  <a href="#️-legal--compliance">Legal</a> •
  <a href="#-founder--ceo">Founder</a> •
  <a href="#contributing">Contributing</a>
</p>

---

## What is OpenClaw?

[OpenClaw](https://github.com/openclaw/openclaw) connects Claude to your tools. Slack, Jira, GitHub, Notion, Calendar — all in natural language.

📚 [Docs](https://docs.clawd.bot) | 💬 [Discord](https://discord.com/invite/clawd) | 🌐 [Skills Hub](https://clawdhub.com)

---

## 🚀 Getting Started

### Prerequisites

| Requirement | Description |
|-------------|-------------|
| **Node.js 22+** | Runtime for OpenClaw |
| **Claude Pro/Max** | Recommended for Opus 4.5 |
| **Slack workspace** | Where you'll interact with the bot |

---

### Choose Your System

<details>
<summary><strong>🪟 Windows (WSL2 + Ubuntu)</strong></summary>

#### Why WSL2?
Runs Linux inside Windows. Isolated, secure, and works great with OpenClaw.

#### Install WSL2

```powershell
# Run in PowerShell as Administrator
wsl --install -d Ubuntu
```

Restart your computer. Then open **Ubuntu** from Start Menu.

#### Set Up Ubuntu

```bash
# Update packages
sudo apt update && sudo apt upgrade -y

# Install Node.js 22 via nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22

# Verify
node --version  # Should show v22.x.x
```

</details>

<details>
<summary><strong>🍎 macOS (OrbStack + Ubuntu)</strong></summary>

#### Why OrbStack?
Runs Linux VMs on Mac. Fast, lightweight, and keeps everything isolated from your main system.

#### Install OrbStack

```bash
# Install via Homebrew
brew install orbstack

# Or download from https://orbstack.dev
```

#### Create Ubuntu VM

```bash
# Create and enter Ubuntu
orb create ubuntu mybot
orb shell mybot
```

#### Set Up Ubuntu (inside the VM)

```bash
# Update packages
sudo apt update && sudo apt upgrade -y

# Install Node.js 22 via nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22

# Verify
node --version  # Should show v22.x.x
```

> **Alternative for Mac:** You can also use [UTM](https://mac.getutm.app/) (free) or [Lima](https://lima-vm.io/) for running Ubuntu.

</details>

<details>
<summary><strong>🐧 Linux (Native Ubuntu/Debian)</strong></summary>

#### Set Up Node.js

```bash
# Update packages
sudo apt update && sudo apt upgrade -y

# Install Node.js 22 via nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
source ~/.bashrc
nvm install 22
nvm use 22

# Verify
node --version  # Should show v22.x.x
```

</details>

---

### Install OpenClaw

After setting up your environment (WSL2, OrbStack, or native Linux):

```bash
# Install globally
npm install -g openclaw@latest

# Run the setup wizard
openclaw onboard --install-daemon
```

The wizard configures:
- Gateway (the bridge between Slack and Claude)
- Workspace (where your configs live)
- Channels (Slack, Discord, etc.)
- Skills (integrations with Jira, GitHub, etc.)

---

### Configure Slack

1. **Create a Slack App** at [api.slack.com/apps](https://api.slack.com/apps)

2. **Enable Socket Mode**
   - Settings > Socket Mode > Enable
   - Copy the **App-Level Token** (starts with `xapp-`)

3. **Add Bot Scopes** (OAuth & Permissions > Scopes > Bot Token Scopes):
   ```
   app_mentions:read, channels:history, channels:read, chat:write,
   groups:history, groups:read, im:history, im:read, im:write
   ```

4. **Subscribe to Events** (Event Subscriptions > Enable > Subscribe to bot events):
   ```
   app_mention, message.channels, message.groups, message.im
   ```

5. **Install to Workspace** and copy the **Bot Token** (starts with `xoxb-`)

6. **Add to Config** (`~/.openclaw/openclaw.json`):

```json
{
  "agent": {
    "model": "anthropic/claude-opus-4-5"
  },
  "channels": {
    "slack": {
      "botToken": "xoxb-your-bot-token",
      "appToken": "xapp-your-app-token",
      "signingSecret": "your-signing-secret"
    }
  }
}
```

---

### Configure Your Tools

Create `TOOLS.md` in your workspace:

```markdown
## Jira
- **Instance:** https://your-company.atlassian.net
- **Email:** your-email@company.com
- **API Token:** your-jira-token
- **Project:** PROJ

## GitHub
- **Token:** ghp_your-github-token
- **Org:** your-org
```

---

### Start & Use

```bash
# Start the gateway
openclaw gateway --port 18789 --verbose
```

In Slack:
1. Invite the bot: `/invite @YourBotName`
2. Ask anything: `@YourBotName give me engineering metrics for PROJ`

---

### Run as Background Service (Optional)

Keep OpenClaw running 24/7:

```bash
sudo nano /etc/systemd/system/openclaw.service
```

```ini
[Unit]
Description=OpenClaw Gateway
After=network.target

[Service]
Type=simple
User=YOUR_USERNAME
ExecStart=/home/YOUR_USERNAME/.nvm/versions/node/v22.0.0/bin/openclaw gateway --port 18789
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl enable openclaw && sudo systemctl start openclaw
```

---

## 💡 Pro Tip: Organize with Slack Channels

Create dedicated channels for different use cases:

| Channel | Purpose |
|---------|---------|
| `#eng-metrics` | Engineering dashboards and reports |
| `#pr-reviews` | Automated PR reviews |
| `#daily-digest` | Newsletter summaries |
| `#status-reports` | Weekly team updates |

Then just `@mention` the bot in the right channel with your prompt!

---

# Roles

Each use case below includes:
- **What it is** — Brief description
- **How it works** — What happens when you use it
- **Prompt** — Copy, paste, customize
- **Example output** — What you'll get back

---

## 👨‍💼 Engineering Manager

You lead teams, track metrics, and need to stay on top of everything without drowning in tools.

---

### Engineering Metrics

**What it is:** Real-time engineering metrics from Jira — cycle time, lead time, throughput, and more.

**How it works:** OpenClaw connects to your Jira, pulls issue data, calculates DORA-style metrics, and returns a formatted report.

**Setup:** Add to `TOOLS.md`:
```markdown
## Jira
- **Instance:** https://YOUR-INSTANCE.atlassian.net
- **Email:** your-email@company.com
- **API Token:** your-token
- **Project:** PROJECT_KEY
- **Board ID:** 123
```

**Prompt:**
```
@openclaw Connect to my Jira (credentials in TOOLS.md) and give me engineering
metrics for project CORE from the last 7 days.

Include: cycle time, lead time, throughput, bugs vs features ratio, WIP.
```

**Example output:**
```
📊 Engineering Metrics — CORE (Last 7 Days)

┌─────────────────┬───────────┐
│ Metric          │ Value     │
├─────────────────┼───────────┤
│ Cycle Time      │ 2.3 days  │
│ Lead Time       │ 4.1 days  │
│ Throughput      │ 23 items  │
│ Bugs/Features   │ 30%/70%   │
│ WIP             │ 8 items   │
└─────────────────┴───────────┘

📈 Trend: Cycle time improved 15% vs previous week
⚠️ Alert: WIP slightly above team limit (6)
```

---

### Newsletter Digest

**What it is:** Daily curated reading from tech newsletters, summarized and ranked.

**How it works:** OpenClaw fetches your newsletters, summarizes each article, ranks the top 3, and posts to your Slack channel on a schedule.

**Prompt:**
```
@openclaw Read the latest from these newsletters and create a daily digest:

- https://newsletter.systemdesign.one
- https://codingchallenges.substack.com
- https://blog.algomaster.io
- https://newsletter.eng-leadership.com

Rules:
- 3-line summary per article
- Rank top 3 (🥇🥈🥉)
- Only send if there's new content
- Include links

Create a cron job: daily at 09:15 → send to #eng-reading
```

**Example output:**
```
📰 Daily Tech Digest — Jan 31, 2025

🥇 "Why We Moved to Event Sourcing" — System Design
   Event sourcing solved our audit trail nightmare. Here's the
   architecture that handles 10M events/day...
   → https://newsletter.systemdesign.one/p/event-sourcing

🥈 "The Hidden Cost of Microservices" — Eng Leadership
   Not every team needs microservices. A framework for deciding
   when monolith-first makes sense...
   → https://newsletter.eng-leadership.com/p/microservices

🥉 "LeetCode Patterns You Actually Use" — AlgoMaster
   The 5 patterns that appear in 80% of real interviews...
   → https://blog.algomaster.io/p/patterns

📚 3 more articles in thread ↓
```

---

### Weekly Status Report

**What it is:** Automated weekly summary of your team's work from Jira and GitHub.

**How it works:** OpenClaw pulls completed/in-progress/blocked items from Jira, commit activity from GitHub, and compiles everything into an executive summary.

**Prompt:**
```
@openclaw Generate a weekly status report for my team.

Check:
- Jira board 123: completed, in progress, blocked
- GitHub repos: acme/backend, acme/frontend

Period: last 7 days
Format: executive summary + details
```

**Example output:**
```
📋 Weekly Status — Team Alpha
Period: Jan 24-31, 2025

━━━ COMPLETED (12) ━━━
✅ CORE-234: User authentication flow
✅ CORE-241: API rate limiting
✅ CORE-245: Dashboard redesign
   ... +9 more

━━━ IN PROGRESS (5) ━━━
🔄 CORE-250: Payment integration (80%)
🔄 CORE-251: Email notifications (60%)
   ... +3 more

━━━ BLOCKED (1) ━━━
🚫 CORE-248: Third-party API credentials pending

━━━ GITHUB ━━━
• 47 commits across 2 repos
• 15 PRs merged (avg review time: 4h)
• 2 PRs awaiting review

📊 Velocity: 34 points (+12% vs last week)
```

---

### 1:1 Prep

**What it is:** Talking points and context for your 1:1 meetings based on the person's recent work.

**How it works:** OpenClaw reviews their commits, PRs, and Jira tickets to surface accomplishments, blockers, and discussion topics.

**Prompt:**
```
@openclaw Prepare my 1:1 with Maria.

Check:
- Her recent commits and PRs in acme/backend
- Jira tickets she worked on (last 2 weeks)
- Any blockers or delays

Generate talking points and questions to ask.
```

**Example output:**
```
👤 1:1 Prep — Maria (Backend Engineer)

━━━ HIGHLIGHTS ━━━
✨ Shipped authentication middleware (CORE-234)
✨ 12 commits, 4 PRs merged
✨ Zero reverts or hotfixes

━━━ DISCUSSION POINTS ━━━
💬 CORE-248 blocked for 5 days — needs API credentials
💬 Large PR #156 open for 3 days — needs review
💬 Took on 2 unplanned bug fixes

━━━ SUGGESTED QUESTIONS ━━━
• "How's the payment integration going?"
• "Need help unblocking the API credentials?"
• "Any concerns about the upcoming deadline?"
```

---

## 🧑‍💻 Developer

You write code, review PRs, and want to automate the boring stuff.

---

### PR Review

**What it is:** Automated code review for your Pull Requests.

**How it works:** OpenClaw fetches the PR diff from GitHub, analyzes it for security, performance, and style issues, and provides actionable feedback.

**Prompt:**
```
@openclaw Review this Pull Request:
- Repository: acme/backend
- PR Number: #142

Focus on:
- Security vulnerabilities
- Performance implications
- Code style consistency
- Missing tests

Provide actionable feedback with code suggestions.
```

**Example output:**
```
🔍 PR Review — acme/backend #142

Title: Add user authentication middleware

✅ Security: No vulnerabilities detected
⚠️ Performance: Consider caching token validation (line 45)
📝 Style: Missing JSDoc on exported functions

━━━ SUGGESTIONS ━━━
• Line 45: Add Redis cache for JWT validation
  ```js
  const cached = await redis.get(`token:${token}`);
  if (cached) return JSON.parse(cached);
  ```
• Line 72: Use constants for error messages
• Missing: Unit tests for edge cases (expired token, invalid signature)

Overall: Approve with minor changes
```

---

### Debug Assistant

**What it is:** Help debugging errors with context-aware suggestions.

**How it works:** You paste the error and code, OpenClaw analyzes the stack trace and code, and suggests fixes.

**Prompt:**
```
@openclaw Help me debug this error:

Error: TypeError: Cannot read property 'id' of undefined
  at processUser (/app/src/users.js:42:15)

Code:
function processUser(user) {
  return { id: user.id, name: user.name };
}

Context: This happens when processing webhook payloads from Stripe.
```

**Example output:**
```
🔧 Debug Analysis

━━━ PROBLEM ━━━
`user` is undefined when `processUser` is called.

━━━ LIKELY CAUSE ━━━
Stripe webhook payload structure differs from expected.
The user object might be nested under `data.object.customer`.

━━━ FIX ━━━
function processUser(user) {
  if (!user) {
    console.error('processUser called with undefined user');
    return null;
  }
  return { id: user.id, name: user.name };
}

━━━ BETTER FIX ━━━
// Extract user from Stripe payload correctly
const user = event.data.object.customer;
```

---

### Architecture Decision Record (ADR)

**What it is:** Structured documentation for architectural decisions.

**How it works:** You describe the context and options, OpenClaw formats it using the Michael Nygard ADR template.

**Prompt:**
```
@openclaw Create an ADR for:

Context: We need to choose a message queue for async processing.

Options:
1. RabbitMQ — mature, feature-rich
2. Redis Streams — simple, we already use Redis
3. AWS SQS — managed, but vendor lock-in

Constraints: Small team, need simple ops, moderate volume (~10k msgs/day)

Use Michael Nygard format.
```

**Example output:**
```
📄 ADR-001: Message Queue Selection

━━━ STATUS ━━━
Proposed

━━━ CONTEXT ━━━
We need async processing for background jobs. Current sync
processing causes timeout issues during peak load.

━━━ DECISION ━━━
Use Redis Streams.

━━━ CONSEQUENCES ━━━
✅ Simple — already have Redis expertise
✅ No new infrastructure to manage
✅ Good enough for 10k msgs/day
⚠️ May need to migrate if volume 10x
⚠️ Less feature-rich than RabbitMQ

━━━ ALTERNATIVES CONSIDERED ━━━
• RabbitMQ: More features, but adds operational complexity
• AWS SQS: Managed, but adds vendor lock-in and latency
```

---

### Commit Message

**What it is:** Generate conventional commit messages from your changes.

**How it works:** You describe or paste the diff, OpenClaw writes a properly formatted commit message.

**Prompt:**
```
@openclaw Write a conventional commit message for:

Added rate limiting middleware to the API.
Limits requests to 100/minute per IP.
Uses Redis for distributed counting.
```

**Example output:**
```
feat(api): add rate limiting middleware

- Limit requests to 100/minute per IP address
- Use Redis for distributed request counting
- Return 429 Too Many Requests when limit exceeded

Closes #234
```

---

## 📋 Product Manager

You write specs, coordinate launches, and communicate with everyone.

---

### Feature Spec

**What it is:** Complete feature specification with user stories and acceptance criteria.

**How it works:** You describe the feature, OpenClaw structures it into a proper spec document.

**Prompt:**
```
@openclaw Write a feature specification for:

Feature: Password reset via email
Problem: Users forget passwords and can't log in
Users: All registered users

Include:
- User stories
- Acceptance criteria
- Edge cases
- Success metrics
```

**Example output:**
```
📋 Feature Spec: Password Reset via Email

━━━ OVERVIEW ━━━
Allow users to reset their password via email link.

━━━ USER STORIES ━━━
• As a user who forgot my password, I want to reset it via email
• As a user, I want the reset link to expire for security

━━━ ACCEPTANCE CRITERIA ━━━
✓ "Forgot password" link on login page
✓ Email sent within 30 seconds
✓ Link expires after 1 hour
✓ Link works only once
✓ Password must meet strength requirements

━━━ EDGE CASES ━━━
• Email not found → Show generic "check your email" (security)
• Expired link → Show "link expired, request new one"
• Already used link → Show "link already used"

━━━ SUCCESS METRICS ━━━
• 90% of reset emails delivered in <30s
• 80% of users complete reset in <5min
• <5% of users need support help
```

---

### Release Notes

**What it is:** User-friendly release notes from your changes.

**How it works:** You list the changes, OpenClaw formats them for your audience (technical or customer-facing).

**Prompt:**
```
@openclaw Generate release notes for version 2.4.0.

Changes:
- Added dark mode
- Fixed login timeout bug
- Improved search performance by 40%
- Added export to CSV

Audience: Customers
Tone: Friendly
```

**Example output:**
```
🎉 What's New in v2.4.0

━━━ NEW FEATURES ━━━
🌙 Dark Mode
   Easy on the eyes! Toggle in Settings > Appearance.

📊 Export to CSV
   Download your data anytime. Look for the export button
   on any table.

━━━ IMPROVEMENTS ━━━
⚡ Faster Search
   Search is now 40% faster. Find what you need, instantly.

━━━ BUG FIXES ━━━
🔧 Fixed an issue where some users were logged out unexpectedly.

━━━ UPDATE NOW ━━━
This update is available now. Refresh your browser to get it!
```

---

### Stakeholder Update

**What it is:** Professional project status update for stakeholders.

**How it works:** You provide the project info, OpenClaw formats it into an executive-friendly update.

**Prompt:**
```
@openclaw Write a stakeholder update for Project Phoenix.

Include:
- Progress this week
- Key decisions made
- Risks and blockers
- Next steps

Tone: Professional but concise
```

**Example output:**
```
📊 Project Phoenix — Weekly Update
Week of Jan 27, 2025

━━━ PROGRESS ━━━
• Completed user authentication module (100%)
• Payment integration at 80% (on track)
• 15 bugs fixed, 3 remaining

━━━ KEY DECISIONS ━━━
• Selected Stripe for payments (over PayPal)
• Pushed mobile app to Phase 2

━━━ RISKS & BLOCKERS ━━━
🟡 Waiting on legal approval for Terms of Service
🟡 QA bandwidth limited next week

━━━ NEXT STEPS ━━━
• Complete payment integration by Feb 3
• Begin beta testing Feb 5
• Prepare launch marketing materials

Overall Status: 🟢 On Track
```

---

## ⚖️ Legal / Compliance

You need documents that actually match what the product does.

---

### Privacy Policy

**What it is:** LGPD/GDPR-compliant privacy policy tailored to your product.

**How it works:** You describe your data practices, OpenClaw generates a complete privacy policy.

**Prompt:**
```
@openclaw Generate a Privacy Policy for my product:

Product: TaskFlow - Project management SaaS
Data collected: Email, name, profile photo, project data
Auth method: Google OAuth
Integrations: GitHub, Jira, Slack
Hosting: AWS in us-east-1
Compliance: LGPD and GDPR

Include data flow tables and retention periods.
```

**Example output:**
```
📜 Privacy Policy — TaskFlow

Last updated: January 31, 2025

━━━ DATA WE COLLECT ━━━
| Data Type      | Source       | Purpose           | Retention |
|----------------|--------------|-------------------|-----------|
| Email          | Google OAuth | Account ID        | Account life |
| Name           | Google OAuth | Display name      | Account life |
| Profile photo  | Google OAuth | Avatar            | Account life |
| Project data   | User input   | Core service      | Account life + 30 days |

━━━ HOW WE USE IT ━━━
• Provide the TaskFlow service
• Send important account notifications
• Improve our service (aggregated analytics only)

━━━ THIRD PARTIES ━━━
• AWS (hosting) — us-east-1
• GitHub, Jira, Slack (integrations you enable)

━━━ YOUR RIGHTS (LGPD/GDPR) ━━━
• Access your data
• Correct your data
• Delete your data
• Export your data
• Withdraw consent

Contact: privacy@taskflow.com
```

---

### Terms of Service

**What it is:** Legal terms of service for your product.

**How it works:** You describe your business model, OpenClaw generates appropriate terms.

**Prompt:**
```
@openclaw Generate Terms of Service for:

Product: TaskFlow
Model: SaaS B2B
Pricing: Subscription (monthly/annual)

Include:
- Acceptable use policy
- Liability limitations
- Data ownership
- SLA: 99.5%
- Jurisdiction: Delaware, USA
```

**Example output:**
```
📜 Terms of Service — TaskFlow

Last updated: January 31, 2025

━━━ ACCEPTABLE USE ━━━
You may NOT use TaskFlow to:
• Violate any laws
• Store illegal content
• Attempt to breach security
• Resell without permission

━━━ DATA OWNERSHIP ━━━
• Your data is yours
• We don't sell your data
• We access data only to provide the service

━━━ SERVICE LEVEL ━━━
• 99.5% uptime guarantee
• Downtime credits: 10x the downtime
• Scheduled maintenance excluded

━━━ LIABILITY ━━━
• Limited to fees paid in last 12 months
• No liability for indirect damages

━━━ JURISDICTION ━━━
• Governed by Delaware law
• Disputes resolved in Delaware courts

Full legal document: taskflow.com/legal/tos
```

---

## 🎯 Founder / CEO

You need the big picture, fast.

---

### Company Metrics Dashboard

**What it is:** Bird's-eye view of company health across all tools.

**How it works:** OpenClaw pulls data from Jira, GitHub, and other sources to create an executive dashboard.

**Prompt:**
```
@openclaw Give me a company health overview:

Check:
- Jira: velocity trends across all projects
- GitHub: commit activity, PR merge time

Period: Last 30 days
Compare to previous period
Highlight anomalies
```

**Example output:**
```
📊 Company Health — Last 30 Days

━━━ ENGINEERING VELOCITY ━━━
         This Month    Last Month    Change
Points      342          298         +15% ✅
PRs Merged   89           72         +24% ✅
Cycle Time  2.1 days    2.8 days    -25% ✅

━━━ TEAM BREAKDOWN ━━━
• Backend:  142 points (+22%)
• Frontend: 118 points (+8%)
• Mobile:    82 points (+12%)

━━━ ANOMALIES ━━━
⚠️ Mobile team WIP spiked to 12 (usually 6)
⚠️ PR review time increased 40% for frontend

━━━ COMMITS ━━━
Total: 847 commits across 12 repos
Top contributors: Maria (92), João (78), Ana (71)
```

---

### Board Update Draft

**What it is:** Professional board update ready for your meeting.

**How it works:** You provide highlights, OpenClaw formats it into a board-ready document.

**Prompt:**
```
@openclaw Draft a board update for Q4 2024:

Include:
- Key metrics (ARR, users, growth)
- Major milestones achieved
- Challenges faced
- Strategic priorities next quarter
- Ask for the board

Tone: Confident but transparent
Length: 1 page
```

**Example output:**
```
📊 Board Update — Q4 2024

━━━ KEY METRICS ━━━
• ARR: $2.4M (+18% QoQ)
• Active Users: 12,400 (+22% QoQ)
• Net Revenue Retention: 115%
• Runway: 18 months

━━━ MILESTONES ━━━
✅ Launched enterprise tier — 3 Fortune 500 pilots
✅ SOC 2 Type II certified
✅ Hired VP Engineering and VP Sales

━━━ CHALLENGES ━━━
• Sales cycle longer than expected (45 days → 68 days)
• Engineering velocity dipped in November (holidays + hiring)

━━━ Q1 2025 PRIORITIES ━━━
1. Close 2 enterprise deals ($500K+ ARR)
2. Launch mobile app
3. Expand to LATAM market

━━━ ASK ━━━
• Intro to [specific target company] for enterprise deal
• Feedback on LATAM expansion timing
```

---

### Investor Email

**What it is:** Monthly investor update email.

**How it works:** You provide the highlights, OpenClaw writes a warm, professional update.

**Prompt:**
```
@openclaw Draft an investor update email:

Highlights:
- Closed Series A at $8M
- Hit $200K MRR

Metrics:
- MRR: $200K
- Growth: 15% MoM
- Customers: 180

Challenges: Hiring senior engineers is hard
Next milestones: Launch enterprise tier, hit $300K MRR

Tone: Founder-to-investor (warm, direct)
```

**Example output:**
```
Subject: January Update: Series A Closed, $200K MRR 🚀

Hi everyone,

Big month. We closed our Series A at $8M and hit $200K MRR.

━━━ NUMBERS ━━━
• MRR: $200K (+15% MoM)
• Customers: 180 (+12 this month)
• Team: 18 people

━━━ WINS ━━━
• Series A closed with [Lead Investor]
• Signed 3 enterprise pilots
• Zero churn for 4th straight month

━━━ CHALLENGES ━━━
Hiring senior engineers remains tough. We've made 2 offers
that were declined for Big Tech. Adjusting comp and trying
new channels.

━━━ NEXT 30 DAYS ━━━
• Launch enterprise tier
• Target: $230K MRR
• Hire 2 senior engineers

As always, intros to senior backend engineers appreciated!

Best,
[Your name]
```

---

# Tips & Best Practices

### Be Specific

❌ `@openclaw Write me a privacy policy`

✅ `@openclaw Write a privacy policy for a B2B SaaS that collects emails via Google OAuth, stores in AWS US-East, needs LGPD compliance`

### Use Dedicated Channels

Create Slack channels for different purposes:
- `#eng-metrics` — Ask for dashboards and reports
- `#pr-reviews` — Request code reviews
- `#daily-digest` — Receive scheduled digests

### Chain Tasks

```
@openclaw Let's do this in steps:
1. First, read our current docs
2. Find gaps
3. Generate missing sections
4. Create a PR
```

---

# Contributing

Add your use cases! This is community-driven.

1. **Fork** this repo
2. **Add** your prompt under the right role
3. **Include**: What it is, How it works, Prompt, Example output
4. **Submit** a PR

---

# Community

- 💬 [Discord](https://discord.com/invite/clawd)
- 📖 [Docs](https://docs.clawd.bot)
- 🐙 [GitHub](https://github.com/openclaw/openclaw)
- 🌐 [Skills Hub](https://clawdhub.com)

---

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

<p align="center">
  <sub>Built with 🤖 by the Clawdbot community</sub>
</p>
