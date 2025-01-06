---
title: Labels
tags: [formatting]
keywords: labels, buttons, bootstrap, api methods
last_updated: July 3, 2025
sidebar: mydoc_sidebar
permalink: mydoc_client_sdk_insights.html
folder: mydoc
---

# Client SDKs Insights

### Potential Improvements

| Title | Description | Ticket or Note |
| --- | --- | --- |
| Java SDK Release Notes | Add the maven repository and maybe the version into the release notes on GitHub. | Investigate if it can be automated. |
| Proto Documentation | Add more descriptive comments on the Proto files that will help for autogenerate doc such as the JavaDocs one. | [BAM-28](https://kodypay.atlassian.net/browse/BAM-28) |
| Units inconsistency in Ecom and Terminal payment. | Consistency for ECOM and Terminal to use minor units.
Deprecate the String attribute.
 | [BAM-27](https://kodypay.atlassian.net/browse/BAM-27) |
| GroupId & Packaging | Match groupId to packaging structure, proposal name is 
com.kodypay.api.grpc.
At the moment the generated files path has the structure currently com.kodypay.grpc. | Future |
| Versioning | Following current structure of triggering the version from the protocols SDK, or independent versioning for each repository. | To be discussed. |