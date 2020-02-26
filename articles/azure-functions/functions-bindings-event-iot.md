---
title: Azure Işlevleri için Azure IoT Hub bağlamaları
description: Azure Işlevlerinde IoT Hub tetikleyiciyi ve bağlamayı kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 1c25543b16c3486a8f6a445427346382faaaa09a
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77586147"
---
# <a name="azure-iot-hub-bindings-for-azure-functions"></a>Azure Işlevleri için Azure IoT Hub bağlamaları

Bu makale kümesinde IoT Hub için Azure Işlevleri bağlamalarıyla nasıl çalışılacağı açıklanmaktadır. IoT Hub desteği, [Azure Event Hubs bağlamasını](functions-bindings-event-hubs.md)temel alır.

> [!IMPORTANT]
> Aşağıdaki kod örnekleri Olay Hub 'ı API 'sini kullanırken, verilen söz dizimi IoT Hub işlevleri için geçerlidir.

| Eylem | Tür |
|--------|------|
| IoT Hub olay akışına gönderilen olaylara yanıt verme. | [Tetikleyici](./functions-bindings-event-iot-trigger.md) |
| IoT olay akışına olay yazma | [Çıkış bağlama](./functions-bindings-event-iot-output.md) |

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Bir olay hub 'ı olay akışına (tetikleyici) gönderilen olaylara yanıt verme](./functions-bindings-event-iot-trigger.md)
- [Olayları bir olay akışına yazma (çıkış bağlama)](./functions-bindings-event-iot-output.md)
