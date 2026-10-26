---
layout: portfolio
title: Configure Outbound HTTPS Connections to External Services
nav_title: Configure Outbound HTTPS
portfolio_sample: true
nav_order: 10
---

# Configure Outbound HTTPS Connections to External Services

<p class="sample-type">Configuration documentation · System administrators</p>

<p class="sample-demonstrates"><strong>Demonstrates:</strong> Explaining technical prerequisites, security-related configuration, procedural steps, and expected system behavior.</p>

Cloud applications often need to communicate with services that are hosted outside the managed application environment. To support these integrations securely, connection details should be maintained separately from customization code and configured for each deployment environment.

This separation allows you to use the same integration code across development, test, and production environments while maintaining environment-specific URLs, credentials, secrets, and certificates independently.

## Before you begin

Before configuring an external connection, make sure you have:

- Access to the utility or configuration mechanism used to register external connections.
- The deployment-specific configuration location or service used to submit encrypted connection information.
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
        "client_id": "<client-id>",
        "client_secret": "<client-secret>"
      },
      "url_fragments": {
        "items": "api/items",
        "status": "api/status"
      }
    }
  },
  "encryption_key": "<path-to-public-key>"
}
```

## Use the connection from customization code

After the external connection information is registered, customization code can refer to the configured connection rather than embedding deployment-specific URLs or secrets in the code.

This separation provides two advantages:

- The same customization can be deployed to multiple environments.
- Connection information can be updated for an individual environment without creating environment-specific versions of the integration code.

## Update connection information

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
