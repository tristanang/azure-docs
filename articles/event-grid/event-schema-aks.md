---
title: Azure Kubernetes Service as Event Grid source
description: This article describes how to use Azure Kubernetes Service as an Event Grid event source. It provides the schema and links to tutorial and how-to articles. 
author: zr-msft
ms.topic: conceptual
ms.date: 12/02/2022
ms.author: zarhoads
---

# Azure Kubernetes Service (AKS) as an Event Grid source

This article provides the properties and schema for AKS events. It also gives you a list of quick starts and tutorials to use AKS as an event source. For an introduction to event schemas, see [Azure Event Grid event schema](event-schema.md) and [Cloud event schema](cloud-event-schema.md).

## Available event types

AKS emits the following event types

|    Event Type                                             |    Description                                                       |
|-----------------------------------------------------------|----------------------------------------------------------------------|
| Microsoft.ContainerService.NewKubernetesVersionAvailable  | Triggered when the list of available Kubernetes versions is updated. |
| Microsoft.ContainerService.ClusterSupportEnded            ||
| Microsoft.ContainerService.ClusterSupportEnding           ||
| Microsoft.ContainerService.NodePoolRollingStarted         ||
| Microsoft.ContainerService.NodePoolRollingSucceeded       ||
| Microsoft.ContainerService.NodePoolRollingFailed          ||

## Properties common to all events

# [Event Grid event schema](#tab/event-grid-event-schema)
When an event is triggered, the Event Grid service sends data about that event to subscribing endpoint.
This section contains an example of what that data would look like for each event. Each event has the following top-level data:

|     Property          |     Type     |     Description                                                                                                                                |
|-----------------------|--------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|    `topic`              |    string    |    Full resource path to the event   source. This field isn't writeable. Event Grid provides this value.                                      |
|    `subject`            |    string    |    Publisher-defined path to the   event subject.                                                                                              |
|    `eventType`          |    string    |    One of the registered event   types for this event source.                                                                                  |
|    `eventTime`          |    string    |    The time the event is generated   based on the provider's UTC time.                                                                         |
|    `id`                 |    string    |    Unique identifier for the event.                                                                                                            |
|    `data`               |    object    |    Blob storage event data.                                                                                                                    |
|    `dataVersion`        |    string    |    The schema version of the data   object. The publisher defines the schema version.                                                          |
|    `metadataVersion`    |    string    |    The schema version of the event   metadata. Event Grid defines the schema of the top-level properties. Event   Grid provides this value.    |

# [Cloud event schema](#tab/cloud-event-schema)

When an event is triggered, the Event Grid service sends data about that event to subscribing endpoint.
This section contains an example of what that data would look like for each event. Each event has the following top-level data:

|     Property          |     Type     |     Description                                                                                                                                |
|-----------------------|--------------|------------------------------------------------------------------------------------------------------------------------------------------------|
|    `source`              |    string    |    Full resource path to the event   source. This field isn't writeable. Event Grid provides this value.                                      |
|    `subject`            |    string    |    Publisher-defined path to the   event subject.                                                                                              |
|    `type`          |    string    |    One of the registered event   types for this event source.                                                                                  |
|    `time`          |    string    |    The time the event is generated   based on the provider's UTC time.                                                                         |
|    `id`                 |    string    |    Unique identifier for the event.                                                                                                            |
|    `data`               |    object    |    Blob storage event data.                                                                                                                    |
| `specversion` | string | CloudEvents schema specification version. |

---

## Example events

### NewKubernetesVersionAvailable

# [Event Grid event schema](#tab/event-grid-event-schema)

```json
{
    "topic": "/subscriptions/<id>/resourceGroups<rg>/providers/Microsoft.ContainerService/managedClusters/<cluster>",
    "subject": "<cluster>",
    "eventType": "Microsoft.ContainerService.NewKubernetesVersionAvailable",
    "id": "1234567890abcdef1234567890abcdef12345678",
    "data": {
      "latestSupportedKubernetesVersion": "1.20.7",
      "latestStableKubernetesVersion": "1.19.11",
      "lowestMinorKubernetesVersion": "1.18.19",
      "latestPreviewKubernetesVersion": "1.21.1"
    },
    "dataVersion": "1",
    "metadataVersion": "1",
    "eventTime": "2021-07-01T04:52:57.0000000Z"
}
```
# [Cloud event schema](#tab/cloud-event-schema)

```json

{
    "source": "/subscriptions/<id>/resourceGroups<rg>/providers/Microsoft.ContainerService/managedClusters/<cluster>",
    "subject": "<cluster>",
    "type": "Microsoft.ContainerService.NewKubernetesVersionAvailable",
    "id": "1234567890abcdef1234567890abcdef12345678",
    "data": {
      "latestSupportedKubernetesVersion": "1.20.7",
      "latestStableKubernetesVersion": "1.19.11",
      "lowestMinorKubernetesVersion": "1.18.19",
      "latestPreviewKubernetesVersion": "1.21.1"
    },
    "specversion": "1.0",
    "time": "2021-07-01T04:52:57.0000000Z"
}
```

---

The data object contains the following properties:

|    Property                        | Type   | Description                                                  |
|------------------------------------|--------|--------------------------------------------------------------|
| `latestSupportedKubernetesVersion` | string | The latest supported version of Kubernetes available.        |
| `latestStableKubernetesVersion`    | string | The latest stable supported version of Kubernetes available. |
| `lowestMinorKubernetesVersion`     | string | The lowest supported version of Kubernetes available.        |
| `latestPreviewKubernetesVersion`   | string | The latest preview version of Kubernetes available.          |

### ClusterSupportEnded

# [Event Grid event schema](#tab/event-grid-event-schema)

```json
{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "eventType": "Microsoft.ContainerService.ClusterSupportEnded",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "kubernetesVersion": "1.23.15"
  },
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2023-03-29T18:00:00.0000000Z"
}
```
# [Cloud event schema](#tab/cloud-event-schema)

```json
{
  "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "type": "Microsoft.ContainerService.ClusterSupportEnded",
  "time": "2023-03-29T18:00:00.0000000Z",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "kubernetesVersion": "1.23.15"
  },
  "specversion": "1.0"
}
```

### ClusterSupportEnding

# [Event Grid event schema](#tab/event-grid-event-schema)

```json
{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "eventType": "Microsoft.ContainerService.ClusterSupportEnding",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "kubernetesVersion": "1.24.10"
  },
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2023-03-29T18:00:00.0000000Z"
}
```

# [Cloud event schema](#tab/cloud-event-schema)

```json
{
  "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "type": "Microsoft.ContainerService.ClusterSupportEnding",
  "time": "2023-03-29T18:00:00.0000000Z",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "kubernetesVersion": "1.24.10"
  },
  "specversion": "1.0"
}
```

### NodePoolRollingStarted

# [Event Grid event schema](#tab/event-grid-event-schema)

```json
{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "eventType": "Microsoft.ContainerService.NodePoolRollingStarted",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "nodePoolName": "nodepool1"
  },
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2023-03-29T18:00:00.0000000Z"
}
```

# [Cloud event schema](#tab/cloud-event-schema)

```json
{
  "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "type": "Microsoft.ContainerService.NodePoolRollingStarted",
  "time": "2023-03-29T18:00:00.0000000Z",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "nodePoolName": "nodepool1"
  },
  "specversion": "1.0"
}
```

### NodePoolRollingSucceeded

# [Event Grid event schema](#tab/event-grid-event-schema)

```json
{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "eventType": "Microsoft.ContainerService.NodePoolRollingSucceeded",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "nodePoolName": "nodepool1"
  },
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2023-03-29T18:00:00.0000000Z"
}
```

# [Cloud event schema](#tab/cloud-event-schema)

```json
{
  "source": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "type": "Microsoft.ContainerService.NodePoolRollingSucceeded",
  "time": "2023-03-29T18:00:00.0000000Z",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "nodePoolName": "nodepool1"
  },
  "specversion": "1.0"
}
```

### NodePoolRollingFailed

# [Event Grid event schema](#tab/event-grid-event-schema)

```json
{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "eventType": "Microsoft.ContainerService.NodePoolRollingFailed",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "nodePoolName": "nodepool1"
  },
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2023-03-29T18:00:00.0000000Z"
}
```

# [Cloud event schema](#tab/cloud-event-schema)

```json
{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.ContainerService/managedClusters/{cluster}",
  "subject": "{cluster}",
  "eventType": "Microsoft.ContainerService.NodePoolRollingFailed",
  "id": "1234567890abcdef1234567890abcdef12345678",
  "data": {
    "nodePoolName": "nodepool1"
  },
  "dataVersion": "1",
  "metadataVersion": "1",
  "eventTime": "2023-03-29T18:00:00.0000000Z"
}
```

## Next steps
See the following tutorial: [Quickstart: Subscribe to Azure Kubernetes Service (AKS) events with Azure Event Grid](../aks/quickstart-event-grid.md).
