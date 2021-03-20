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
ms.openlocfilehash: ca483d0b71bde945a7e46da785dd6a76b3a8f177
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98693415"
---
AMQP-Over-WebSockets Protocol seçeneği, HTTP/REST API gibi bağlantı noktası TCP 443 üzerinden çalışır, ancak Aksi halde düz AMQP ile aynı şekilde çalışır. Bu seçenek, ek el sıkışma ve HTTPS bağlantı noktasını paylaşmak için zorunluluğunu getirir kadar çok daha fazla ek yük nedeniyle ilk bağlantı gecikmesini daha yüksektir. Bu mod seçilirse, TCP bağlantı noktası 443 iletişim için yeterlidir. Aşağıdaki seçenekler, düz AMQP veya AMQP WebSockets modunun seçilmesine izin verir:

| Dil | Seçenek   |
| -------- | ----- |
| .NET     | [TransportType. AMQP](/dotnet/api/microsoft.azure.servicebus.transporttype) veya [TransportType. AmqpWebSockets](/dotnet/api/microsoft.azure.servicebus.transporttype) ile [servicebusconnection. TransportType](/dotnet/api/microsoft.azure.servicebus.servicebusconnection.transporttype) özelliği |
| Java     | com. Microsoft. [Azure. ServiceBus. ClientSettings](/java/api/com.microsoft.azure.servicebus.clientsettings.clientsettings) [com. Microsoft. Azure. ServiceBus. Ilkel. TRANSPORTTYPE. amqp](/java/api/com.microsoft.azure.servicebus.primitives.transporttype) veya [com.Microsoft.Azure.ServiceBus.Primitives.TransportType.AMQP_WEB_SOCKETS](/java/api/com.microsoft.azure.servicebus.primitives.transporttype) |
| Düğüm  | [Servicebusclientoptions](/javascript/api/@azure/service-bus/servicebusclientoptions) bir `webSocket` Oluşturucu bağımsız değişkenine sahip. |
| Python | [TransportType. AMQP](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) veya [TransportType. AmqpOverWebSocket](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.TransportType) ile [ServiceBusClient.transport_type](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-servicebus/latest/azure.servicebus.html#azure.servicebus.ServiceBusClient) |