# Awesome Clawdbot [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated collection of awesome use cases, prompts, workflows, and resources for [Clawdbot](https://github.com/clawdbot/clawdbot) — your AI-powered personal assistant that connects Claude to everything.

<p align="center">
  <a href="#use-cases">Use Cases</a> •
  <a href="#workflows">Workflows</a> •
  <a href="#prompts">Prompts</a> •
  <a href="#integrations">Integrations</a> •
  <a href="#contributing">Contributing</a>
</p>

---

## Contents

- [What is Clawdbot?](#what-is-clawdbot)
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
