---
title: Azure İşlevler hata işleme kılavuzu
description: Azure İşlevlerinde belirli bağlama hatalarına bağlantılariçeren hataları işlemeyi öğrenin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: cshoe
ms.openlocfilehash: befdb4a8cceaef18961c1e9297e23ed5d405ff50
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586198"
---
# <a name="azure-functions-error-handling"></a>Azure Fonksiyonları hata işleme

Azure İşlevlerinde hataları işlemek, kaybolan verileri, gözden kaçan olayları önlemek ve uygulamanızın durumunu izlemek için önemlidir.

Bu makalede, bağlamaya özgü hatalara bağlantılarla birlikte hata işleme için genel stratejiler açıklanmaktadır.

## <a name="handling-errors"></a>Hataları işleme

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

## <a name="binding-error-codes"></a>Hata kodlarını bağlama

Azure hizmetleriyle tümleştirme yaparken, hatalar temel hizmetlerin API'lerinden kaynaklanabilir. Bağlayıcıya özgü hatalarla ilgili bilgiler aşağıdaki makalelerin **İstisnalar ve iade kodları** bölümünde mevcuttur:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob depolama](functions-bindings-storage-blob-output.md#exceptions-and-return-codes)

+ [Etkinlik Hub'ları](functions-bindings-event-hubs-output.md#exceptions-and-return-codes)

+ [IoT Hub'ları](functions-bindings-event-iot-output.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Kuyruk depolama](functions-bindings-storage-queue-output.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus-output.md#exceptions-and-return-codes)

+ [Tablo depolama](functions-bindings-storage-table.md#exceptions-and-return-codes)
