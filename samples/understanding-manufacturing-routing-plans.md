---
layout: portfolio
title: Understanding Manufacturing Routing Plans
nav_title: Manufacturing Routing Plans
portfolio_sample: true
nav_order: 50
---

# Understanding Manufacturing Routing Plans
<p class="sample-type">Conceptual documentation · Manufacturing planners and application users</p>

**Demonstrates:** Explaining relationships between complex product concepts, manufacturing scenarios, configuration choices, and their practical use.

A routing plan defines how a manufacturing process is applied at a specific production facility. It connects three core elements:

- **Bill of materials:** Defines what is produced.
- **Process plan:** Defines the operations used to produce it.
- **Production facility:** Defines where the work is performed and which manufacturing resources are available.

A single process plan can have multiple routing plans. This allows manufacturing planners to evaluate alternative production scenarios without creating unrelated copies of the complete process definition.

## Why routing plans are useful

Manufacturing requirements can change based on production quantity, facility capacity, available equipment, or the intended production period. A routing plan captures the manufacturing configuration selected for a particular scenario.

For example, a low-volume scenario might use one workstation and a manual tool. A high-volume scenario might use a different workstation, production line, or process plan. Separate routing plans let planners represent and compare these alternatives.

## Information contained in a routing plan

A routing plan identifies:

- The part or product to be manufactured
- The applicable process plan
- The production facility
- The production line, workstation, or work center
- Localized operations and manufacturing resources
- Supporting documents and routing attributes

The exact information available depends on the manufacturing model and the configuration of the production environment.

## Example production scenarios

Assume that a company manufactures the same assembly in two quantities.

### Low-volume production

```text
Part: Assembly A
Quantity: 100
Process plan: Standard Assembly Process
Facility: Plant North
Line: Line 1
Workstation: Station 1
Tool: Manual Tool Set
```

### High-volume production

```text
Part: Assembly A
Quantity: 1,500
Process plan: Standard Assembly Process
Facility: Plant North
Line: Line 1
Workstation: Station 2
Tool: Automated Tool Set
```

The routing plans use the same part and process plan but select different manufacturing resources for the two production quantities. A planner can also use a different process plan if the high-volume scenario requires a different sequence of operations.

## Routing plans and localized operations

A process plan provides the source operations. A routing plan can localize those operations for a particular production facility or line.

Localization can account for differences such as:

- Available tools and resources
- Workstation layout
- Line-specific operation assignments
- Facility-specific processing requirements

The localized representation remains related to the source process plan so that planners can compare the manufacturing definition with its facility-specific implementation.

## Keeping plans synchronized

A routing plan can become out of date when its source process plan is revised independently. Before localizing additional operations, reconcile the routing plan with the current process plan.

Reconciliation updates applicable values from source operations to localized operations. Depending on system configuration, selected operations can be excluded when their localized values must be preserved.

## Benefits

Routing plans help manufacturing teams:

- Evaluate alternative production scenarios before release
- Connect product, process, and facility information
- Select appropriate lines, work centers, and tools
- Validate manufacturing decisions earlier
- Maintain facility-specific operation details
- Compare low-volume and high-volume production approaches

## Key considerations

- A routing plan requires a part, process plan, and production facility to represent a complete manufacturing scenario.
- Multiple routing plans can be associated with the same process plan.
- Routing plans should remain synchronized with their source process plans.
- Configuration criteria can affect which operations and structures are displayed.
- A copied routing plan can provide a starting point for another production scenario, but the copied information should be reviewed before use.
