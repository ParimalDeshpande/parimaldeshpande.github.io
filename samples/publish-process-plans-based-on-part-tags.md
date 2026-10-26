---
layout: portfolio
title: Publish Process Plans Based on Part Tags
nav_title: Publish by Part Tags
portfolio_sample: true
nav_order: 20
---

# Publish Process Plans Based on Part Tags
<p class="sample-type">Feature and integration documentation · Manufacturing and integration users</p>

<p class="sample-demonstrates"><strong>Demonstrates:</strong> Explaining complex product behavior, first-time and incremental publishing scenarios, structured output, and examples.

Part tags provide a way to filter a process plan when the plan is associated with multiple parts or when the same part is used across multiple assembly branches. After a part tag is assigned, you can filter the process plan structure to display only the objects associated with the selected tag.

The publishing system can use these tags to generate separate process plan structures for each associated part. When tag-based publishing is enabled, a separate representation of the process plan and its applicable structure is included in the published output for each part tag.

## Enable publishing based on part tags

To publish a process plan based on part tags, set **Publish Based on Part Tags** to **Yes** when creating the process plan or editing its common attributes.

By default, **Publish Based on Part Tags** is set to **No**.

Consider the following requirements:

- When **Publish Based on Part Tags** is set to **Yes**, you must assign at least one part tag before publishing the process plan.
- You cannot change the **Publish Based on Part Tags** setting after the process plan has been published.

The following examples show how tag-based publishing works when **Publish Based on Part Tags** is set to **Yes**.

## Example 1: First-time publishing

Consider a process plan associated with two parts:

- `Part_A`
- `Part_B`

The process plan contains the following operations:

```text
ProcessPlan
├── Operation1 (Part Tag: Part_A)
└── Operation2 (Part Tag: Part_B)
```

When the process plan is published for the first time, the published output contains:

- Two process plans, one for each associated part tag
- Two operations, one for each associated part tag
- Two parts
- Two part-to-process-plan links

Objects associated with a part tag contain a unique `PartTagId` in the published output. This identifier distinguishes the objects associated with one tagged process plan from objects associated with another.

As a result, changes to objects associated with one part tag do not affect the publishing activities associated with other part tags in the original process plan.

## Example 2: Incremental publishing

Assume that the original process plan is updated as follows:

```text
ProcessPlan
├── Operation1 (Part Tag: Part_A)
├── Operation2 (Part Tag: Part_B)
├── Operation3 (Part Tag: Part_B)
└── Operation4 (No Part Tag)
```

In this example:

- `Operation3` is added for `Part_B`.
- `Operation4` does not have a part tag and therefore applies to both tagged process plans.
- `Operation1` and `Operation2` are unchanged.

During incremental publishing, the published output contains:

- Two revised process plans, one for each associated part tag
- `Operation3` for `Part_B`
- `Operation4` for both part tags

Because `Operation1` and `Operation2` have not changed, they are not republished as changed operations.

## Identifying tagged objects in the published output

The publishing system adds a unique `PartTagId` to process plan objects and applicable child objects or associated links.

For example, an associated document link can contain the tag identifier:

```xml
<DocumentLink>
    <ObjectID>DOC-LINK-001</ObjectID>
    <DocumentNumber>DOC-001</DocumentNumber>
    <DocumentVersion>A</DocumentVersion>
    <AssociatedObject>Operation2</AssociatedObject>
    <PartTagId>TAG-002</PartTagId>
    <TargetID>TARGET-001</TargetID>
</DocumentLink>
```

The process plan associated with the same tag contains the corresponding identifier:

```xml
<ProcessPlan>
    <ObjectID>PROCESS-PLAN-001</ObjectID>
    <Number>PP-001</Number>
    <Name>Manufacturing Process Plan</Name>
    <Category>production</Category>
    <Version>A</Version>
    <Iteration>2</Iteration>
    <PartTagId>TAG-002</PartTagId>
    <TargetID>TARGET-001</TargetID>
</ProcessPlan>
```

The `PartTagId` identifies which tag-specific process plan an object belongs to in the published output.
