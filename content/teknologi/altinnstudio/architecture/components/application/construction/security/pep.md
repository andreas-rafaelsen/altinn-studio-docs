---
title: Policy Enforcment Point
linktitle: PEP
description: Description of Policy Enforcment Point for Altinn Studio Apps
tags: [architecture, security]
toc: true
---

There will be some different Policy Enforcement Points in Altinn Platform depending on the service in platform. 

## Standard PEP

[See GitHub](https://github.com/Altinn/altinn-studio/issues/2554).

Attribute based authorization is best solved with
[Policy Based Authorization in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/security/authorization/policies?view=aspnetcore-3.0)

The Policy Enforcement Point in the ASP.Net Web application template is created as a
[Authorization Handler](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Security/Authorization/Core/src/AuthorizationHandler.cs).

In the App there is defined a set of
[AuthorizationRequirements](https://github.com/aspnet/AspNetCore/blob/release/3.0/src/Security/Authorization/Core/src/IAuthorizationRequirement.cs) 
and for each operation of the different API endpoints needs to be configured with the correct requirement.

Example on requirements are:

- **InstanceRead** (User/system needs to be authorized to perform read action on the instance in current state)
- **InstanceWrite** (User/system needs to be authorized to perform write action on the instance and its data in current state)
- **InstanceInstantiate** (user/system needs to be authorized to Instantiate an instance for an app)

The PEP will based on route data (like instanceId) and the authenticated Identity create a
[decision request](https://github.com/Altinn/altinn-studio/blob/master/src/Altinn.Platform/Altinn.Platform.Authorization/IntegrationTests/Data/Xacml/3.0/AltinnApps/AltinnApps0007Request.json) and call PDP.
Based on the [response](https://github.com/Altinn/altinn-studio/blob/master/src/Altinn.Platform/Altinn.Platform.Authorization/IntegrationTests/Data/Xacml/3.0/AltinnApps/AltinnApps0007Response.json) the PEP will deny or approve the user. (Deny = http 403)

The PEP validates any obligation from the PDP like minimum authentication level. If this is not valid, the request will be denied (HTTP 403).

## Custom PEP

TODO
