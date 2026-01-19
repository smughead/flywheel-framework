# PRD Full: Complete Guide & Example

This file contains detailed writing guidelines and a complete example for creating a PRD Full.

---

## Writing Guidelines

### Context
Start with the broader business or product context, then add a **user-facing statement** (marketing-style, benefit-focused) at the end of this section. The user-facing statement should be concise (1-2 sentences) and describe the feature's value proposition.

**Example:**
```
Our platform helps teams collaborate on projects, but users struggle to understand project health at a glance. Leadership lacks visibility into blockers and progress across teams...

User-facing statement:
Get instant visibility into project health with smart dashboards and automated status updates — no more chasing teammates for updates.
```

### Problem
Be specific about what's broken or missing. Include:
- Current pain points (be concrete, name real scenarios)
- Why existing solutions fall short
- The core challenges that must be addressed

Avoid generic statements. Anchor to real user frustrations or business gaps.

### Usage Scenario
Provide concrete, named examples showing when and how users would interact with this feature. Use real personas, customer names, or specific scenarios. Multiple short scenarios work better than one long abstract description.

**Pattern:** `[Customer/persona name]: [specific situation] → [how they'd use the feature] → [outcome]`

### Solution
Describe how you'll solve the problem at a high level. This section should:
- Outline the approach without diving into implementation details
- Often includes phased delivery (v1/MVP, v2, future)
- Can include design principles or key constraints
- Use subsections for different versions or components

Keep focused on the "what" and "why" of the solution, not the detailed "how."

### Outcomes
Define measurable success criteria. Include both:
- **Quantitative metrics** (adoption rates, completion percentages, time saved)
- **Qualitative goals** (user confidence, reduced frustration, improved visibility)

Write outcomes as specific, testable statements starting with action verbs or measurements.

### References
List all supporting materials:
- Internal meeting notes (with dates and attendees)
- Customer calls or discovery sessions
- Design files, research documents
- Related technical specs or issue trackers
- Screenshots or current-state documentation

---

## Complete Example: Customer Feedback Loop System

This is a reference example of a well-structured PRD following the six-section format.

---

### Context

Our product collects customer feedback through multiple channels (support tickets, NPS surveys, in-app feedback widgets), but this data lives in silos. Product teams spend hours manually aggregating feedback, and valuable insights get lost or delayed. Customer-facing teams can't easily see whether reported issues have been addressed.

**User-facing statement:**
Transform scattered customer feedback into actionable product insights with automated aggregation, AI-powered theming, and closed-loop communication — so customers know their voice matters.

### Problem

Feedback aggregation is manual and inconsistent: product managers pull data from Zendesk, survey tools, and Slack channels into spreadsheets that quickly become stale.

Teams waste 5-10 hours weekly on feedback triage that could be automated. Critical patterns get missed because data isn't centralized.

Customer-facing teams (support, success) can't tell customers whether their feedback influenced the roadmap — damaging trust and reducing future feedback quality.

Existing solutions require dedicated analyst time or expensive third-party tools that don't integrate with our workflow.

Biggest challenges: centralizing feedback from disparate sources, surfacing patterns automatically, and closing the loop with customers when their feedback drives changes.

### Usage Scenario

**Sarah (Product Manager):** After a feature launch, Sarah wants to understand customer reaction. Instead of manually checking five different tools, she opens the feedback dashboard and sees auto-categorized themes. She identifies that 40% of feedback mentions "confusing onboarding flow" and creates a follow-up project directly from the insight.

**Marcus (Customer Success):** A key account mentioned frustration with export functionality six months ago. When the export redesign ships, Marcus gets an automatic notification that feedback from his accounts influenced this release. He reaches out to the customer with a personalized update, strengthening the relationship.

**Leadership (Quarterly Planning):** The exec team reviews a trend report showing feedback themes over time. They see "mobile experience" rising as a pain point over three quarters and prioritize mobile improvements for the next cycle.

**Support Team:** When responding to a ticket about a known issue, agents see that this issue is tagged as "in progress" on the roadmap and can share an estimated timeline with the customer.

### Solution

We'll deliver a centralized feedback hub with automated ingestion, AI-powered categorization, and closed-loop notifications.

#### v1 (MVP)

**Automated Ingestion**
- Connect to primary feedback sources: support tickets, NPS surveys, in-app feedback widget
- Daily sync with deduplication and source tracking
- Manual import option for ad-hoc feedback (CSV, copy-paste)

**AI-Powered Categorization**
- Auto-tag feedback by theme (e.g., "performance," "pricing," "UX")
- Sentiment detection (positive, negative, neutral)
- Duplicate/similar feedback clustering
- Manual override and custom tag creation

**Dashboard & Search**
- Filterable view by source, theme, sentiment, date range, customer segment
- Trend charts showing theme volume over time
- Export to CSV for offline analysis

**Basic Closed-Loop**
- Link feedback items to roadmap items/projects
- When a linked project ships, generate a list of customers whose feedback contributed

#### v2 (Enhanced)

- Slack/Teams integration for real-time feedback alerts on high-priority themes
- Customer-facing "feedback status" page showing shipped improvements
- Automatic outreach emails when feedback-linked features ship
- Integration with CRM to show feedback history on customer records
- Predictive insights: surface emerging themes before they become major issues

### Outcomes

Success means:

- 80% reduction in time spent manually aggregating feedback (from 8 hours/week to <2 hours)
- 90%+ of incoming feedback auto-categorized correctly (measured by spot-check audits)
- Product teams reference feedback dashboard in 70%+ of planning discussions
- 50%+ of customers whose feedback influenced a release receive proactive notification
- NPS improvement of 5+ points within two quarters of launch (customers feel heard)
- Support team reports 30% reduction in "any updates on my request?" follow-up tickets

### References

- June 12 – Customer Discovery (Enterprise accounts): "Feedback visibility challenges"
- June 18 – Internal (Product, Support, Success): "Current feedback workflow pain points"
- June 25 – Competitive analysis: Productboard, Canny, UserVoice feature comparison
- July 2 – Internal (Engineering): "Integration feasibility assessment"
- Customer interview recordings (5 sessions, June 2025)
- Current state documentation: Screenshots of existing spreadsheet trackers
- Design mockups: Figma link (feedback dashboard v1)
