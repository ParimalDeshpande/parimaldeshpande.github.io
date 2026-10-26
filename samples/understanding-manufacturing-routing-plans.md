---
layout: portfolio
title: Understanding Manufacturing Routing Plans
nav_title: Manufacturing Routing Plans
portfolio_sample: true
nav_order: 50
---

# Understanding Manufacturing Routing Plans

<p class="sample-type">Conceptual documentation · Manufacturing planners and application users</p>

<p class="sample-demonstrates"><strong>Demonstrates:</strong> Explaining relationships between complex product concepts, manufacturing scenarios, configuration choices, and their practical use.</p>

A routing plan defines how a manufacturing process is applied at a specific production facility. It connects three core elements:

- **Bill of materials:** Defines what is produced.
- **Process plan:** Defines the operations used to produce it.
- **Production facility:** Defines where the work is performed and which manufacturing resources are available.

A process plan can have multiple routing plans. This allows manufacturing planners to represent different production scenarios without creating unrelated copies of the complete process definition.

## Why routing plans are useful

Manufacturing requirements can vary based on production quantity, facility capacity, available equipment, or the intended production period. A routing plan represents the manufacturing configuration selected for a particular scenario.

For example, a low-volume production scenario might use one workstation and a manual tool. A high-volume scenario might use a different workstation, production line, or process plan.

Using separate routing plans allows planners to represent and compare these alternatives while maintaining their relationship to the underlying manufacturing process.

## Information in a routing plan

A routing plan can identify:

- The part or product to be manufactured
- The applicable process plan
- The production facility
- The production line, workstation, or work center
- Localized operations and manufacturing resources
- Supporting documents and routing attributes

The information available in a routing plan depends on the manufacturing model and the configuration of the production environment.

## Example production scenarios

Consider an assembly that can be manufactured in different quantities using different resources.

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

Both routing plans use the same part and process plan, but select different manufacturing resources for the two production scenarios.

A planner could also associate a different process plan with one of the routing plans when that scenario requires a different sequence of manufacturing operations.

## Routing plans and localized operations

A process plan provides the source operations used to manufacture a product. A routing plan can localize those operations for a particular production facility, production line, or manufacturing scenario.

Localization can account for differences such as:

- Available tools and resources
- Workstation layout
- Line-specific operation assignments
- Facility-specific processing requirements

For example:

```text
Source process plan
        |
        +-- Operation 10
        |
        +-- Operation 20
        |
        +-- Operation 30
                |
                v
        Routing plan
                |
                +-- Plant North
                +-- Line 1
                +-- Station 2
                +-- Automated Tool Set
```

The localized representation remains related to the source process plan, allowing planners to maintain facility-specific information while retaining the relationship to the original manufacturing definition.

## Keep routing plans synchronized

A routing plan can become out of date when its source process plan is revised independently.

For example:

```text
Process plan
Version A
     |
     +-- Routing plan created
     |
     v
Process plan revised
Version B
     |
     v
Routing plan may require reconciliation
```

Reconcile the routing plan with the current process plan before making additional localization changes when synchronization is required.

Reconciliation updates applicable information from the source operations to their localized representations. Depending on the system configuration, selected localized values can be preserved when they should not be replaced by values from the source process plan.

## Key considerations

When working with routing plans:

- Associate the routing plan with the part, process plan, and production facility required for the manufacturing scenario.
- Use multiple routing plans when the same manufacturing definition must support different facilities, resources, quantities, or production approaches.
- Keep routing plans synchronized with their source process plans when the source manufacturing definition changes.
- Consider configuration criteria that can affect which operations and structures are available in a routing plan.
- Review copied routing-plan information before using it for another production scenario.

Routing plans provide a way to connect product, process, facility, and resource information while allowing manufacturing planners to represent different production scenarios without maintaining completely independent process definitions.
