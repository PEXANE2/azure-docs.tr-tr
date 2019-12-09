---
title: Azure Işlevleri için Azure IoT Hub bağlamaları
description: Azure Işlevlerinde IoT Hub bağlamalarının nasıl kullanılacağını anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 03/05/2019
ms.author: cshoe
ms.openlocfilehash: 2940f9e2adff82c100ed347431e8c1d27c30202e
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2019
ms.locfileid: "74924448"
---
# <a name="azure-iot-hub-bindings-for-azure-functions"></a>Azure Işlevleri için Azure IoT Hub bağlamaları

Bu makalede, IoT Hub için Azure Işlevleri bağlamalarıyla nasıl çalışılacağı açıklanmaktadır. IoT Hub desteği, [Azure Event Hubs bağlamasını](functions-bindings-event-hubs.md)temel alır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Azure Işlevleri sürüm 1. x için IoT Hub bağlamaları [Microsoft. Azure. WebJobs. ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet paketi, sürüm 2. x içinde verilmiştir. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x-and-higher"></a>Paketler-Işlevler 2. x ve üzeri

2\. x ve üzeri Işlevler için [Microsoft. Azure. WebJobs. Extensions. EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) paketini, sürüm 3. x ' i kullanın. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

> [!IMPORTANT]
> Aşağıdaki kod örnekleri Olay Hub 'ı API 'sini kullanırken, verilen söz dizimi IoT Hub işlevleri için geçerlidir.

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
