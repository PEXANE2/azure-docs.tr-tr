---
title: Azure Işlevleri hata işleme Kılavuzu
description: Azure Işlevlerinde belirli bağlama hatalarının bağlantılarıyla ilgili hataları işleme hakkında bilgi edinin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: cshoe
ms.openlocfilehash: befdb4a8cceaef18961c1e9297e23ed5d405ff50
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "77586198"
---
# <a name="azure-functions-error-handling"></a>Azure Işlevleri hata işleme

Azure Işlevlerinde hataları işleme, kayıp verilerin, unutulan olayların olmaması ve uygulamanızın durumunu izlemek için önemlidir.

Bu makalede, bağlama özgü hataların bağlantılarıyla birlikte hata işleme için genel stratejiler açıklanmaktadır.

## <a name="handling-errors"></a>Hataları işleme

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

## <a name="binding-error-codes"></a>Bağlama hata kodları

Azure hizmetleriyle tümleştirilirken, hatalar temeldeki hizmetlerin API 'Lerinden kaynaklanmayabilir. Bağlama özgü hatalarla ilgili bilgiler aşağıdaki makalelerin **özel durumlar ve dönüş kodları** bölümünde bulunabilir:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [BLOB depolama](functions-bindings-storage-blob-output.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs-output.md#exceptions-and-return-codes)

+ [IoT Hub 'Ları](functions-bindings-event-iot-output.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Kuyruk depolama](functions-bindings-storage-queue-output.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus-output.md#exceptions-and-return-codes)

+ [Tablo depolama](functions-bindings-storage-table.md#exceptions-and-return-codes)
