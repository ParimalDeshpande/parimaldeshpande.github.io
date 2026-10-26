---
layout: portfolio
title: Create a Routing Plan Using a REST API
nav_title: Create Routing Plan with REST API
portfolio_sample: true
nav_order: 30
---

# Create a Routing Plan Using a REST API

<p class="sample-type">API documentation · Developers and integration specialists</p>

<p class="sample-demonstrates"><strong>Demonstrates:</strong> Structuring REST API guidance with prerequisites, request examples, JSON payloads, object relationships, and verification.</p>

A routing plan connects a manufacturing process plan, a bill of materials, and a production facility. Use the manufacturing REST API to create routing plans and associate each routing plan with the applicable part and process plan.

## Before you begin

Obtain the identifiers of the objects that you want to associate with the routing plan:

- Production facility
- Process plan
- Part
- Navigation criteria for each referenced object

Also make sure you have:

- The target organization and product context
- A valid anti-forgery token required by the environment

The process plan, part, and production facility are required for a complete routing-plan association.

## Request

Send a `POST` request to the routing-plan creation endpoint:

```http
POST /api/manufacturing/routing-plans
```

Include the following request headers:

```http
Content-Type: application/json
X-CSRF-Token: <token>
```

## Understand the request structure

The request contains a `routingPlans` array. Each item in the array represents one routing plan and contains:

- `routingPlan`: Defines the routing plan, its production facility, and applicable configuration information.
- `assignments`: Defines the objects associated with the routing plan, such as the process plan and part.
- `navigationCriteriaId`: Identifies the navigation criteria applicable to the referenced object.

To create multiple routing plans in a single request, include multiple items in the `routingPlans` array.

## Create one routing plan

The following request creates a routing plan named **Assembly Routing Plan** and associates it with a process plan and a part:

```json
{
  "routingPlans": [
    {
      "routingPlan": {
        "name": "Assembly Routing Plan",
        "description": "Routing plan for the primary assembly line",
        "folder": "/Manufacturing/Routing Plans",
        "facilityId": "FACILITY-001",
        "navigationCriteriaId": "NAV-PLANT-001"
      },
      "assignments": [
        {
          "objectType": "processPlan",
          "objectId": "PROCESS-PLAN-001",
          "navigationCriteriaId": "NAV-PLAN-001"
        },
        {
          "objectType": "part",
          "objectId": "PART-001",
          "navigationCriteriaId": "NAV-PART-001"
        }
      ]
    }
  ]
}
```

A successful request creates:

- One routing plan
- One association between the routing plan and the process plan
- One association between the routing plan and the part

## Create multiple routing plans

To create multiple routing plans in one request, add another routing-plan object to the `routingPlans` array.

The following example creates separate routing plans for low-volume and high-volume production scenarios:

```json
{
  "routingPlans": [
    {
      "routingPlan": {
        "name": "Low-Volume Routing",
        "facilityId": "FACILITY-001",
        "navigationCriteriaId": "NAV-PLANT-001"
      },
      "assignments": [
        {
          "objectType": "processPlan",
          "objectId": "PROCESS-PLAN-001",
          "navigationCriteriaId": "NAV-PLAN-001"
        },
        {
          "objectType": "part",
          "objectId": "PART-001",
          "navigationCriteriaId": "NAV-PART-001"
        }
      ]
    },
    {
      "routingPlan": {
        "name": "High-Volume Routing",
        "facilityId": "FACILITY-002",
        "navigationCriteriaId": "NAV-PLANT-002"
      },
      "assignments": [
        {
          "objectType": "processPlan",
          "objectId": "PROCESS-PLAN-002",
          "navigationCriteriaId": "NAV-PLAN-002"
        },
        {
          "objectType": "part",
          "objectId": "PART-002",
          "navigationCriteriaId": "NAV-PART-002"
        }
      ]
    }
  ]
}
```

Each item in the `routingPlans` array is processed as a separate routing plan with its own production facility, process plan, part, and navigation criteria.

## Verify the result

After the request succeeds, verify that:

- Each routing plan was created in the intended product context.
- Each routing plan references the expected production facility.
- The expected part and process-plan associations are present.
- The navigation criteria return the intended manufacturing configuration.

If a routing plan is created without an expected association, verify the object identifiers, assignment objects, and navigation criteria in the request.
