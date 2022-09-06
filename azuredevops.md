# Azure Devops CloudEvents Adapter

## This is a completely oppinionated approach and does not reflect anything official with cloudevents

This document describes how to convert
[Azure Devops Service Hook Events](https://docs.microsoft.com/en-us/azure/devops/service-hooks/events?view=azure-devops)
into a CloudEvents.

Azure Devops Webhook event documentation:
https://docs.microsoft.com/en-us/azure/devops/service-hooks/events?view=azure-devops

Each section below describes how to determine the CloudEvents attributes
based on the specified event.

## Build and Release

### Build Completed

| CloudEvents Attribute | Value                                                       |
| :-------------------- | :---------------------------------------------------------- |
| `id`                  | "id" value                                                  |
| `source`              | "resource.url" value                                        |
| `specversion`         | `1.0`                                                       |
| `type`                | `com.azuredevops.build.complete.` + "resource.status" value + "resource.reason" |
| `datacontentencoding` | Omit                                                        |
| `datacontenttype`     | `application/json`                                          |
| `dataschema`          | Omit                                                        |
| `subject`             | "resource.buildNumber" value                                |
| `time`                | "resource.finishTime" value                                 |
| `data`                | Content of HTTP request body                                |

### Release Abandoned

| CloudEvents Attribute | Value                                          |
| :-------------------- | :--------------------------------------------- |
| `id`                  | "id" value                                     |
| `source`              | "resource.release.releaseDefinition.url" value |
| `specversion`         | `1.0`                                          |
| `type`                | `com.azuredevops.release.abandoned`            |
| `datacontentencoding` | Omit                                           |
| `datacontenttype`     | `application/json`                             |
| `dataschema`          | Omit                                           |
| `subject`             | "resource.release.id" value                    |
| `time`                | "resource.release.modifiedOn" value            |
| `data`                | Content of HTTP request body                   |

### Release Created

| CloudEvents Attribute | Value                                          |
| :-------------------- | :--------------------------------------------- |
| `id`                  | "id" value                                     |
| `source`              | "resource.release.releaseDefinition.url" value |
| `specversion`         | `1.0`                                          |
| `type`                | `com.azuredevops.release.created`              |
| `datacontentencoding` | Omit                                           |
| `datacontenttype`     | `application/json`                             |
| `dataschema`          | Omit                                           |
| `subject`             | "resource.release.id" value                    |
| `time`                | "resource.release.modifiedOn" value            |
| `data`                | Content of HTTP request body                   |

### Release deployment approval completed / Release deployment approval pending

| CloudEvents Attribute | Value                                                                  |
| :-------------------- | :--------------------------------------------------------------------- |
| `id`                  | "id" value                                                             |
| `source`              | "resource.release.releaseDefinition.url" value                         |
| `specversion`         | `1.0`                                                                  |
| `type`                | `com.azuredevops.release.approval.` + "resource.approval.status" value |
| `datacontentencoding` | Omit                                                                   |
| `datacontenttype`     | `application/json`                                                     |
| `dataschema`          | Omit                                                                   |
| `subject`             | "resource.approval.id" value                                           |
| `time`                | "resource.approval.modifiedOn" value                                   |
| `data`                | Content of HTTP request body                                           |

### Release deployment completed

| CloudEvents Attribute | Value                                                                               |
| :-------------------- | :---------------------------------------------------------------------------------- |
| `id`                  | "id" value                                                                          |
| `source`              | "resource.release.releaseDefinition.url" value                                      |
| `specversion`         | `1.0`                                                                               |
| `type`                | `com.azuredevops.release.deployment` + "resource.deployment.deploymentStatus" value |
| `datacontentencoding` | Omit                                                                                |
| `datacontenttype`     | `application/json`                                                                  |
| `dataschema`          | Omit                                                                                |
| `subject`             | "resource.deployment.id" value                                                      |
| `time`                | "resource.deployment.completedOn" value                                             |
| `data`                | Content of HTTP request body                                                        |

### Release deployment started

| CloudEvents Attribute | Value                                                                      |
| :-------------------- | :------------------------------------------------------------------------- |
| `id`                  | "id" value                                                                 |
| `source`              | "resource.release.releaseDefinition.url" value                             |
| `specversion`         | `1.0`                                                                      |
| `type`                | `com.azuredevops.release.deployment` + "resource.environment.status" value |
| `datacontentencoding` | Omit                                                                       |
| `datacontenttype`     | `application/json`                                                         |
| `dataschema`          | Omit                                                                       |
| `subject`             | "resource.environment.id" value                                            |
| `time`                | "resource.environment.modifiedOn" value                                    |
| `data`                | Content of HTTP request body                                               |

## Pipelines

### Run state changed

| CloudEvents Attribute | Value                                                                                  |
| :-------------------- | :------------------------------------------------------------------------------------- |
| `id`                  | "id" value                                                                             |
| `source`              | "resource.pipeline.url" value                                                          |
| `specversion`         | `1.0`                                                                                  |
| `type`                | `com.azuredevops.pipelines.run.state.` + "resource.run.state" value                    |
| `datacontentencoding` | Omit                                                                                   |
| `datacontenttype`     | `application/json`                                                                     |
| `dataschema`          | Omit                                                                                   |
| `subject`             | "resource.pipeline.id" value                                                           |
| `time`                | "resource.run.finishedDate" value, unless "null", then "resource.run.createDate" value |
| `data`                | Content of HTTP request body                                                           |

### Run stage state changed

| CloudEvents Attribute | Value                                                                                  |
| :-------------------- | :------------------------------------------------------------------------------------- |
| `id`                  | "id" value                                                                             |
| `source`              | "resource.pipeline.url" value                                                          |
| `specversion`         | `1.0`                                                                                  |
| `type`                | `com.azuredevops.pipelines.stage.state.` + "resource.stage.state" value                |
| `datacontentencoding` | Omit                                                                                   |
| `datacontenttype`     | `application/json`                                                                     |
| `dataschema`          | Omit                                                                                   |
| `subject`             | "resource.pipeline.id" value                                                           |
| `time`                | "resource.run.finishedDate" value, unless "null", then "resource.run.createDate" value |
| `data`                | Content of HTTP request body                                                           |

### Run stage waiting for approval / Run Stage approval completed

| CloudEvents Attribute | Value                                                                          |
| :-------------------- | :----------------------------------------------------------------------------- |
| `id`                  | "id" value                                                                     |
| `source`              | TDB                                                                            |
| `specversion`         | `1.0`                                                                          |
| `type`                | `com.azuredevops.pipelines.stage.approval.` + "resource.approval.status" value |
| `datacontentencoding` | Omit                                                                           |
| `datacontenttype`     | `application/json`                                                             |
| `dataschema`          | Omit                                                                           |
| `subject`             | TDB                                                                            |
| `time`                | "resource.approval.lastModified" value                                         |
| `data`                | Content of HTTP request body                                                   |

## Code

### Code checked in

| CloudEvents Attribute | Value                          |
| :-------------------- | :----------------------------- |
| `id`                  | "id" value                     |
| `source`              | "resource.url" value           |
| `specversion`         | `1.0`                          |
| `type`                | `com.azuredevops` + "eventType" value |
| `datacontentencoding` | Omit                           |
| `datacontenttype`     | `application/json`             |
| `dataschema`          | Omit                           |
| `subject`             | "resource.changesetId" value   |
| `time`                | "resource.createdDate" value   |
| `data`                | Content of HTTP request body   |

### Code pushed

| CloudEvents Attribute | Value                           |
| :-------------------- | :------------------------------ |
| `id`                  | "id" value                      |
| `source`              | "resource.repository.url" value |
| `specversion`         | `1.0`                           |
| `type`                | `com.azuredevops` + "eventType" value     |
| `datacontentencoding` | Omit                            |
| `datacontenttype`     | `application/json`              |
| `dataschema`          | Omit                            |
| `subject`             | "resource.pushId" value         |
| `time`                | "resource.date" value           |
| `data`                | Content of HTTP request body    |

### Pull request created

| CloudEvents Attribute | Value                                      |
| :-------------------- | :----------------------------------------- |
| `id`                  | "id" value                                 |
| `source`              | "resource.repository.url" value            |
| `specversion`         | `1.0`                                      |
| `type`                | `com.azuredevops` + "eventType" value      |
| `datacontentencoding` | Omit                                       |
| `datacontenttype`     | `application/json`                         |
| `dataschema`          | Omit                                       |
| `subject`             | "resource.pullRequestId" value             |
| `time`                | "createdDate" value                        |
| `data`                | Content of HTTP request body               |

### Pull request merge commit created

| CloudEvents Attribute | Value                                     |
| :-------------------- | :---------------------------------------- |
| `id`                  | "id" value                                |
| `source`              | "resource.repository.url" value           |
| `specversion`         | `1.0`                                     |
| `type`                | `com.azuredevops` + "eventType" value   |
| `datacontentencoding` | Omit                                      |
| `datacontenttype`     | `application/json`                        |
| `dataschema`          | Omit                                      |
| `subject`             | "resource.pullRequestId" value            |
| `time`                | "resourceclosedDate" value                |
| `data`                | Content of HTTP request body              |

### Pull request updated

| CloudEvents Attribute | Value                                                                |
| :-------------------- | :------------------------------------------------------------------- |
| `id`                  | "id" value                                                           |
| `source`              | "resource.repository.url" value                                      |
| `specversion`         | `1.0`                                                                |
| `type`                | `com.azuredevops` + "eventType" value                          |
| `datacontentencoding` | Omit                                                                 |
| `datacontenttype`     | `application/json`                                                   |
| `dataschema`          | Omit                                                                 |
| `subject`             | "resource.pullRequestId" value                                       |
| `time`                | "resource.closedDate" value, unless "null", then "createdDate" value |
| `data`                | Content of HTTP request body                                         |

## Work Items

TDB
