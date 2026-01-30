# Awesome Clawdbot [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated collection of awesome use cases, prompts, workflows, and resources for [Clawdbot](https://github.com/clawdbot/clawdbot) — your AI-powered personal assistant that connects Claude to everything.

<p align="center">
  <a href="#use-cases">Use Cases</a> •
  <a href="#tutorials">Tutorials</a> •
  <a href="#workflows">Workflows</a> •
  <a href="#prompts">Prompts</a> •
  <a href="#contributing">Contributing</a>
</p>

---

## Contents

- [What is Clawdbot?](#what-is-clawdbot)
- [Tutorials](#tutorials)
  - [🎯 Complete Tutorial: Automated Jira Metrics Dashboard](#-complete-tutorial-automated-jira-metrics-dashboard)
- [Use Cases](#use-cases)
  - [Engineering Metrics & Reports](#engineering-metrics--reports)
  - [Legal & Compliance Documents](#legal--compliance-documents)
  - [Code Review & Development](#code-review--development)
  - [Documentation Generation](#documentation-generation)
  - [Automation & Scheduling](#automation--scheduling)
- [Workflows](#workflows)
- [Prompts](#prompts)
- [Integrations](#integrations)
- [Tips & Best Practices](#tips--best-practices)
- [Contributing](#contributing)
- [Community](#community)

---

## What is Clawdbot?

Clawdbot is an open-source AI assistant framework that connects Claude (Anthropic's AI) to your tools, files, and services. It runs locally and supports multiple channels, integrations, and automation capabilities.

**Key Features:**
- 🔌 Multi-channel messaging (Slack, Discord, Telegram, WhatsApp, Signal)
- 🛠️ Tool integration (GitHub, Jira, Notion, Calendar)
- 🌐 Browser automation
- ⏰ Cron jobs and scheduled tasks
- 🧠 Persistent memory across sessions
- 🔒 Local-first, privacy-focused

📚 [Official Documentation](https://docs.clawd.bot) | 💬 [Discord Community](https://discord.com/invite/clawd)

---

## Tutorials

### 🎯 Complete Tutorial: Automated Jira Metrics Dashboard

**Build an automated engineering metrics system that delivers daily reports to Slack with zero manual effort.**

This tutorial walks you through creating a complete metrics automation like we use for our engineering squads. By the end, you'll have:

- Automated daily/weekly reports delivered to Slack
- Key engineering metrics (Throughput, Lead Time, Cycle Time, P75)
- Energy investment tracking (Value vs Tech Debt vs Toil)
- Proactive alerts for items exceeding cycle time thresholds
- Business days calculation (excludes weekends)

---

#### 🎯 Objective

Create an automated system that:
1. Fetches data from Jira API
2. Calculates engineering metrics
3. Formats a professional report
4. Delivers to a Slack channel on schedule
5. Alerts about items taking too long

---

#### ✅ Advantages

| Benefit | Description |
|---------|-------------|
| **Zero Manual Work** | Reports generate and deliver automatically |
| **Data-Driven Decisions** | See trends, not just snapshots |
| **Predictability** | P75 percentiles tell you what to promise stakeholders |
| **Early Warning System** | Know when items are stuck before standup |
| **Energy Visibility** | See if you're building value or fighting fires |
| **Business Days** | Accurate cycle times excluding weekends |

---

#### 📋 Prerequisites

- Clawdbot installed and running
- Slack channel configured in Clawdbot
- Jira account with API access
- Python 3.x available

---

#### 🔧 Step 1: Get Your Jira API Token

1. Go to [Atlassian API Tokens](https://id.atlassian.com/manage-profile/security/api-tokens)
2. Click "Create API token"
3. Name it (e.g., "Clawdbot Metrics")
4. Copy the token (you won't see it again!)

**What you need:**
```
JIRA_URL: https://your-company.atlassian.net
EMAIL: your-email@company.com
TOKEN: your-api-token-here
BOARD_ID: 123  (find in your board URL)
PROJECT_KEY: PROJ  (your project key)
```

---

#### 🔧 Step 2: Create the Metrics Script

Ask Clawdbot to create the script:

```markdown
Create a Python script that:

1. Connects to Jira API at [YOUR_JIRA_URL]
2. Fetches issues from board ID [YOUR_BOARD_ID]
3. Calculates these metrics for the last 3 months, grouped by month:
   - Throughput (items completed per week)
   - Average Lead Time (created → done, in business days)
   - Average Cycle Time (in progress → done, in business days)
   - P75 Lead Time and P75 Cycle Time
4. Separately tracks Issue type metrics (bugs)
5. Calculates energy investment:
   - Value: Stories with Epic (direct user value)
   - Tech: Housekeeping tasks
   - Toil: Bug fixes and maintenance
6. Finds in-progress items exceeding P75 cycle time
7. Outputs a formatted report for Slack

Use these credentials:
- Email: [YOUR_EMAIL]
- API Token: [YOUR_TOKEN]

Valid issue types: História, Issue, Task, Toil
In-progress statuses: Doing, Review, Refining, Blocked

Save to: scripts/jira_metrics.py
Accept board key as command line argument (e.g., PROJ)
```

**What Clawdbot Creates:**

A script that:
- Uses only Python standard library (no pip install needed)
- Handles pagination for large boards
- Calculates business days (excludes weekends)
- Classifies items by epic name patterns
- Formats output for Slack markdown

---

#### 🔧 Step 3: Test the Script

```bash
python3 scripts/jira_metrics.py PROJ
```

**Expected Output:**

```
Squad Name (PROJ)
`Data: 30/01/2025 17:45 (business days)`

Metrics (Story, Issue, Task, Toil)
```
| Month    | Thrput/wk | AVG LT | AVG CT | P75 LT | P75 CT |
|----------|-----------|--------|--------|--------|--------|
| 2024-11  | 6.8       | 21.4d  | 3.1d   | 9d     | 4d     |
| 2024-12  | 6.8       | 10.8d  | 5.8d   | 14d    | 10d    |
| 2025-01  | 5.5       | 12.0d  | 5.4d   | 14d    | 8d     |
|----------|-----------|--------|--------|--------|--------|
| AVERAGE  | 6.3       | 14.9d  | 4.8d   | 14d    | 8d     |
```

Metrics (Issues)                                    Investment (%)
```
| Month   | Thrput/wk | P75 LT |    |  Month   | Value% | Tech% | Toil% |
|---------|-----------|--------|    |----------|--------|-------|-------|
| 2024-11 | 1.2       | 4d     |    | 2024-11  | 33.3%  | 40.7% | 25.9% |
| 2024-12 | 1.2       | 2d     |    | 2024-12  | 51.9%  | 25.9% | 22.2% |
| 2025-01 | 1.2       | 3d     |    | 2025-01  | 50.0%  | 18.2% | 31.8% |
```

> _The team can work on `6.3 items/week` in parallel and deliver in `8d` with 75% confidence_

⚡ Items exceeding P75 Cycle Time (8d):
🚨 `PROJ-123` [Doing] - 18d (225% of P75) - Feature implementation
🚨 `PROJ-456` [Review] - 12d (150% of P75) - Bug fix

---

#### 🔧 Step 4: Create the Slack Channel

1. Create a dedicated Slack channel (e.g., `#squad-metrics`)
2. Get the channel ID:
   - Open Slack in browser
   - Go to your channel
   - Channel ID is in the URL: `slack.com/.../**C0XXXXXXXX**`

---

#### 🔧 Step 5: Configure the Cron Job

Ask Clawdbot to set up the scheduled job:

```markdown
Create a cron job with these settings:

Name: jira-proj-metrics
Schedule: Every weekday at 9:00 AM (America/Sao_Paulo timezone)
Target Channel: #squad-metrics (channel ID: C0XXXXXXXX)

Task: Execute the script and send output to Slack:
python3 /path/to/scripts/jira_metrics.py PROJ

Send the output exactly as returned, without modifications.
```

**What Clawdbot Configures:**

```yaml
name: jira-proj-metrics
schedule:
  kind: cron
  expr: "0 9 * * 1-5"  # 9 AM, Monday-Friday
  tz: "America/Sao_Paulo"
sessionTarget: isolated
payload:
  kind: agentTurn
  message: "Execute: python3 /path/to/scripts/jira_metrics.py PROJ\n\nSend the output exactly as returned."
  deliver: true
  channel: slack
  to: "channel:C0XXXXXXXX"
```

---

#### 🔧 Step 6: Set Up Multiple Squads (Optional)

For multiple teams, create one cron job per squad:

```markdown
Create these cron jobs:

1. Squad Alpha Metrics
   - Name: jira-alpha-metrics
   - Schedule: 9:00 AM weekdays
   - Script: python3 scripts/jira_metrics.py ALPHA
   - Target: #alpha-metrics (C0XXXXXXX1)

2. Squad Beta Metrics
   - Name: jira-beta-metrics
   - Schedule: 9:00 AM weekdays
   - Script: python3 scripts/jira_metrics.py BETA
   - Target: #beta-metrics (C0XXXXXXX2)
```

---

#### 📊 Understanding the Metrics

| Metric | What It Measures | Why It Matters |
|--------|------------------|----------------|
| **Throughput** | Items completed per week | Team capacity and velocity |
| **Lead Time** | Backlog → Done (business days) | How long customers wait |
| **Cycle Time** | In Progress → Done (business days) | Active work time |
| **P75** | 75th percentile | Predictable delivery promise |
| **Value %** | Stories linked to Epics | Direct user value delivery |
| **Tech %** | Housekeeping/refactoring | Technical health investment |
| **Toil %** | Bug fixes and maintenance | Firefighting indicator |

---

#### 🚨 Understanding the Alerts

The system automatically flags items that are taking too long:

| Alert | Meaning | Action |
|-------|---------|--------|
| 🚨 **Red Alert** | >100% of P75 Cycle Time | Needs immediate attention |
| ⚠️ **Warning** | 80-100% of P75 Cycle Time | Getting risky, check blockers |

**Example:** If your P75 Cycle Time is 8 days, any item in progress for 8+ days gets a 🚨.

---

#### 💡 Pro Tips

**1. Customize Classification**

Adjust how items are classified by epic name patterns:

```python
# In your script
if 'bugfixing' in epic_name or 'bugfix' in epic_name:
    return 'Toil'
if 'housekeeping' in epic_name or 'tech-debt' in epic_name:
    return 'Tech'
if issue_type == 'Story':
    return 'Value'
```

**2. Adjust Time Windows**

Change the lookback period:
```python
three_months_ago = datetime.now() - timedelta(days=90)  # Adjust as needed
```

**3. Add More Statuses**

If your board has different status names:
```python
IN_PROGRESS_STATUSES = ['Doing', 'In Review', 'Testing', 'Blocked']
```

**4. Weekly Instead of Daily**

Change schedule to Mondays only:
```yaml
schedule:
  expr: "0 9 * * 1"  # Monday at 9 AM
```

---

#### 🔄 Maintenance

**Update credentials:** If your token expires, update the script and Clawdbot will use the new one automatically.

**Add new boards:** Just create a new cron job with the new board key.

**Disable temporarily:** Use `cron disable [job-name]` to pause without deleting.

---

#### 📈 Real Results

After implementing this system, teams typically see:

- **30 min/week saved** on manual reporting
- **Earlier detection** of stuck items
- **Better sprint planning** with P75 data
- **Visibility** into value vs maintenance balance
- **Data-driven retrospectives**

---

## Use Cases

### Engineering Metrics & Reports

Automate your team's engineering metrics with Jira/GitHub integration and scheduled reports.

#### Automated Sprint Metrics Dashboard

Generate weekly/monthly reports with key engineering metrics delivered to Slack.

**What You Can Track:**
- Throughput (items completed per week)
- Lead Time (backlog → done)
- Cycle Time (in progress → done)
- P75 percentiles for predictability
- Energy investment breakdown (Value vs Tech Debt vs Toil)

**Setup Example:**

1. Create a metrics script that queries your Jira API
2. Schedule it with Clawdbot's cron

```markdown
# Cron job configuration
Schedule: Every Monday at 9:00 AM
Target: #engineering-metrics (Slack)

Task: Run the Jira metrics script for project [PROJECT_KEY] and send the formatted report.
```

**Sample Output:**

```
📊 Squad Engineering Metrics
Data: 2025-01-30 (business days)

Metrics (Story, Issue, Task, Toil)
| Month    | Thrput/wk | AVG LT | AVG CT | P75 LT | P75 CT |
|----------|-----------|--------|--------|--------|--------|
| 2024-11  | 6.8       | 21.4d  | 3.1d   | 9d     | 4d     |
| 2024-12  | 6.8       | 10.8d  | 5.8d   | 14d    | 10d    |
| 2025-01  | 5.5       | 12.0d  | 5.4d   | 14d    | 8d     |
|----------|-----------|--------|--------|--------|--------|
| AVERAGE  | 6.3       | 14.9d  | 4.8d   | 14d    | 8d     |

🎯 Energy Investment:
| Month   | Value % | Tech % | Toil % |
|---------|---------|--------|--------|
| 2024-11 | 33.3%   | 40.7%  | 25.9%  |
| 2024-12 | 51.9%   | 25.9%  | 22.2%  |
| 2025-01 | 50.0%   | 18.2%  | 31.8%  |

⚡ Items exceeding P75 Cycle Time:
🚨 PROJ-123 [Doing] - 18d (225% of P75) - Feature X implementation
🚨 PROJ-456 [Review] - 12d (150% of P75) - Bug fix Y
```

**Key Benefits:**
- Automatic business days calculation (excludes weekends/holidays)
- P75 percentile tracking for predictable delivery estimates
- Energy investment visibility (are you spending time on value or firefighting?)
- Proactive alerts for items exceeding cycle time thresholds

---

#### Issue Resolution Tracking

Track bug resolution separately from feature work.

**Prompt Template:**

```markdown
Analyze our issue resolution metrics:

Project: [PROJECT_KEY]
Time Range: Last 3 months

Calculate:
- Issue throughput per week
- P75 Lead Time for issues
- Trend analysis (improving/degrading)

Compare against feature delivery metrics.
```

---

### Legal & Compliance Documents

Generate comprehensive, legally-sound documents tailored to your product.

#### Privacy Policy Generator

Create GDPR/LGPD/CCPA-compliant privacy policies that match your actual data practices.

**Prompt Template:**

```markdown
I need a Privacy Policy for my SaaS product:

**Product Info:**
- Name: [Your Product Name]
- Type: [SaaS/Mobile App/Web Platform]
- Industry: [Engineering Tools/Healthcare/Finance/etc.]

**Data Collection:**
- Authentication: [Google OAuth/Email-Password/SSO]
- User data: [Name, Email, Profile Photo, etc.]
- Third-party integrations: [GitHub, Jira, Linear, etc.]
- Data from integrations: [Commits, PRs, Issues, etc.]

**Infrastructure:**
- Hosting: [AWS/GCP/Azure]
- Database: [PostgreSQL/MongoDB/etc.]
- Region: [US-East/EU-West/etc.]

**Compliance Requirements:**
- Jurisdictions: [Brazil LGPD/EU GDPR/California CCPA]
- Role: [Data Controller/Data Processor/Both]

Generate a comprehensive Privacy Policy including:
1. Clear role definitions (Controller vs Processor)
2. All data collected with purposes and legal bases
3. Retention periods
4. Security measures
5. International transfer mechanisms
6. User rights procedures
```

**What You Get:**
- Complete Privacy Policy document
- Data flow tables with legal bases
- Retention schedule
- Subprocessor list template
- User rights request procedures

---

#### Terms of Service Generator

**Prompt Template:**

```markdown
I need Terms of Service for my SaaS product:

**Product Info:**
- Name: [Your Product Name]
- Model: [SaaS B2B/B2C/Marketplace]
- Pricing: [Subscription/Usage-based/Freemium]

**Business Requirements:**
- Liability cap: [12 months fees/fixed amount]
- SLA target: [99.5%/99.9%]
- Support: [Email/Chat/Phone]
- Jurisdiction: [São Paulo, Brazil/Delaware, USA]

**Clauses Needed:**
- [ ] Acceptable use policy
- [ ] API usage limits
- [ ] Data ownership
- [ ] IP protection
- [ ] Indemnification

Generate comprehensive Terms of Service.
```

---

### Code Review & Development

#### Automated PR Review

**Setup:**
1. Connect GitHub via Clawdbot's GitHub skill
2. Configure webhook or mention-based triggers

**Prompt Template:**

```markdown
Review PR #[NUMBER] in [owner/repo]:

Focus on:
- [ ] Security vulnerabilities
- [ ] Performance implications
- [ ] Code style consistency
- [ ] Test coverage
- [ ] Documentation needs

Provide actionable feedback with code suggestions.
```

---

#### Architecture Decision Records

**Prompt Template:**

```markdown
Create an ADR for:

**Context:**
[Current situation and why a decision is needed]

**Decision Drivers:**
- [Driver 1]
- [Driver 2]

**Options:**
1. [Option A]
2. [Option B]
3. [Option C]

**Constraints:**
- Timeline: [X weeks]
- Team expertise: [Relevant skills]

Generate ADR in Michael Nygard format.
```

---

### Documentation Generation

#### API Documentation

**Prompt Template:**

```markdown
Generate API docs for my endpoints:

Base URL: https://api.example.com/v1
Auth: Bearer token (JWT)

Endpoints:
1. POST /users - Create user
2. GET /users/{id} - Get user
3. PUT /users/{id} - Update user
4. DELETE /users/{id} - Delete user

Include for each:
- Description
- Request/Response examples
- Error codes
- Rate limits
- Code samples (curl, JavaScript, Python)

Use OpenAPI 3.0 format.
```

---

### Automation & Scheduling

#### Daily Standup Generator

**Cron Setup:**

```yaml
schedule: "0 9 * * 1-5"  # 9 AM, Monday-Friday
target: #team-standup
task: |
  Check Jira for my in-progress tickets.
  Generate standup update:
  - Completed yesterday
  - Working on today
  - Blockers
```

---

#### Heartbeat Monitoring

Use `HEARTBEAT.md` for periodic checks:

```markdown
# HEARTBEAT.md
- Check unread emails (last 2 hours)
- Summarize urgent items
- Check calendar for upcoming meetings
- Flag items needing attention
```

---

## Workflows

### Multi-Step Document Generation

```
User Request → Gather Context → Generate Draft → Review & Iterate → Export → Save to Git/Notion
```

### Scheduled Compliance Audit

1. **Trigger**: Monthly cron job
2. **Scan**: Check data flows against policy
3. **Report**: Generate compliance checklist
4. **Alert**: Notify team of gaps
5. **Track**: Update dashboard

---

## Prompts

### Quick Reference

| Task | Prompt |
|------|--------|
| Summarize meeting | "Summarize key decisions and action items from [notes]" |
| Explain code | "Explain this code for a junior developer: [code]" |
| Debug | "Debug this error: [error]. Code: [code]" |
| Draft email | "Draft email to [recipient] about [topic]. Tone: [formal/friendly]" |
| Commit message | "Write conventional commit for: [changes]" |

---

## Integrations

### Recommended Combinations

| Use Case | Skills |
|----------|--------|
| DevOps | GitHub + Jira + Slack |
| Documentation | Notion + GitHub |
| Personal Assistant | Calendar + Email + Weather |
| Team Management | Slack + Jira + GitHub |

---

## Tips & Best Practices

### 1. Be Specific

❌ "Generate metrics report"

✅ "Generate Jira metrics for project PROJ, last 3 months, calculate throughput, lead time, cycle time with P75 percentiles, group by month, use business days only"

### 2. Use Memory

- Store project context in `MEMORY.md`
- Keep daily notes in `memory/YYYY-MM-DD.md`
- Reference previous work: "Using the same format as before..."

### 3. Chain Tasks

```markdown
Let's do this step by step:
1. First, read our current documentation
2. Then, scan codebase for new endpoints
3. Generate docs for undocumented endpoints
4. Create a PR with updates
```

### 4. Request Structured Output

- "Output as markdown table"
- "Use JSON format"
- "Create Mermaid diagram"

---

## Contributing

We love contributions! This list is community-driven.

### How to Contribute

1. **Fork** this repository
2. **Add** your use case, prompt, or workflow
3. **Follow** the existing format
4. **Submit** a Pull Request

### Contribution Guidelines

- One item per PR (unless closely related)
- Include real examples (sanitized of personal data)
- Test your prompts before submitting
- Use English for all contributions

### Format Template

```markdown
#### Your Use Case Title

Brief description.

**Prompt Template:**

\`\`\`markdown
Your prompt with [PLACEHOLDERS]
\`\`\`

**What You Get:**
- Expected output 1
- Expected output 2
```

### Ideas We Want

- 🏢 Business document templates
- 🔧 DevOps automation workflows
- 📊 Data analysis prompts
- 🔐 Security audit checklists
- 🌍 Multi-language examples

---

## Community

- 💬 **Discord**: [Join us](https://discord.com/invite/clawd)
- 📖 **Docs**: [docs.clawd.bot](https://docs.clawd.bot)
- 🐙 **GitHub**: [clawdbot/clawdbot](https://github.com/clawdbot/clawdbot)
- 🌐 **Skills Hub**: [clawdhub.com](https://clawdhub.com)

---

<p align="center">
  <sub>Built with 🤖 by the Clawdbot community</sub>
</p>
