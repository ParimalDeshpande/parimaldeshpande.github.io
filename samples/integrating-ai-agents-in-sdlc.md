---
layout: portfolio
title: Integrating AI Agents into the Software Development Lifecycle
nav_title: AI Agents in the SDLC
portfolio_sample: true
nav_order: 60
---

# Integrating AI Agents into the Software Development Lifecycle

<p class="sample-type">Technical concept and workflow documentation · Software development teams</p>

<p class="sample-demonstrates"><strong>Demonstrates:</strong> Explaining AI-assisted development workflows, agents, reusable skills, repository context, testing, governance, and human review through an end-to-end example.</p>

AI-assisted development can support more than code generation. When integrated into the software development lifecycle (SDLC), AI agents can assist with requirements analysis, design, implementation, testing, code review, documentation, deployment, and maintenance.

A practical AI-assisted workflow can combine:

- AI agents for specialized development tasks
- Reusable skills for repeatable workflows
- Repository instructions and project context
- Approved tools and external data sources
- Different AI models based on task requirements
- Automated testing and validation
- Human review and approval

The objective is not to replace the existing SDLC. Instead, AI becomes another capability within the development workflow. Developers, reviewers, security specialists, product owners, technical writers, and other stakeholders remain responsible for validating artifacts and making decisions.

> **Important:** AI-generated output can be incomplete or incorrect. Treat generated requirements, designs, code, tests, and documentation as proposed artifacts that require appropriate review and validation.

## AI-assisted SDLC workflow

Different agents can specialize in stages of the SDLC while sharing approved project context.

For example:

```text
Feature Request
      |
      v
Requirements Agent
      |
      v
Planning Agent
      |
      v
Development Agent
      |
      v
Testing Agent
      |
      v
Code Review Agent
      |
      v
Documentation Agent
      |
      v
Human Approval
      |
      v
Deployment
```

This separation gives each agent a focused responsibility and provides clear points at which teams can review and validate the generated output.

## Example scenario

Consider a web application that displays search results.

Users request the following enhancement:

> Allow users to export filtered search results to a CSV file.

The following sections demonstrate how an AI-assisted workflow could support this feature throughout the SDLC.

## Requirements and planning

AI can help analyze an initial feature request before implementation begins.

For example, a requirements-focused agent could examine the following request:

```text
Allow users to export filtered search results to a CSV file.
```

The agent could identify questions such as:

- Should the export contain all matching results or only the currently displayed page?
- Should active filters and sorting be applied?
- Which columns should be exported?
- How should commas, line breaks, and special characters be handled?
- Which character encoding should be used?
- What should happen when no results are available?
- Is there a maximum number of records that can be exported?
- Should the export include only data that the current user is authorized to view?
- What should happen if the export cannot be completed?

After these questions are resolved, the agent can help organize the approved requirements.

For example:

```text
Feature: Export filtered search results

The application must allow users to export search results in CSV format.

Acceptance criteria:

- The export contains all results matching the active filters.
- The export uses the active sorting order.
- Only fields visible to the current user are included.
- The CSV file uses UTF-8 encoding.
- Column headings are included in the first row.
- Values containing commas, quotation marks, or line breaks are escaped.
- The export action is unavailable when no results exist.
- Export errors are reported to the user.
```

The product owner or requirements owner reviews and approves the final requirements before implementation begins.

## Design and implementation planning

After the requirements are approved, a planning agent can help translate them into an implementation plan.

For the CSV export feature, the plan might identify changes in the following areas:

```text
User interface
    |
    +-- Add an Export action
    +-- Disable the action when no results exist
    +-- Display export status and errors

Application API
    |
    +-- Add an export request
    +-- Pass active filters and sorting criteria

Service layer
    |
    +-- Apply active filters
    +-- Apply authorization rules
    +-- Retrieve matching records
    +-- Generate CSV data

Testing
    |
    +-- CSV generation
    +-- Filtering and sorting
    +-- Character escaping
    +-- UTF-8 encoding
    +-- Authorization
    +-- Empty results
    +-- Error handling

Documentation
    |
    +-- Export procedure
    +-- Export limitations
    +-- Troubleshooting information
    +-- Release notes
```

The implementation plan should be reviewed before an agent begins modifying the repository.

## Provide repository context

An agent needs enough project context to understand existing architecture, conventions, terminology, and constraints. Without that context, the agent might generate a technically plausible solution that does not fit the repository.

A repository can include AI-specific configuration alongside application code:

```text
project/
├── .github/
│   ├── agents/
│   ├── skills/
│   └── copilot-instructions.md
├── docs/
├── src/
├── tests/
└── README.md
```

These resources have different purposes:

- **Repository instructions** provide project-wide standards and context.
- **Custom agents** define specialized roles and workflows.
- **Agent skills** provide reusable instructions, scripts, templates, and reference material for specific tasks.
- **Tools** allow an agent to interact with approved systems or perform controlled actions.
- **Tests and validation scripts** verify proposed changes.

Repository instructions might describe:

- Application architecture
- Supported programming languages and frameworks
- Coding conventions
- Required test frameworks
- Security requirements
- Documentation standards
- Commands used to build and validate the project
- Files or directories that agents should not modify

Keep important project information in approved, version-controlled artifacts rather than relying only on information supplied through individual prompts.

## Use custom agents

A general-purpose AI assistant does not need to perform every SDLC activity in the same way.

Custom agents can provide specialized roles for particular development activities. For example, a repository might contain:

```text
.github/
└── agents/
    ├── requirements-reviewer.agent.md
    ├── implementation-planner.agent.md
    ├── development-agent.agent.md
    ├── test-reviewer.agent.md
    └── documentation-reviewer.agent.md
```

Each agent has a clearly defined responsibility.

| Agent | Primary responsibility |
| --- | --- |
| Requirements Reviewer | Identify missing, conflicting, or ambiguous requirements |
| Implementation Planner | Convert approved requirements into an implementation plan |
| Development Agent | Implement an approved plan within repository constraints |
| Test Reviewer | Review test coverage against approved behavior |
| Documentation Reviewer | Identify documentation impact and review content changes |

Keeping responsibilities separate prevents one agent from becoming responsible for the entire development process.

### Example documentation agent

A documentation-focused agent can identify documentation impact and review proposed documentation changes associated with a feature.

For example:

```markdown
---
name: Documentation Reviewer
description: Reviews documentation affected by software changes.
---

# Responsibilities

When reviewing a change:

1. Identify user-visible behavior introduced or modified by the change.
2. Determine which existing documentation could be affected.
3. Check terminology against existing documentation.
4. Identify missing prerequisites, limitations, and expected results.
5. Review procedures for clarity and completeness.
6. Identify documentation links that require validation.
7. Report unsupported statements or missing source information.

Do not change documented product behavior without supporting information.
Do not publish documentation without technical and editorial review.
```

The agent does not replace technical or editorial review. Its purpose is to provide a consistent first-pass analysis and identify issues for a human reviewer.

Custom agents can be defined through agent profiles that describe the agent's purpose and instructions and can restrict the tools available to the agent. Repository-level agent profiles can be stored in `.github/agents`. For current configuration details, see [Custom agents in GitHub Copilot](https://docs.github.com/en/copilot/concepts/customizations/custom-agents).

## Use agent skills

Agents define specialized roles, while skills provide reusable instructions and supporting resources for particular tasks.

A repository might contain:

```text
.github/
└── skills/
    ├── requirements-review/
    ├── test-generation/
    ├── documentation-review/
    └── release-notes/
```

For example, a documentation-review skill might contain:

```text
.github/
└── skills/
    └── documentation-review/
        ├── SKILL.md
        ├── references/
        │   └── style-guide.md
        └── scripts/
            └── validate-links.py
```

### Example documentation-review skill

```markdown
---
name: documentation-review
description: Reviews product documentation for structure, clarity, terminology, and common documentation issues.
---

# Documentation review

When reviewing documentation:

1. Check that the topic has a clear purpose.
2. Verify that prerequisites appear before procedural steps.
3. Check terminology for consistency.
4. Identify ambiguous or incomplete instructions.
5. Identify missing expected results.
6. Check whether user-visible limitations are documented.
7. Run the link-validation script when links are modified.
8. Report issues without changing the documented technical behavior.

Use the documentation style guide in `references/style-guide.md`
when performing the review.
```

A reusable skill prevents teams from having to provide the same task-specific instructions in every prompt. It also keeps relevant instructions, reference material, templates, and supporting scripts together.

For current details, see [Agent Skills in GitHub Copilot](https://docs.github.com/en/copilot/concepts/customizations/agent-skills).

## Understand the difference between agents and skills

Agents and skills complement each other.

```text
Custom agent
     |
     +-- Defines a role
     |
     +-- Defines overall instructions
     |
     +-- Controls available tools
     |
     +-- Uses relevant skills
              |
              +-- Task instructions
              +-- Scripts
              +-- Templates
              +-- Reference material
```

For example, a **Documentation Reviewer** agent might use:

- A documentation-review skill
- A terminology-checking skill
- A link-validation skill
- A release-note skill

A development agent might instead use:

- An API-development skill
- A test-generation skill
- A code-quality skill
- A security-review skill

This approach allows reusable capabilities to be shared across workflows without duplicating instructions in each agent definition.

## Connect agents to external tools

Some development workflows require information or actions outside the repository.

Depending on the approved development environment, an agent might need to:

- Retrieve an approved work item.
- Query a requirements-management system.
- Access an internal knowledge source.
- Run an external validation tool.
- Retrieve API specifications.
- Check the status of a build or deployment.

Tools and integrations should be limited to the agent's responsibility. For example, an agent that reviews requirements might need read access to a work-item system but should not automatically receive deployment permissions.

A simplified architecture can look like this:

```text
                         Agent
                           |
          +----------------+----------------+
          |                |                |
     Instructions        Skills           Tools
          |                |                |
     Repository         Scripts        MCP servers
       context          Templates      Approved APIs
          |             References     External systems
          |                |                |
          +----------------+----------------+
                           |
                           v
                      AI model
                           |
                           v
                   Proposed artifact
                           |
                           v
                Automated validation
                           |
                           v
                     Human review
```

In development environments that support Model Context Protocol (MCP) integration, MCP servers can provide agents with access to configured tools or contextual information. Access should still follow organizational security requirements and the principle of least privilege.

## Development

During development, a coding agent can use the approved requirements and implementation plan together with repository context.

Typical tasks can include:

- Locating relevant components
- Creating or updating application code
- Refactoring existing code
- Updating configuration
- Creating tests
- Explaining unfamiliar code
- Preparing changes for review

For the CSV export feature, the agent might receive instructions such as:

```text
Implement the approved CSV export plan.

Requirements:
- Apply active filters and sorting.
- Export only fields visible to the current user.
- Use UTF-8 encoding.
- Escape commas, quotation marks, and line breaks.
- Return a clear error when the export cannot be completed.

Constraints:
- Follow the existing service architecture.
- Do not add a new third-party dependency.
- Add unit tests for CSV formatting and authorization.
- Do not modify unrelated files.
```

The proposed implementation should follow the source-control, review, and branch-protection practices established for the project.

## Testing and quality assurance

AI can assist with generating or reviewing tests after implementation.

For the CSV export feature, test scenarios might include:

```text
Scenario 1: Successful export
Given search results are available
When the user exports the results
Then a CSV file containing the matching records is created.

Scenario 2: Filtered export
Given a search filter is active
When the user exports the results
Then only records matching the active filter are exported.

Scenario 3: Empty results
Given no search results are available
Then the export action is unavailable.

Scenario 4: Restricted fields
Given the user cannot access a restricted field
When the user exports the results
Then the restricted field is not included.

Scenario 5: Special characters
Given a value contains a comma, quotation mark, or line break
When the value is exported
Then the value is escaped correctly in the CSV file.

Scenario 6: Export failure
Given the export cannot be completed
When the user requests an export
Then the application reports the error without exposing sensitive details.
```

AI-generated tests should still be reviewed against the approved acceptance criteria. A large number of generated tests does not demonstrate adequate coverage if important behaviors are missing.

## Perform AI-assisted code review

After implementation and automated testing, AI-assisted review can provide another layer of analysis before human approval.

The review can evaluate questions such as:

- Does the implementation satisfy the approved acceptance criteria?
- Were unrelated files modified?
- Are authorization checks preserved?
- Is sensitive data excluded from the export?
- Are CSV values escaped correctly?
- Do the tests cover expected success and failure paths?
- Are errors handled appropriately?
- Does the change introduce user-visible behavior that requires documentation?

AI-assisted review complements rather than replaces required pull-request reviews, automated quality checks, security checks, and approvals.

## Include documentation in the SDLC

Documentation should be included in the feature workflow rather than handled only after development is complete.

For the CSV export feature, a documentation agent could identify the following impact:

```text
Feature change
     |
     +-- User documentation
     |      Add a procedure for exporting results
     |
     +-- API documentation
     |      Document the export API if externally available
     |
     +-- Release notes
     |      Describe the new export capability
     |
     +-- Troubleshooting
            Document common export failures
```

The documentation-review skill can then evaluate proposed content against established documentation standards.

For example:

```markdown
## Export search results

You can export search results to a CSV file for use in spreadsheet
and reporting applications.

Before exporting the results, apply the filters and sorting criteria
that you want to include.

1. Open the search results.
2. Apply the required filters.
3. Select **Export**.
4. Save the generated CSV file.

The export contains all results matching the active filters. It includes
only the fields that you have permission to view.
```

Technical and editorial reviewers should verify the documentation against the implemented behavior before publication.

## Use different AI models

An AI-assisted workflow does not need to depend on a single underlying large language model.

The agent provides the task definition, instructions, repository context, tools, and reusable skills. The selected model performs the model-based reasoning and generation.

Conceptually:

```text
User or workflow
       |
       v
AI agent
       |
       +-- Instructions
       +-- Repository context
       +-- Agent skills
       +-- Tools
       +-- External context
       |
       v
Selected AI model
       |
       v
Proposed artifact
       |
       v
Automated validation
       |
       v
Human review
```

Where the development platform supports model selection, teams can select an approved model based on task requirements, organizational policies, availability, and evaluation results.

For example, an AI development platform might provide access to models from providers such as:

- Anthropic Claude
- OpenAI GPT
- Other supported model families

Avoid designing the workflow around an assumption that one model will always be available or provide the best result for every task.

Keep important project knowledge in version-controlled instructions, skills, specifications, tests, and reference material. This makes the workflow more portable and reduces dependence on an individual model or prompt.

## End-to-end example

The CSV export enhancement can move through the following AI-assisted lifecycle:

```text
1. Feature request
      |
      v
   Export filtered results to CSV

2. Requirements Reviewer
      |
      +-- Identifies ambiguities
      +-- Proposes acceptance criteria
      |
      v
   Product owner approval

3. Implementation Planner
      |
      +-- Identifies affected components
      +-- Creates an implementation plan
      |
      v
   Engineering review

4. Development Agent
      |
      +-- Reads approved requirements
      +-- Uses repository instructions
      +-- Loads relevant skills
      +-- Implements the approved plan
      |
      v
   Proposed implementation

5. Testing
      |
      +-- Existing automated tests
      +-- Newly generated tests
      +-- Edge-case testing
      +-- Security validation
      |
      v
   Test results

6. Code Review
      |
      +-- AI-assisted review
      +-- Human technical review
      |
      v
   Approved implementation

7. Documentation
      |
      +-- Documentation impact analysis
      +-- User documentation
      +-- API documentation
      +-- Release notes
      |
      v
   Technical and editorial review

8. Deployment
      |
      +-- Required release approvals
      +-- Automated deployment controls
      |
      v
   Released feature

9. Maintenance
      |
      +-- Issue analysis
      +-- Test updates
      +-- Code updates
      +-- Documentation updates
```

AI participates throughout the workflow, but approval points remain explicit.

## Human review and governance

Introducing AI agents does not remove accountability from the development process.

Organizations should define which activities an agent may perform and which activities require human approval.

Human review is particularly important for:

- Requirements and product decisions
- Architecture decisions
- Security-sensitive changes
- Authentication and authorization
- Privacy-sensitive functionality
- Production configuration
- Destructive operations
- External-facing documentation
- Deployment approval

An AI agent can propose requirements, code, tests, or documentation, but the responsible stakeholder should approve the artifact according to the organization's established process.

## Security considerations

Before integrating AI agents into development workflows:

- Define which repositories and systems agents may access.
- Limit tools to those required for the agent's responsibility.
- Use read-only access where write access is unnecessary.
- Protect credentials, tokens, secrets, and sensitive data.
- Review external tools and integrations before enabling them.
- Validate generated code using established security practices.
- Require appropriate review before production deployment.
- Maintain source-control and pull-request protections.
- Record significant agent actions where auditing is required.
- Review AI-generated documentation before publication.

Do not place credentials or confidential information directly in prompts, instructions, or skill resources unless the approved platform and organizational policy explicitly support that use.

## Best practices

When integrating AI agents into the SDLC:

1. **Give agents clearly defined responsibilities.**  
   Use focused agents rather than making one agent responsible for the entire lifecycle.

2. **Keep approved project knowledge in version-controlled resources.**  
   Store reusable instructions, standards, specifications, and skills with the project when appropriate.

3. **Use skills for repeatable tasks.**  
   Package recurring activities such as requirements analysis, test generation, documentation review, and release-note preparation into reusable skills.

4. **Apply least-privilege access.**  
   Give agents only the context, repositories, systems, and tools required for their responsibilities.

5. **Validate generated artifacts.**  
   Requirements, code, tests, and documentation require appropriate verification.

6. **Separate generation from approval.**  
   An agent can propose a change, but approval should follow the established controls for that type of change.

7. **Design for model flexibility.**  
   Keep important workflows and project knowledge separate from the underlying AI model where practical.

## Summary

AI agents can support activities throughout the SDLC without replacing established engineering practices.

A structured AI-assisted workflow combines specialized agents, reusable skills, repository context, approved tools, automated validation, and appropriate AI models. Human review remains necessary for decisions and artifacts that require accountability.

The goal is not to automate the entire lifecycle. Instead, use AI where it can accelerate appropriate work while keeping requirements, engineering controls, security, documentation, and release decisions subject to the required validation and approval.

## References

- [Custom agents in GitHub Copilot](https://docs.github.com/en/copilot/concepts/customizations/custom-agents)
- [Agent Skills in GitHub Copilot](https://docs.github.com/en/copilot/concepts/customizations/agent-skills)
