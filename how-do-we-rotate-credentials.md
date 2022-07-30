# How do we rotate credentials

## Context
The software system must comply with security requirements and principles in terms of credentials and other
secrets management, provide capabilities of securing, auditing, and rotating those sensitive assets.

## Problem
Change of the credentials in the central storage is typically delayed from the change of the configuration state of
consumers of such credentials.

Each credential is used by a minimum of two components - service provider and service consumer. In more
complicated scenarios there are multiple consumers or even multiple service providers (clusters).

And since the update of credentials is a distributed transaction, if the single credential instance is used, then during the update process the interaction between components that require that credentials will be interrupted.

## Solution

The solution for this problem is to use two pairs of credentials - The Blue-Green approach.

**Green credentials** - are the freshest, the main set of credentials that are supposed to be used by service providers
and service consumers.

As a part of the rotation, the downstream components which are responsible for updating credentials for service
providers are supposed to insert **GREEN** credentials into the service provider.

As a part of the rotation, the downstream components which are responsible for updating configuration for service
consumers are supposed to get updated **GREEN** credentials and set them for use. Until that happens the existing
configuration in service consumer keeps working because the old **GREEN** credentials have not been terminated and
have become **BLUE** ones.

**Blue credentials** - are the second set of supplementary credentials that are populated by the values of **GREEN**
credentials before it gets updated during the rotation event. This set of credentials allows the service consumers to
retain access to the service providers until their configuration is not updated with the freshest **GREEN** credentials.

During the rotation process, the **GREEN** credentials become **BLUE**. Once the rotation transaction is complete, the set of **BLUE** credentials that existed before the rotation event ceases to exist.


|Blue-Green Credentials|Regular Credentials|
|-|-|
|![bg-creds](/blue-green-credentials/blue-green-creds.png)|![bg-creds](/blue-green-credentials/regular-creds.png)|

### Secret Rotation Components
The main responsibility of any rotation purposed component is to update the rotation object inside its storage.

Depending on the design the rotation purposed component may also upsert or cleanup credentials inside the
service provider component.

### Credentials Resolution for Service Consumers
Depending on the way of updating the service provider in the rotation cycle, the service consumer employs
different algorithms for credentials resolution.

#### For service consumers who see new GREEN credentials before they applied to the service provider
![bg-creds](/blue-green-credentials/blue-green-consumer-resolution-without-see.png)

#### For service consumers who DO NOT see new GREEN credentials before they applied to the service provider
![bg-creds](/blue-green-credentials/blue-green-consumer-resolution-rotation-dont-see.png)