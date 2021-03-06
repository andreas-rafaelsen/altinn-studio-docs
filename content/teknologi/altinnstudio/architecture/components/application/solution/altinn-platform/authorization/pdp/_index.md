---
title: Policy Decision Point
linktitle: PDP
description: Description of the Authoirzation architecture
tags: [architecture, security]
---

The Policy Decision Point is implemented in the 
[authorization application](https://github.com/Altinn/altinn-studio/issues/1166) 
that is deployed Altinn Platform. 

The Policy Decision Point follow [eXtensible Access Control Markup Language (XACML) Version 3.0](https://docs.oasis-open.org/xacml/3.0/xacml-3.0-core-spec-os-en.html). 

This mean that the rules are defined in XACML Policies files and PDP evalutes request based on the rules. 

The PDP evaluates the Context Request based on standard XACML 3.0 behaviour. There is no specific Altinn behaviour.

Policy Decision Point exposes a method that authorize the decision request. 

PDP uses the configured [Context Handler](../contexthandler) to enrich the decision request with attributes about the subject, resource and environment. 


If instanceID or dataID is used as Resource ID PDP will use the Context Handler to identifiy the correct appId, 
the instance workflow state and the reporteId for the existing resource.

For request for non existing instances the appId will be used and the reportee is a required input.

## PDP flow

The diagram below show the detailed flow.

![PDP flow](pdpflow.svg "PDP flow")

Flow explained

1. Decision Request is sent to context handler for enriching
2. Context handler call PIP for resource attributes
3. Context handler call PIP for subject attributes
4. PRP Identifies the correct policy based on resource attributes in enriched decision request
5. PDP identifyes matching rules
6. PDP checks if there is any matching rules and returnes interderminate if not
7. Matches the rules with subject attributes
8. Verify match and return indeterminate if not
9. Evaluate any conditions in policy
10. If condiation does not match return indterminate
11. Add any obligations to the result
12. Return the decsion result


## XACML Conformance
The PDP tries to follow XACML standard and have implemented some conformance tests. The goal is to fully implement
all conformance test. See test here. 
