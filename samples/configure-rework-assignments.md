---
layout: portfolio
title: Configure Rework Assignments
portfolio_sample: true
nav_order: 40
---

# Configure Rework Assignments in a Workflow

A standard change workflow can assign a rework task to every participant in an assignee role, even when only one participant completed the original work. Customize the workflow when rework must return only to the participant who completed the task.

This approach is useful when a team can accept a shared work item, but the person who performed the work should also correct it if the review identifies a problem.

## Intended outcome

Consider a change task assigned to three participants:

1. One participant accepts and completes the work task.
2. A reviewer evaluates the completed work.
3. The reviewer returns the task for rework.
4. The workflow assigns the rework task only to the participant who completed the original task.

The customized workflow determines the actual task completer and places that participant in a dedicated rework role.

## Before you begin

You should be familiar with:

- Workflow templates
- Life-cycle templates
- Object initialization rules
- Role configuration
- Server-side workflow expressions
- Basic Java programming

Always create a copy of a provided workflow template before customization. This preserves the original template and separates custom behavior from standard product behavior.

## Create a rework role

Create a role named **Rework Assignee** in the role configuration used by the workflow.

You can skip this step if an existing role already represents the participants responsible for rework.

## Copy the workflow template

1. Open the workflow template administration utility.
2. Locate the workflow used for change activities.
3. Create a copy of the template.
4. Give the copy a descriptive name, such as **Custom Change Activity Workflow**.
5. Open the copied template in the workflow editor.

## Update the rework task

1. Locate the rework loop in the workflow.
2. Open the rework activity.
3. In the participant configuration, add **Rework Assignee**.
4. Remove the general assignee role if all original assignees should not receive the rework task.
5. Save the activity configuration.

## Determine the rework assignee

Add an expression step immediately before the rework activity. The expression must:

1. Check whether the rework activity was completed previously.
2. If it was completed previously, retrieve the participants from its latest completed execution.
3. Otherwise, retrieve the participants who completed the original work activity.
4. Assign the retrieved participants to the **Rework Assignee** role.

The following pseudocode illustrates the logic without relying on product-specific classes:

```java
Role sourceRole = REWORK_ASSIGNEE;
Map<Role, Set<User>> participants =
    workflow.getLatestCompletedParticipants(changeItem, "Rework Task");

if (participants == null) {
    sourceRole = ASSIGNEE;
    participants =
        workflow.getLatestCompletedParticipants(changeItem, "Complete Work Task");
}

workflow.setRoleParticipants(
    changeItem,
    REWORK_ASSIGNEE,
    participants.get(sourceRole)
);
```

Replace the activity names with the exact names used in your workflow template.

## Validate the workflow

1. Check the expression syntax and correct any errors.
2. Save and check in the customized workflow template.
3. Associate the workflow with the applicable life-cycle configuration.
4. Update the object-creation rule to use the customized life cycle, if required by the system.
5. Test the workflow with multiple original assignees.

Verify the following scenarios:

- One participant completes the original work task.
- More than one participant completes the original work task.
- The task goes through multiple rework cycles.
- No completed activity is available for participant retrieval.

## Expected result

When a reviewer requests rework, the workflow assigns the new task to the participant or participants who completed the relevant prior activity. Other members of the original assignee group do not receive the rework task unless they also completed that activity.
