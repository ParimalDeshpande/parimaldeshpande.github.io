---
layout: portfolio
title: Configure Rework Assignments in a Workflow
nav_title: Configure Rework Assignments
portfolio_sample: true
nav_order: 40
---

# Configure Rework Assignments in a Workflow

<p class="sample-type">Customization documentation · Application administrators and developers</p>

<p class="sample-demonstrates"><strong>Demonstrates:</strong> Presenting a customization scenario from problem and intended behavior through configuration, implementation logic, and validation.</p>

In a change workflow, a rework task can be assigned to every participant in an assignee role, even when only one participant completed the original task. Customize the workflow when the rework task must return only to the participant who completed the work.

Use this approach when a work item is assigned to multiple participants but rework should be assigned only to the participant who completed the original task.

## How the customization works

Consider a change task assigned to three participants:

1. One participant accepts and completes the work task.
2. A reviewer evaluates the completed work.
3. The reviewer returns the task for rework.
4. The workflow assigns the rework task only to the participant who completed the original task.

The customized workflow identifies the participant who completed the applicable task and assigns that participant to a dedicated **Rework Assignee** role.

If the workflow goes through another rework cycle, the workflow uses the participants from the latest completed rework activity.

## Before you begin

Before customizing the workflow, make sure you are familiar with:

- Workflow templates
- Life-cycle templates
- Object initialization rules
- Role configuration
- Server-side workflow expressions
- Basic Java programming

Create a copy of the provided workflow template before making changes. This preserves the original template and keeps customized behavior separate from standard workflow behavior.

## Create a rework role

Create a role named **Rework Assignee** in the role configuration used by the workflow.

You can skip this step if an existing role already represents the participants responsible for rework.

## Copy the workflow template

1. Open the workflow template administration utility.
2. Locate the workflow used for change activities.
3. Create a copy of the template.
4. Give the copied template a descriptive name, such as **Custom Change Activity Workflow**.
5. Open the copied template in the workflow editor.

## Update the rework task

1. Locate the rework loop in the workflow.
2. Open the rework activity.
3. Add **Rework Assignee** to the participant configuration.
4. Remove the general assignee role if the rework task should not be assigned to all original assignees.
5. Save the activity configuration.

## Determine the rework assignee

Add an expression step immediately before the rework activity.

Configure the expression to:

1. Determine whether the rework activity was previously completed.
2. If a previous rework activity exists, retrieve the participants from its latest completed execution.
3. Otherwise, retrieve the participants who completed the original work activity.
4. Assign the retrieved participants to the **Rework Assignee** role.

The following simplified pseudocode illustrates the assignment logic without relying on product-specific classes:

```java
Role sourceRole = REWORK_ASSIGNEE;

Map<Role, Set<User>> participants =
    workflow.getLatestCompletedParticipants(
        changeItem,
        "Rework Task"
    );

if (participants == null) {
    sourceRole = ASSIGNEE;

    participants =
        workflow.getLatestCompletedParticipants(
            changeItem,
            "Complete Work Task"
        );
}

workflow.setRoleParticipants(
    changeItem,
    REWORK_ASSIGNEE,
    participants.get(sourceRole)
);
```

Replace the activity names in the example with the exact activity names used in your workflow template.

## Validate the customization

Before testing the workflow:

1. Check the workflow expression syntax and correct any errors.
2. Save and check in the customized workflow template.
3. Associate the customized workflow with the applicable life-cycle configuration.
4. If required by your application configuration, update the object-creation rule to use the customized life cycle.

## Test the workflow

Test the customization with multiple original assignees.

Verify the following scenarios:

- One participant completes the original work task.
- More than one participant completes the original work task.
- The workflow goes through multiple rework cycles.
- No completed activity is available for participant retrieval.

For each scenario, verify that the rework task is assigned only to the participants who completed the applicable prior activity.

## Expected result

When a reviewer requests rework, the workflow assigns the rework task to the participant or participants who completed the applicable prior activity.

Other members of the original assignee group do not receive the rework task unless they also completed that activity.
