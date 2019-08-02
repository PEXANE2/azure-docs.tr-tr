---
title: Azure Işlevleri hata işleme Kılavuzu | Microsoft Docs
description: İşlevlerinizin yürütülmesi sırasında oluşan hataları işlemeye yönelik genel rehberlik ve bağlamaya özgü hata konularına bağlantılar sağlar.
services: functions
cloud: ''
documentationcenter: ''
author: craigshoemaker
manager: gwallace
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: cshoe
ms.openlocfilehash: bc0ffec1bfe55ee81573feffc58e1adb11421eec
ms.sourcegitcommit: fecb6bae3f29633c222f0b2680475f8f7d7a8885
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68667440"
---
# <a name="azure-functions-error-handling"></a>Azure Işlevleri hata işleme

Bu konu, işlevleriniz yürütülürken oluşan hataları işlemeye yönelik genel rehberlik sağlar. Ayrıca, ortaya çıkabilecek bağlamaya özgü hataları tanımlayan konuların bağlantılarını sağlar. 

## <a name="handling-errors-in-functions"></a>İşlevlerde hataları işleme
[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

 
## <a name="binding-error-codes"></a>Bağlama hata kodları

Azure hizmetleriyle tümleştirilirken, temel hizmetlerin API 'Lerinden kaynaklanan hatalar ortaya çıkabilir. Bu hizmetler için hata kodu belgelerinin bağlantıları, aşağıdaki tetikleyici ve bağlama başvuru konularının **özel durumlar ve dönüş kodları** bölümünde bulunabilir:

+ [Azure Cosmos DB](functions-bindings-cosmosdb.md#exceptions-and-return-codes)

+ [Blob depolama](functions-bindings-storage-blob.md#exceptions-and-return-codes)

+ [Event Hubs](functions-bindings-event-hubs.md#exceptions-and-return-codes)

+ [Notification Hubs](functions-bindings-notification-hubs.md#exceptions-and-return-codes)

+ [Kuyruk depolama](functions-bindings-storage-queue.md#exceptions-and-return-codes)

+ [Service Bus](functions-bindings-service-bus.md#exceptions-and-return-codes)

+ [Tablo depolama](functions-bindings-storage-table.md#exceptions-and-return-codes)
