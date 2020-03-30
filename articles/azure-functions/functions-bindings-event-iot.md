---
title: Azure İşlevleri için Azure IoT Hub bağlamaları
description: Azure İşlevlerinde IoT Hub tetikleyicisini ve bağlamayı kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 1c25543b16c3486a8f6a445427346382faaaa09a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586147"
---
# <a name="azure-iot-hub-bindings-for-azure-functions"></a>Azure İşlevleri için Azure IoT Hub bağlamaları

Bu makale kümesi, IoT Hub için Azure İşlevbağlamaları ile nasıl çalışılabildiğini açıklar. IoT Hub desteği, [Azure Etkinlik Hub'ları Bağlama'yı](functions-bindings-event-hubs.md)temel adatır.

> [!IMPORTANT]
> Aşağıdaki kod örnekleri Olay Hub API'sini kullanırken, verilen sözdizimi IoT Hub işlevleri için geçerlidir.

| Eylem | Tür |
|--------|------|
| IoT hub olay akışına gönderilen olaylara yanıt verin. | [Tetikleyici](./functions-bindings-event-iot-trigger.md) |
| Olayları Bir IoT olay akışına yazma | [Çıkış bağlama](./functions-bindings-event-iot-output.md) |

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Olay merkezi olay akışına gönderilen olaylara yanıt verme (Tetikleyici)](./functions-bindings-event-iot-trigger.md)
- [Olayları olay akışına yazma (Çıktı bağlama)](./functions-bindings-event-iot-output.md)
