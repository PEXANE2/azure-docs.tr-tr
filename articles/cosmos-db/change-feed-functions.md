---
title: Azure Işlevleriyle Azure Cosmos DB değişiklik akışını kullanma
description: Azure Işlevleriyle Azure Cosmos DB değişiklik akışını kullanın
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 299799566f0d54cc3fa017b032e15b387fe8cdd5
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467918"
---
# <a name="serverless-event-based-architectures-with-azure-cosmos-db-and-azure-functions"></a>Azure Cosmos DB ve Azure Işlevleri ile sunucusuz olay tabanlı mimariler

Azure Işlevleri, [değişiklik akışına](change-feed.md)bağlanmak için en kolay yolu sağlar. Azure Cosmos kapsayıcısının değişiklik akışındaki her yeni olayda otomatik olarak tetiklenecek küçük reaktif Azure Işlevleri oluşturabilirsiniz.

![Cosmos DB için Azure Işlevleri tetikleyicisiyle çalışan sunucusuz olay tabanlı Işlevler](./media/change-feed-functions/functions.png)

[Cosmos DB Için Azure işlevleri tetiklemesi](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger)sayesinde, herhangi bir [çalışan altyapısını](./change-feed-processor.md)sürdürmenize gerek kalmadan [değişiklik akışı işlemcisinin](./change-feed-processor.md)ölçeklendirilmesine ve güvenilir olay algılama işlevlerinden yararlanabilirsiniz. Olay kaynağını belirleme işlem hattının geri kalanı hakkında endişelenmeden Azure Işlevinizin mantığına odaklanmanız yeterlidir. Tetikleyiciyi diğer [Azure işlevleri bağlamalarıyla](../azure-functions/functions-triggers-bindings.md#supported-bindings)de karıştırabilirsiniz.

> [!NOTE]
> Şu anda, Cosmos DB için Azure Işlevleri yalnızca çekirdek (SQL) API 'SI ile kullanım için desteklenir.

## <a name="requirements"></a>Gereksinimler

Sunucusuz bir olay tabanlı akış uygulamak için şunlar gerekir:

* **İzlenen kapsayıcı**: İzlenen kapsayıcı, izlenmekte olan Azure Cosmos kapsayıcısıdır ve değişiklik beslemenin oluşturulduğu verileri depolar. İzlenen kapsayıcıya yapılan tüm ekler ve değişiklikler (örn. CRUD) kapsayıcının değişiklik akışında yansıtılır.
* **Kira kapsayıcısı**: Kira kapsayıcısı, birden çok ve dinamik sunucusuz Azure Işlev örneklerinde durum tutar ve dinamik ölçeklendirmeyi sağlar. Bu Kiralama kapsayıcısı, Cosmos DB için Azure Işlevleri tetikleyicisi tarafından el ile veya otomatik olarak oluşturulabilir. Kira kapsayıcısını otomatik olarak oluşturmak için, [yapılandırmada](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration) *Createleasecollectionifnotexists* bayrağını ayarlayın. Bölümlenmiş kira kapsayıcıları, `/id` bölüm anahtarı tanımına sahip olmak için gereklidir.

## <a name="create-your-azure-functions-trigger-for-cosmos-db"></a>Cosmos DB için Azure Işlevleri tetiklerinizi oluşturma

Azure işlevinizi Cosmos DB için Azure Işlevleri tetikleyicisi ile oluşturmak artık tüm Azure Işlevleri IDE ve CLı tümleştirmeleri arasında desteklenmektedir:

* Visual Studio kullanıcıları için [Visual Studio uzantısı](../azure-functions/functions-develop-vs.md) .
* Visual Studio Code kullanıcıları için [Visual Studio Core uzantısı](https://code.visualstudio.com/tutorials/functions-extension/create-function) .
* Ve son olarak, platformlar arası IDE deneyimi için [temel CLI araçları](../azure-functions/functions-run-local.md#create-func) .

## <a name="run-your-trigger-locally"></a>Tetikleyiciyi yerel olarak çalıştırın

Azure bir abonelik olmadan sunucusuz olay tabanlı akışlarınızı oluşturup geliştirmek için Azure Işlevinizi [Azure Cosmos DB öykünücüsü](./local-emulator.md) ile [yerel olarak](../azure-functions/functions-develop-local.md) çalıştırabilirsiniz.

Bulutta canlı senaryoları test etmek isterseniz, kredi kartı veya Azure aboneliği gerekmeden [Cosmos DB ücretsiz olarak deneyebilirsiniz](https://azure.microsoft.com/try/cosmosdb/) .

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde değişiklik akışı hakkında daha fazla bilgi edinebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Değişiklik akışını okuma yolları](read-change-feed.md)
* [Kullanarak değişiklik akışı işlemci kitaplığı](change-feed-processor.md)
* [Değişiklik akışı işlemci kitaplığıyla çalışma](change-feed-processor.md)
* [Azure Cosmos DB ve Azure Işlevleri 'ni kullanarak sunucusuz veritabanı hesaplama](serverless-computing-database.md)
