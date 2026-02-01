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

## What is Clawdbot?

[Clawdbot](https://github.com/clawdbot/clawdbot) connects Claude to your tools. Slack, Jira, GitHub, Notion, Calendar — all in natural language.

📚 [Docs](https://docs.clawd.bot) | 💬 [Discord](https://discord.com/invite/clawd) | 🌐 [Skills Hub](https://clawdhub.com)

---

## 🚀 Getting Started

### 1. Install Clawdbot

```bash
# Clone the repository
git clone https://github.com/clawdbot/clawdbot.git
cd clawdbot

# Install dependencies
npm install

# Configure your environment
cp .env.example .env
```

### 2. Connect to Slack

Add your Slack credentials to `.env`:

```bash
SLACK_BOT_TOKEN=xoxb-your-bot-token
SLACK_APP_TOKEN=xapp-your-app-token
SLACK_SIGNING_SECRET=your-signing-secret
```

### 3. Configure Your Tools

Create a `TOOLS.md` file with your integrations:

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

### 4. Start the Bot

```bash
npm start
```

### 5. Use It!

Just mention `@clawdbot` in any Slack channel with your prompt.

---

## 💬 How It Works (Slack Examples)

Here's how Clawdbot delivers results directly in Slack:

### Example 1: Engineering Metrics

**You ask:**
```
@clawdbot Give me engineering metrics for project CORE from the last 7 days.
Include: cycle time, lead time, throughput, bugs vs features ratio.
```

**Clawdbot responds:**

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

### Example 2: PR Review

**You ask:**
```
@clawdbot Review PR #142 in acme/backend
```

**Clawdbot responds:**

```
🔍 PR Review — acme/backend #142

Title: Add user authentication middleware

✅ Security: No vulnerabilities detected
⚠️ Performance: Consider caching token validation (line 45)
📝 Style: Missing JSDoc on exported functions

Suggestions:
• Line 45: Add Redis cache for JWT validation
• Line 72: Use constants for error messages
• Missing: Unit tests for edge cases

Overall: Approve with minor changes
```

---

### Example 3: Newsletter Digest (Scheduled)

**You configure:**
```
@clawdbot Read my newsletters and send a daily digest to #eng-reading at 09:15
```

**Clawdbot delivers every morning:**

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

### Example 4: Weekly Status Report

**You ask:**
```
@clawdbot Generate weekly status report for team Alpha
```

**Clawdbot responds:**

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
• 47 commits across 3 repos
• 15 PRs merged (avg review time: 4h)
• 2 PRs awaiting review

📊 Velocity: 34 points (+12% vs last week)
```

---

# Roles

## 👨‍💼 Engineering Manager

You lead teams, track metrics, and need to stay on top of everything without drowning in tools.

### Engineering Metrics (Jira)

Get cycle time, lead time, throughput — just ask.

**Setup:** Add to your `TOOLS.md`:
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
Connect to my Jira (credentials in TOOLS.md) and give me engineering metrics 
for project [PROJECT_KEY] from the last [7/14/30] days.

Include: cycle time, lead time, throughput, bugs vs features ratio, WIP.
Language: [Portuguese/English]
```

---

### Newsletter Digest (Stay Updated)

Daily curated reading from tech newsletters. Summarized, ranked, delivered.

**Prompt:**
```
Read the latest from these newsletters and create a daily digest:

- https://newsletter.systemdesign.one
- https://codingchallenges.substack.com  
- https://blog.algomaster.io
- https://newsletter.eng-leadership.com
- [ADD YOUR FAVORITES]

Rules:
- 3-line summary per article
- Rank top 3 (🥇🥈🥉)
- Only send if there's new content
- Include links

Create a cron job: daily at 09:15 → send to #[channel]
Language: [Portuguese/English]
```

---

### Weekly Status Report

```
Generate a weekly status report for my team.

Check:
- Jira board [BOARD_ID]: completed, in progress, blocked
- GitHub repos: [owner/repo]

Period: last 7 days
Format: executive summary + details
Language: [Portuguese/English]
```

---

### 1:1 Prep

```
Prepare my 1:1 with [DEV_NAME].

Check:
- Their recent commits and PRs in [repos]
- Jira tickets they worked on (last 2 weeks)
- Any blockers or delays

Generate talking points and questions to ask.
```

---

## 🧑‍💻 Developer

You write code, review PRs, and want to automate the boring stuff.

### PR Review

```
Review this Pull Request:
- Repository: [owner/repo]
- PR Number: #[NUMBER]

Focus on:
- Security vulnerabilities
- Performance implications  
- Code style consistency
- Missing tests

Provide actionable feedback with code suggestions.
```

---

### Debug Assistant

```
Help me debug this error:

Error: [PASTE ERROR MESSAGE]

Code:
[PASTE RELEVANT CODE]

Context: [What you were trying to do]
```

---

### Architecture Decision Record (ADR)

```
Create an ADR for:

Context: [Why we need to decide something]

Options:
1. [Option A]
2. [Option B]
3. [Option C]

Constraints: [Timeline, budget, team skills]

Use Michael Nygard format.
```

---

### Commit Message

```
Write a conventional commit message for these changes:

[PASTE DIFF OR DESCRIBE CHANGES]
```

---

## 📋 Product Manager

You write specs, coordinate launches, and communicate with everyone.

### Feature Spec

```
Write a feature specification for:

Feature: [FEATURE NAME]
Problem: [What problem it solves]
Users: [Who will use it]

Include:
- User stories
- Acceptance criteria
- Edge cases
- Success metrics
```

---

### Release Notes

```
Generate release notes for version [X.Y.Z].

Changes:
- [List of changes or point to commits/PRs]

Audience: [Internal team / Customers / Both]
Tone: [Technical / Friendly / Marketing]
```

---

### Stakeholder Update

```
Write a stakeholder update for [PROJECT_NAME].

Include:
- Progress this week
- Key decisions made
- Risks and blockers
- Next steps

Tone: Professional but concise
```

---

## ⚖️ Legal / Compliance

You need documents that actually match what the product does.

### Privacy Policy

```
Generate a Privacy Policy for my product:

Product: [NAME] - [DESCRIPTION]
Data collected: [List data types]
Auth method: [Google OAuth / Email / SSO]
Integrations: [GitHub, Jira, etc.]
Hosting: [AWS/GCP] in [REGION]
Compliance: [LGPD / GDPR / CCPA]

Include data flow tables and retention periods.
```

---

### Terms of Service

```
Generate Terms of Service for:

Product: [NAME]
Model: [SaaS B2B / B2C / Marketplace]
Pricing: [Subscription / Usage-based / Free]

Include:
- Acceptable use policy
- Liability limitations
- Data ownership
- SLA: [99.5% / 99.9%]
- Jurisdiction: [São Paulo / Delaware]
```

---

## 🎯 Founder / CEO

You need the big picture, fast.

### Company Metrics Dashboard

```
Give me a company health overview:

Check:
- Jira: velocity trends across all projects
- GitHub: commit activity, PR merge time
- [Add other data sources]

Period: Last 30 days
Compare to previous period
Highlight anomalies
```

---

### Board Update Draft

```
Draft a board update for [MONTH/QUARTER]:

Include:
- Key metrics (ARR, users, growth)
- Major milestones achieved
- Challenges faced
- Strategic priorities next quarter
- Ask for the board

Tone: Confident but transparent
Length: 1 page
```

---

### Investor Email

```
Draft an investor update email:

Highlights:
- [KEY WIN 1]
- [KEY WIN 2]

Metrics:
- [METRIC 1]: [VALUE]
- [METRIC 2]: [VALUE]

Challenges: [Be honest]
Next milestones: [What's coming]

Tone: Founder-to-investor (warm, direct)
```

---

# Tips & Best Practices

### Be Specific

❌ `Write me a privacy policy`

✅ `Write a privacy policy for a B2B SaaS that collects emails via Google OAuth, stores in AWS US-East, needs LGPD compliance`

### Use Memory

- Store context in `MEMORY.md`
- Reference previous work: *"Using the same format as the ToS we created..."*

### Chain Tasks

```
Let's do this in steps:
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
3. **Include** a real example (sanitized)
4. **Submit** a PR

### Format

```markdown
### Your Use Case Title

Brief description.

**Prompt:**
\`\`\`
Your prompt with [PLACEHOLDERS]
\`\`\`
```

---

# Community

- 💬 [Discord](https://discord.com/invite/clawd)
- 📖 [Docs](https://docs.clawd.bot)
- 🐙 [GitHub](https://github.com/clawdbot/clawdbot)
- 🌐 [Skills Hub](https://clawdhub.com)

---

[![CC0](https://mirrors.creativecommons.org/presskit/buttons/88x31/svg/cc-zero.svg)](https://creativecommons.org/publicdomain/zero/1.0/)

<p align="center">
  <sub>Built with 🤖 by the Clawdbot community</sub>
</p>
