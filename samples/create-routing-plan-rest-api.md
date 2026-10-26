---
layout: portfolio
title: Create a Routing Plan Using a REST API
nav_title: Using a REST API
portfolio_sample: true
nav_order: 30
---

# Create a Routing Plan Using a REST API
<p class="sample-type">API documentation · Developers and integration specialists</p>

**Demonstrates:** Structuring REST API guidance with prerequisites, request examples, JSON payloads, object relationships, and verification.

A routing plan connects a manufacturing process plan, a bill of materials, and a production facility. Use the manufacturing REST API to create one or more routing plans and associate each plan with the applicable part and process plan.

## Before you begin

Obtain the following information before sending the request:

- Identifier of the production facility
- Identifier of the process plan
- Identifier of the part
- Navigation criteria identifier for each referenced object
- Target organization and product context
- Valid anti-forgery token required by your environment

The process plan, part, and production facility are required for a complete routing-plan association.

## Endpoint

Send a `POST` request to the routing-plan creation endpoint:

```http
POST /api/manufacturing/routing-plans
```

## Request headers

```http
Content-Type: application/json
X-CSRF-Token: <token>
```

## Create one routing plan

The following request creates one routing plan and associates it with a process plan and a part:

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

To create multiple routing plans in one request, add another routing-plan object to the `routingPlans` array:

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

Each item in the array is processed as a separate routing plan with its own facility, process plan, part, and navigation criteria.

## Verify the result

After the request succeeds, verify that:

- Each routing plan was created in the intended product context.
- Each routing plan references the correct production facility.
- The expected part and process-plan associations are present.
- The navigation criteria return the intended manufacturing configuration.

If a routing plan is created without the expected association, verify that the request contains the correct process-plan identifier and assignment object.
