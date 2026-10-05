---

name: jira-ticket-description
description: >-
Transforms technical requirements, implementation notes, bug reports, or
informal descriptions into clear, structured, business-oriented Jira ticket
descriptions. Use when creating, rewriting, or improving Jira tickets,
requirements, acceptance criteria, business rules, or technical task
descriptions.
-------------

# Jira Ticket Description Skill

## Purpose

This skill is responsible for transforming technical prompts, requirements, implementation notes, bug reports, or informal descriptions into **clear, structured, and business-oriented Jira ticket descriptions**.

The generated description should focus on:

* The problem being addressed.
* The objective of the change.
* The expected system behavior.
* Business rules and constraints.
* Relevant edge cases.
* Testable acceptance criteria.

The description should provide enough detail for developers, QA, product, and other stakeholders to understand **what needs to be implemented and what behavior is expected**, without unnecessarily prescribing how the implementation should be done.

---

## Core Principles

### 1. Describe the work as something that needs to be implemented

The ticket represents work that is yet to be completed.

Use future-oriented or requirement-oriented language:

* "The system should..."
* "The application must..."
* "The system shall..."
* "When..."
* "If..."
* "In the absence of..."
* "The implementation should..."

Avoid describing the change as if it has already been completed:

* "This was implemented..."
* "The method was changed..."
* "We added..."
* "The system now..."
* "The implementation was updated..."

The ticket should describe the **desired state**, not the implementation history.

---

### 2. Prioritize business rules over implementation details

The description should explain **what the system must do**, rather than providing a step-by-step explanation of how the developer should implement it.

Avoid:

> Modify `SomeService#create()` to call `SomeRepository#countByUserId()` and then validate the result.

Prefer:

> Before creating a new record, the system must verify the number of existing records associated with the user and ensure that the configured limit has not been reached.

Implementation details should only be included when they are necessary to clarify the requirement or prevent ambiguity.

---

### 3. Do not prescribe an implementation unnecessarily

When multiple technical approaches could satisfy the requirement, describe the expected behavior instead of selecting a specific implementation.

The ticket should generally not dictate:

* Which class should be modified.
* Which method should be changed.
* Which repository or service should be called.
* Which design pattern should be used.
* The exact database query.
* The exact internal control flow.

These decisions should normally remain part of the implementation phase.

---

### 4. Do not invent requirements

Only include requirements that are explicitly stated or that can be directly inferred from the provided context.

Do not introduce:

* New business rules.
* New validations.
* New error behaviors.
* New permissions.
* New data structures.
* Assumptions about configuration precedence.
* Assumptions about expected system behavior.

If the prompt contains an unresolved question or ambiguity, preserve it as an ambiguity instead of inventing a solution.

For example, if the input says:

> "The fallback order is not defined."

Do not arbitrarily establish an order.

Instead, express the requirement generically:

> If a specific configuration is unavailable, the system must use the applicable fallback configuration according to the existing business rules.

---

## Recommended Structure

Whenever applicable, structure the Jira description using the following sections.

### Objective

Briefly describe the purpose of the change.

The objective should answer:

> Why is this change necessary?

Keep it concise and focused on the problem or expected outcome.

---

### Requirements

Describe the main behaviors that need to be implemented.

Each requirement should represent a meaningful functional expectation.

Example:

* The system must validate the current number of records associated with the user.
* The configured maximum must be respected during creation.
* When a specific configuration is unavailable, the applicable fallback configuration must be considered.
* Creation must be prevented when the configured limit has already been reached.

---

### Business Rules

Describe important rules, constraints, conditions, and exceptions explicitly.

Use conditional language such as:

* **When...**
* **If...**
* **If there is no...**
* **When the limit is reached...**
* **When the configuration is unavailable...**

Example:

> **Limit rule:** Before creating a new record, the system must verify the current number of records associated with the user against the applicable maximum.

> **Fallback rule:** If the primary configuration is unavailable, the system must use the applicable fallback value.

Business rules should focus on **observable behavior**, not implementation mechanics.

---

### Acceptance Criteria

Acceptance criteria must be objective, testable, and directly connected to the requirements.

Prefer the following format:

### Scenario: [Scenario name]

**Given** [initial context]

**When** [action]

**Then** [expected result]

Include both successful and unsuccessful scenarios whenever the requirement contains a meaningful boundary or restriction.

Example:

### Scenario: User is below the configured limit

**Given** the user has fewer records than the configured maximum

**When** a new record is created

**Then** the creation should be allowed.

### Scenario: User has reached the configured limit

**Given** the user has reached the configured maximum

**When** a new record creation is requested

**Then** the creation should be rejected.

---

## Level of Detail

The description should be **high-level but specific enough to remove ambiguity**.

### Include

* Business objective.
* Expected behavior.
* Functional requirements.
* Business rules.
* Relevant conditions and exceptions.
* Limits and constraints.
* Configuration precedence when explicitly defined.
* Relevant edge cases.
* Testable acceptance criteria.

### Avoid

* Line-by-line implementation details.
* Method-by-method descriptions.
* Repository or class references unless explicitly required.
* Source code.
* Commit history.
* Internal refactoring details.
* Unnecessary technical implementation decisions.
* Statements describing completed work.
* Vague acceptance criteria such as "the system should work correctly."

---

## Handling Technical Details

Technical details may be included when they are important to understand the requirement.

For example:

> The validation must be performed using the `user_id` associated with the request.

This is appropriate when the identifier is part of the business rule.

However, avoid turning the ticket into an implementation plan:

> Update `CredentialService` and call `CredentialRepository.countByUserId()` before invoking the creation method.

The preferred approach is to describe the required behavior:

> The system must verify the number of existing records associated with the requesting user before allowing a new record to be created.

---

## Handling Ambiguities

When the input contains uncertainty, do not convert assumptions into requirements.

If the prompt says:

> "The fallback order is not clear."

The generated ticket should not arbitrarily define the order.

Instead:

> The applicable fallback configuration must be used when the primary configuration is unavailable, following the existing business rule for configuration precedence.

If the ambiguity is critical to implementation or acceptance testing, explicitly identify it as something that must be clarified.

---

## Language and Style

The generated Jira description should be:

* Professional.
* Concise.
* Clear.
* Objective.
* Business-oriented.
* Technically accurate.
* Written in English unless another language is explicitly requested.
* Compatible with Jira Markdown.

Prefer terminology such as:

* "must"
* "should"
* "shall"
* "must not"
* "should not"
* "when applicable"
* "in the absence of"
* "when the limit is reached"
* "when the condition is satisfied"

Avoid unnecessary conversational language.

---

## Transformation Process

When receiving a prompt, follow this process internally:

### 1. Identify the objective

Determine:

* What problem is being solved?
* Why is the change necessary?
* What behavior should exist after implementation?

### 2. Identify the relevant domain entities

Extract the entities involved in the requirement without unnecessarily exposing internal implementation details.

Examples:

* User
* Account
* Order
* Credential
* Product
* Configuration
* Transaction

### 3. Extract business rules

Identify:

* Limits.
* Conditions.
* Restrictions.
* Fallbacks.
* Precedence rules.
* Required validations.
* Allowed and disallowed states.
* Expected outcomes.

### 4. Separate requirements from implementation

Convert technical instructions into behavioral requirements whenever possible.

For example:

> "Count records by user ID before creating."

Becomes:

> "The system must verify the number of existing records associated with the user before allowing a new record to be created."

### 5. Identify ambiguities

Do not make assumptions when the input does not define a behavior.

Preserve important ambiguities or indicate that the behavior should follow an existing domain rule.

### 6. Create acceptance criteria

Each important business rule should have at least one testable acceptance scenario.

Include:

* Positive scenarios.
* Negative scenarios.
* Boundary conditions.
* Fallback behavior when applicable.

### 7. Perform a final review

Before generating the final description, verify that:

* The ticket describes work that still needs to be implemented.
* The objective is clear.
* Business rules are explicit.
* Acceptance criteria are testable.
* No unsupported requirements were introduced.
* Implementation details have not unnecessarily replaced business requirements.
* The description is understandable without knowing the implementation history.

---

## Output Format

The default output should follow this structure:

```markdown
## Objective

[Brief description of the problem and objective]

## Requirements

- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

## Business Rules

- **Rule 1:** [Business rule]
- **Rule 2:** [Business rule]
- **Rule 3:** [Business rule]

## Acceptance Criteria

### Scenario: [Scenario name]

**Given** [initial context]

**When** [action]

**Then** [expected result]

### Scenario: [Scenario name]

**Given** [initial context]

**When** [action]

**Then** [expected result]
```

Only include sections that are relevant to the ticket. Do not add unnecessary sections simply to fill the template.

---

## Example Transformation

### Input

> We need to limit the number of credentials created per user. When creating a credential, validate the user and determine the maximum number of credentials allowed. If a specific limit is unavailable, use the applicable default configuration.

### Expected Output

```markdown
## Objective

Implement a limit on the number of credentials that can be created for each user, ensuring that credential creation respects the applicable configuration.

## Requirements

- The system must validate the number of existing credentials associated with the user before creating a new credential.
- The applicable maximum credential limit must be determined before creation.
- If a specific limit is unavailable, the applicable default configuration must be used.
- Credential creation must be prevented when the configured limit has already been reached.

## Business Rules

- **Per-user limit:** Credential creation must be controlled independently for each user.
- **Maximum limit:** The number of existing credentials must not exceed the applicable configured maximum.
- **Fallback configuration:** When a specific limit is unavailable, the applicable default configuration must be considered.
- **Limit reached:** When the user has reached the maximum allowed number of credentials, additional credential creation attempts must be rejected.

## Acceptance Criteria

### Scenario: User is below the configured limit

**Given** the user has fewer credentials than the applicable maximum

**When** a new credential creation is requested

**Then** the creation should be allowed.

### Scenario: User has reached the configured limit

**Given** the user has reached the applicable maximum number of credentials

**When** a new credential creation is requested

**Then** the creation must be rejected.

### Scenario: Specific configuration is unavailable

**Given** there is no specific limit available for the user

**When** the maximum credential limit is determined

**Then** the applicable default configuration must be used.
```

---

## Output Requirements

The final result produced by this skill must be a **Jira-ready description**.

Do not provide an explanation of how the description was generated unless explicitly requested.

The description should communicate the **business requirement and expected behavior**, while leaving unnecessary implementation decisions to the development phase.
