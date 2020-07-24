---
title: Azure Işlevleri için Azure IoT Hub tetikleyicisi
description: Azure Işlevlerinde bir IoT Hub olay akışına gönderilen olaylara yanıt vermeyi öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 25396da3fb9a4293633308bf2e9d3c6b3d07265d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87041643"
---
# <a name="azure-iot-hub-trigger-for-azure-functions"></a>Azure Işlevleri için Azure IoT Hub tetikleyicisi

Bu makalede, IoT Hub için Azure Işlevleri bağlamalarıyla nasıl çalışılacağı açıklanmaktadır. IoT Hub desteği, [Azure Event Hubs bağlamasını](functions-bindings-event-hubs.md)temel alır.

Kurulum ve yapılandırma ayrıntıları hakkında bilgi için bkz. [genel bakış](functions-bindings-event-iot.md).

> [!IMPORTANT]
> Aşağıdaki kod örnekleri Olay Hub 'ı API 'sini kullanırken, verilen söz dizimi IoT Hub işlevleri için geçerlidir.

[!INCLUDE [functions-bindings-event-hubs](../../includes/functions-bindings-event-hubs-trigger.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Olayları bir olay akışına yazma (çıkış bağlama)](./functions-bindings-event-iot-output.md)
