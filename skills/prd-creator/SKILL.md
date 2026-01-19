---
name: prd-creator
description: Create Product Requirements Documents (PRDs). Use when someone asks to "create a PRD," "write a PRD," "help me document this feature," "turn these notes into a PRD," "document requirements," or "spec out this feature." Supports two templates - PRD Full (6-section format for strategic, multi-faceted features requiring stakeholder alignment) and PRD Lite (4-section format for tactical, well-bounded implementations).
user-invocable: true
allowed-tools: Read, Write, Glob, Grep
---

# PRD Creator

Create Product Requirements Documents using proven templates.

## Example Prompts

### 1. "Create a PRD for the AI coaching feature"
> Asks which template (Full or Lite), gathers requirements, generates structured document

### 2. "Turn these notes into a PRD"
> Transforms rough feature notes into properly formatted PRD

### 3. "Write a quick PRD for this bug fix enhancement"
> Uses PRD Lite for tactical, well-bounded scope

## Choosing a Template

When asked to create a PRD, first determine which template to use:

**Ask the user:** "Do you want a **PRD Full** (for strategic, multi-faceted features) or a **PRD Lite** (for tactical, well-bounded implementations)?"

**PRD Full** - For strategic, multi-faceted features requiring stakeholder alignment. Use when:
- The feature is strategically important to product/business direction
- Multiple components or systems need to work together
- You need to build shared understanding of the "why" across teams
- Success depends on measurable business outcomes (not just "it works")
- The solution may evolve through multiple phases or versions

**PRD Lite** - For tactical, well-bounded implementations. Use when:
- The scope is clear and limited to a specific area
- You mainly need to capture requirements and acceptance criteria
- The change is straightforward (even if technically complex)
- Quick validation and iteration is the goal
- A single team can own the decision

**Key insight:** The discriminator isn't build time but rather strategic vs tactical scope, context required for stakeholders, and risk/uncertainty level.

If unsure, default to asking the user.

## PRD Full Structure

Six sections in this order:

1. **Context** - What is the goal? Why now? Include a user-facing marketing statement.
2. **Problem** - What user or business problem are we solving? Why does it matter?
3. **Usage Scenario** - Real-life examples of when/why users would use this feature.
4. **Solution** - High-level description of how we plan to solve the problem.
5. **Outcomes** - What does success look like? Define KPIs and metrics.
6. **References** - Links to designs, research, feedback, or related documents.

## PRD Lite Structure

Four sections in this order:

1. **Problem** - Brief statement of what needs to be solved
2. **Solution** - High-level description of what you're building (include relevant links)
3. **Requirements** - Dynamic table with requirement details (columns vary based on needs)
4. **Acceptance Criteria** - Bulleted list of testable conditions

## Creating a PRD

When asked to create a PRD:

1. **Gather information** - Ask clarifying questions if the user's request lacks detail about the problem, users, or solution approach.

2. **Use the appropriate structure** - PRD Full has six sections, PRD Lite has four sections. Always include all sections in order.

3. **Consult the reference guide** - Load the detailed writing guide for your chosen template:
   - **PRD Full**: Review `references/prd-full.md` for detailed section-by-section guidelines and a complete example
   - **PRD Lite**: Review `references/prd-lite.md` for detailed section-by-section guidelines and a complete example

4. **Write in prose, not bullets** - PRDs are narrative documents. Use paragraphs and complete sentences. Bullets are fine within sections (especially Solution and Outcomes) but avoid making the entire document a bulleted list.

5. **Deliver as markdown** - Create a `.md` file for easy editing and version control.

