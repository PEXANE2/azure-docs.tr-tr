---
title: include dosyası
description: include dosyası
services: service-bus-messaging
author: clemensv
ms.service: service-bus-messaging
ms.topic: include
ms.date: 11/24/2020
ms.author: clemensv
ms.custom: include file
ms.openlocfilehash: fe205524c5aef6a43771cc09c810da217678d108
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96022158"
---
AMQP-Over-WebSockets Protocol seçeneği, HTTP/REST API gibi bağlantı noktası TCP 443 üzerinden çalışır, ancak Aksi halde düz AMQP ile aynı şekilde çalışır. Bu seçenek, ek el sıkışma ve HTTPS bağlantı noktasını paylaşmak için zorunluluğunu getirir kadar çok daha fazla ek yük nedeniyle ilk bağlantı gecikmesini daha yüksektir. Bu mod seçilirse, TCP bağlantı noktası 443 iletişim için yeterlidir. Aşağıdaki seçenekler, düz AMQP veya AMQP WebSockets modunun seçilmesine izin verir:

| Dil | Seçenek   |
| -------- | ----- |
| .NET     | [TransportType. AMQP](/dotnet/api/microsoft.azure.servicebus.transporttype?view=azure-dotnet) veya [TransportType. AmqpWebSockets](/dotnet/api/microsoft.azure.servicebus.transporttype?view=azure-dotnet) ile [servicebusconnection. TransportType](/dotnet/api/microsoft.azure.servicebus.servicebusconnection.transporttype?view=azure-dotnet) özelliği |
| Java     | com. Microsoft. [Azure. ServiceBus. ClientSettings](/java/api/com.microsoft.azure.servicebus.clientsettings.clientsettings?view=azure-java-stable) [com. Microsoft. Azure. ServiceBus. Ilkel. TRANSPORTTYPE. amqp](/java/api/com.microsoft.azure.servicebus.primitives.transporttype?view=azure-java-stable) veya [com.Microsoft.Azure.ServiceBus.Primitives.TransportType.AMQP_WEB_SOCKETS](/java/api/com.microsoft.azure.servicebus.primitives.transporttype?view=azure-java-stable) |
| Node  | [Servicebusclientoptions](/javascript/api/@azure/service-bus/servicebusclientoptions?view=azure-node-latest) bir `webSocket` Oluşturucu bağımsız değişkenine sahip. |
| Python | [TransportType. AMQP](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) veya [TransportType. AmqpOverWebSocket](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) ile [ServiceBusClient.transport_type](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.ServiceBusClient) |