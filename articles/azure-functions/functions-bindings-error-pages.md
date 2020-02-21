---
title: Azure Işlevleri hata işleme Kılavuzu
description: Azure Işlevlerinde belirli bağlama hatalarının bağlantılarıyla ilgili hataları işleme hakkında bilgi edinin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: cshoe
ms.openlocfilehash: 0da8322a57d925608f7b52054c7a52e6cb3e5d06
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77484664"
---
# <a name="azure-functions-error-handling"></a>Azure Işlevleri hata işleme

Azure Işlevlerinde hataları işleme, kayıp verilerin, unutulan olayların olmaması ve uygulamanızın durumunu izlemek için önemlidir.

Bu makalede, bağlama özgü hataların bağlantılarıyla birlikte hata işleme için genel stratejiler açıklanmaktadır.

## <a name="handling-errors"></a>Hataları işleme

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

## <a name="binding-error-codes"></a>Bağlama hata kodları

Azure hizmetleriyle tümleştirilirken, hatalar temeldeki hizmetlerin API 'Lerinden kaynaklanmayabilir. Bağlama özgü hatalarla ilgili bilgiler aşağıdaki makalelerin **özel durumlar ve dönüş kodları** bölümünde bulunabilir:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob depolama](functions-bindings-storage-blob-output.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Kuyruk depolama](functions-bindings-storage-queue-output.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus-output.md#exceptions-and-return-codes)

+ [Tablo depolama](functions-bindings-storage-table.md#exceptions-and-return-codes)
