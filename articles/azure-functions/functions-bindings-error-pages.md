---
title: Azure Işlevleri hata işleme Kılavuzu
description: Azure Işlevlerinde belirli bağlama hatalarının bağlantılarıyla ilgili hataları işleme hakkında bilgi edinin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: cshoe
ms.openlocfilehash: 06dc4c5c1713ee10f263e573a698e9ea36ca2662
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74227333"
---
# <a name="azure-functions-error-handling"></a>Azure Işlevleri hata işleme

Azure Işlevlerinde hataları işleme, kayıp verilerin, unutulan olayların olmaması ve uygulamanızın durumunu izlemek için önemlidir.

Bu makalede, bağlama özgü hataların bağlantılarıyla birlikte hata işleme için genel stratejiler açıklanmaktadır.

## <a name="handling-errors"></a>Hataları işleme

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

## <a name="binding-error-codes"></a>Bağlama hata kodları

Azure hizmetleriyle tümleştirilirken, hatalar temeldeki hizmetlerin API 'Lerinden kaynaklanmayabilir. Bağlama özgü hatalarla ilgili bilgiler aşağıdaki makalelerin **özel durumlar ve dönüş kodları** bölümünde bulunabilir:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob depolama](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Kuyruk depolama](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Tablo depolama](functions-bindings-storage-table.md#exceptions-and-return-codes)
