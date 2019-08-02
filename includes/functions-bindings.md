---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 0134d20f4b5d2816fa38bfb7377154463acd4781
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68669858"
---
Bu tabloda, Azure Işlevleri çalışma zamanının iki ana sürümünde desteklenen bağlamalar gösterilmektedir:

| Type | 'in | 2. x<sup>1</sup> | Tetikleyici | Girdi | Output |
| ---- | :-: | :-: | :------: | :---: | :----: |
| [Blob depolama](../articles/azure-functions/functions-bindings-storage-blob.md)          |✔|✔|✔|✔|✔|
| [Cosmos DB](../articles/azure-functions/functions-bindings-documentdb.md)               |✔|✔|✔|✔|✔|
| [Event Grid](../articles/azure-functions/functions-bindings-event-grid.md)              |✔|✔|✔| | |
| [Event Hubs](../articles/azure-functions/functions-bindings-event-hubs.md)              |✔|✔|✔| |✔|
| [HTTP & Web kancaları](../articles/azure-functions/functions-bindings-http-webhook.md)             |✔|✔|✔| |✔|
| [Excel<br/>tablolarını Microsoft Graph](../articles/azure-functions/functions-bindings-microsoft-graph.md)   ||✔| |✔|✔|
| [OneDrive<br/>dosyalarını Microsoft Graph](../articles/azure-functions/functions-bindings-microsoft-graph.md) ||✔| |✔|✔|
| [Outlook<br/>e-postası Microsoft Graph](../articles/azure-functions/functions-bindings-microsoft-graph.md)  ||✔| | |✔|
| [Microsoft Graph<br/>olaylar](../articles/azure-functions/functions-bindings-microsoft-graph.md)         ||✔|✔|✔|✔|
| [Microsoft Graph<br/>kimlik doğrulaması belirteçleri](../articles/azure-functions/functions-bindings-microsoft-graph.md)    ||✔| |✔| |
| [Mobile Apps](../articles/azure-functions/functions-bindings-mobile-apps.md)             |✔| | |✔|✔|
| [Notification Hubs](../articles/azure-functions/functions-bindings-notification-hubs.md) |✔|| | |✔|
| [Kuyruk depolama](../articles/azure-functions/functions-bindings-storage-queue.md)         |✔|✔|✔| |✔|
| [SendGrid](../articles/azure-functions/functions-bindings-sendgrid.md)                   |✔|✔| | |✔|
| [Service Bus](../articles/azure-functions/functions-bindings-service-bus.md)             |✔|✔|✔| |✔|
| [SignalR](../articles/azure-functions/functions-bindings-signalr-service.md)             | |✔| |✔|✔|
| [Tablo depolama](../articles/azure-functions/functions-bindings-storage-table.md)         |✔|✔| |✔|✔|
| [Timer](../articles/azure-functions/functions-bindings-timer.md)                         |✔|✔|✔| | |
| [Twilio](../articles/azure-functions/functions-bindings-twilio.md)                       |✔|✔| | |✔|

<sup>1</sup> 2. x IÇINDE, http ve Zamanlayıcı hariç tüm bağlamaların kayıtlı olması gerekir. Bkz. [bağlama uzantılarını kaydetme](../articles/azure-functions/functions-bindings-register.md).
