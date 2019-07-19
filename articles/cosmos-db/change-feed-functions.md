---
title: Azure Işlevleriyle Azure Cosmos DB değişiklik akışını kullanma
description: Azure Işlevleriyle Azure Cosmos DB değişiklik akışını kullanın
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: db3f1b6657ae455ae049eaffd6758fc7e6944fb9
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68001019"
---
# <a name="serverless-event-based-architectures-with-azure-cosmos-db-and-azure-functions"></a>Azure Cosmos DB ve Azure Işlevleri ile sunucusuz olay tabanlı mimariler

Azure Işlevleri, [değişiklik akışına](change-feed.md)bağlanmak için en kolay yolu sağlar. Azure Cosmos kapsayıcısının değişiklik akışındaki her yeni olayda otomatik olarak tetiklenecek küçük reaktif Azure Işlevleri oluşturabilirsiniz.

![Azure Cosmos DB tetikleyicisiyle çalışan sunucusuz olay tabanlı Işlevler](./media/change-feed-functions/functions.png)

[Azure Cosmos DB tetikleyicisiyle](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger), herhangi bir [çalışan altyapısını](./change-feed-processor.md)sürdürme gerekmeden [değişiklik akışı işlemcisinin](./change-feed-processor.md)ölçeklendirilmesine ve güvenilir olay algılama işlevlerinden yararlanabilirsiniz. Olay kaynağını belirleme işlem hattının geri kalanı hakkında endişelenmeden Azure Işlevinizin mantığına odaklanmanız yeterlidir. Tetikleyiciyi diğer [Azure işlevleri bağlamalarıyla](../azure-functions/functions-triggers-bindings.md#supported-bindings)de karıştırabilirsiniz.

> [!NOTE]
> Şu anda Azure Cosmos DB tetikleyicisi yalnızca çekirdek (SQL) API 'SI ile kullanım için desteklenir.

## <a name="requirements"></a>Gereksinimler

Sunucusuz bir olay tabanlı akış uygulamak için şunlar gerekir:

* **İzlenen kapsayıcı**: İzlenen kapsayıcı, izlenmekte olan Azure Cosmos kapsayıcısıdır ve değişiklik beslemenin oluşturulduğu verileri depolar. İzlenen kapsayıcıya yapılan tüm ekler ve değişiklikler (örn. CRUD) kapsayıcının değişiklik akışında yansıtılır.
* **Kira kapsayıcısı**: Kira kapsayıcısı, birden çok ve dinamik sunucusuz Azure Işlev örneklerinde durum tutar ve dinamik ölçeklendirmeyi sağlar. Bu kira kapsayıcısı el ile veya otomatik olarak oluşturulabilir Azure Cosmos DB Trigger.To otomatik olarak kira kapsayıcısını oluşturabilir, [yapılandırmada](../azure-functions/functions-bindings-cosmosdb-v2.md#trigger---configuration) *Createleasecollectionifnotexists* bayrağını ayarlayabilir. Bölümlenmiş kira kapsayıcıları, `/id` bölüm anahtarı tanımına sahip olmak için gereklidir.

## <a name="create-your-azure-cosmos-db-trigger"></a>Azure Cosmos DB tetikleyiciyi oluşturma

Azure işlevinizi bir Azure Cosmos DB tetikleyicisiyle oluşturmak artık tüm Azure Işlevleri IDE ve CLı tümleştirmeleri arasında desteklenmektedir:

* Visual Studio kullanıcıları için [Visual Studio uzantısı](../azure-functions/functions-develop-vs.md) .
* Visual Studio Code kullanıcıları için [Visual Studio Core uzantısı](https://code.visualstudio.com/tutorials/functions-extension/create-function) .
* Ve son olarak, platformlar arası IDE deneyimi için [temel CLI araçları](../azure-functions/functions-run-local.md#create-func) .

## <a name="run-your-azure-cosmos-db-trigger-locally"></a>Azure Cosmos DB tetikleyiciyi yerel olarak çalıştırın

Azure bir abonelik olmadan sunucusuz olay tabanlı akışlarınızı oluşturup geliştirmek için Azure Işlevinizi [Azure Cosmos DB öykünücüsü](./local-emulator.md) ile [yerel olarak](../azure-functions/functions-develop-local.md) çalıştırabilirsiniz.

Bulutta canlı senaryoları test etmek isterseniz, kredi kartı veya Azure aboneliği gerekmeden [Cosmos DB ücretsiz olarak deneyebilirsiniz](https://azure.microsoft.com/try/cosmosdb/) .

## <a name="next-steps"></a>Sonraki adımlar

Şimdi aşağıdaki makalelerde değişiklik akışı hakkında daha fazla bilgi edinebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Değişiklik akışını okuma yolları](read-change-feed.md)
* [Kullanarak değişiklik akışı işlemci kitaplığı](change-feed-processor.md)
* [Değişiklik akışı işlemci kitaplığıyla çalışma](change-feed-processor.md)
* [Azure Cosmos DB ve Azure Işlevleri 'ni kullanarak sunucusuz veritabanı hesaplama](serverless-computing-database.md)
