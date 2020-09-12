---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.custom: fasttrack-edit
ms.openlocfilehash: 2c3eb2423c308d512bb880525cb15fef2ffbdb2c
ms.sourcegitcommit: c94a177b11a850ab30f406edb233de6923ca742a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/01/2020
ms.locfileid: "89303931"
---
Bu tabloda, Azure Işlevleri çalışma zamanının ana sürümlerinde desteklenen bağlamalar gösterilmektedir:


| Tür | 'in | 2. x ve üzeri<sup>1</sup> | Tetikleyici | Girdi | Çıktı |
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob depolama](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|
| [Cosmos DB](../articles/azure-functions/functions-bindings-documentdb.md)               |✔|✔|✔|✔|✔|
| [Davpr](https://github.com/dapr/azure-functions-extension)<sup>3</sup>             | |✔|✔|✔|✔|
| [Event Grid](../articles/azure-functions/functions-bindings-event-grid.md)              |✔|✔|✔| |✔|
| [Event Hubs](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|
| [HTTP & Web kancaları](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
| [IoT Hub’ı](../articles/azure-functions/functions-bindings-event-iot.md)             |✔|✔|✔| |✔|
| [Kafka](https://github.com/azure/azure-functions-kafka-extension)<sup>2</sup>             | |✔|✔| |✔|
| [Mobile Apps](../articles/azure-functions/functions-bindings-mobile-apps.md)             |✔| | |✔|✔|
| [Notification Hubs](../articles/azure-functions/functions-bindings-notification-hubs.md) |✔|| | |✔|
| [Kuyruk depolama](../articles/azure-functions/functions-bindings-storage-queue.md)         |✔|✔|✔| |✔|
| [Çbbitmq](https://github.com/azure/azure-functions-rabbitmq-extension)<sup>2</sup>             | |✔|✔| |✔|
| [SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md)                   |✔|✔| | |✔|
| [Service Bus](../articles/azure-functions/functions-bindings-service-bus.md)             |✔|✔|✔| |✔|
| [SignalR](../articles/azure-functions/functions-bindings-signalr-service.md)             | |✔| |✔|✔|
| [Tablo depolama](../articles/azure-functions/functions-bindings-storage-table.md)         |✔|✔| |✔|✔|
| [Zamanlayıcı](../articles/azure-functions/functions-bindings-timer.md)                         |✔|✔|✔| | |
| [Twilio](../articles/azure-functions/functions-bindings-twilio.md)                       |✔|✔| | |✔|

<sup>1</sup> sürüm 2. x çalışma zamanı ile başlayarak, http ve Zamanlayıcı hariç tüm bağlamaların kayıtlı olması gerekir. Bkz. [bağlama uzantılarını kaydetme](../articles/azure-functions/functions-bindings-register.md).

<sup>2</sup> tetikleyici tüketim planında desteklenmez. [Çalışma zamanına dayalı Tetikleyiciler](../articles/azure-functions/functions-networking-options.md#premium-plan-with-virtual-network-triggers)gerektirir.

<sup>3</sup> yalnızca Kubernetes, IoT Edge ve yalnızca diğer şirket içinde barındırılan modlarda desteklenir.
