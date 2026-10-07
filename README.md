# jira-ticket-writer-skill

An agent skill that turns technical requirements, implementation notes, bug reports, or informal descriptions into clear, structured, business-oriented Jira ticket descriptions.

Generated tickets focus on the problem, the objective, the expected behavior, business rules, edge cases, and testable acceptance criteria — without prescribing implementation details unnecessarily.

## Installation

```bash
npx skills add matheuspedrososeg/jira-ticket-writer-skill
```

## Usage

Once installed, the agent uses the skill automatically when you ask it to create, rewrite, or improve a Jira ticket. For example:

> Write a Jira ticket: users should not be able to create more than the configured limit of projects; show an error when they try.

## Output structure

```markdown
## Objective
## Requirements
## Business Rules
## Acceptance Criteria
### Scenario: ...
```

See [SKILL.md](SKILL.md) for the full guidelines.
