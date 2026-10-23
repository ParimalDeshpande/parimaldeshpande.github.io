---
layout: portfolio
title: HTTPS Connections
portfolio_sample: true
nav_order: 20
---

# Configure Outbound HTTPS Connections to External Services
Cloud applications often need to communicate with services that are hosted outside the managed application environment. To support these integrations securely, connection details should be maintained separately from customization code and configured for each deployment environment.

This approach allows the same integration code to be used across development, test, and production environments while keeping environment-specific URLs, credentials, secrets, and certificates separate.

## Before you begin

Before configuring an external connection, make sure you have:

- Access to the utility or configuration mechanism used to register external connections.
- The deployment-specific location where encrypted connection information is submitted.
- A public key or equivalent mechanism for encrypting sensitive connection information.
- A client certificate and key chain, if the external service requires client-certificate authentication.
- A server certificate chain, if the external service uses a certificate that is not trusted by the application's runtime by default.

Use certificates obtained from a trusted source. Store only the static information required to establish the connection, such as client identifiers, client secrets, tenant identifiers, or token issuer URLs. If the external service uses short-lived access tokens, obtain those tokens at runtime rather than storing them as static connection data.

## Configure deployment-specific connection information

1. Generate or obtain the template configuration file for external connections.
2. Create a configuration file for the target environment.
3. Define each external connection with a unique connection name and base URL.
4. Add optional connection information as required, such as secrets, client certificates, server certificates, or reusable URL fragments.
5. Specify the key that is used to encrypt sensitive connection information.
6. Submit the configuration using the supported external-connection utility.
7. If rollout scheduling is supported, specify when the updated connection information should become active.

Keep separate configuration files for development, test, and production environments. This prevents a non-production deployment from unintentionally connecting to a production endpoint.

## Example configuration

The following example illustrates the structure of an external-connection configuration file. The names and values are fictitious.

```json
{
  "external_connections": {
    "inventory.service": {
      "baseurl": "https://inventory.example.com",
      "secrets": {
        "client_id": "example-client-id",
        "client_secret": "example-client-secret"
      },
      "url_fragments": {
        "items": "api/items",
        "status": "api/status"
      }
    }
  },
  "encryption_key": "/path/to/public-key.txt"
}
```

## Use the connection from customization code

After the external connection information is registered, customization code can refer to the configured connection rather than embedding deployment-specific URLs or secrets in the code.

This separation provides two advantages:

- The same customization can be deployed to multiple environments.
- Connection information can be updated for an individual environment without creating environment-specific versions of the integration code.

## Updating connection information

When an endpoint URL, certificate, or secret changes, update the deployment-specific connection configuration and submit it again using the supported configuration mechanism.

If configuration changes are scheduled, allow for the platform's rollout process before relying on the new values. Until the update becomes active, the integration might continue to use the previously configured connection information.

## Example deployment model

| Environment | External endpoint |
| --- | --- |
| Development | `https://inventory-dev.example.com` |
| Test | `https://inventory-test.example.com` |
| Production | `https://inventory.example.com` |

The integration code can use the same logical connection name in every environment while the underlying endpoint and credentials remain environment-specific.

---

## About this writing sample

This portfolio sample is a generalized adaptation of published enterprise-software documentation. Product names, utility names, APIs, configuration parameters, URLs, credentials, and other implementation-specific details have been replaced or simplified for portfolio use.

The sample demonstrates my approach to:

- Explaining a complex integration concept before introducing configuration details.
- Separating prerequisites, configuration, examples, and update guidance into scannable sections.
- Presenting security-related information close to the task where it is needed.
- Using examples to clarify environment-specific configuration without exposing real deployment information.
- Writing technical content for administrators and developers in a task-oriented format.
