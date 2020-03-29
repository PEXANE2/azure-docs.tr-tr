---
title: Azure Fonksiyonları ile Azure Cosmos DB değişiklik akışı nasıl kullanılır?
description: Azure Cosmos DB değişiklik akışına bağlanmak için Azure Işlevleri'ni kullanın. Daha sonra her yeni olay tetiklenen reaktif Azure işlevleri oluşturabilirsiniz.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/03/2019
ms.reviewer: sngun
ms.openlocfilehash: 7a74635551d8416bf60689b1f1403f29883e81bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851375"
---
# <a name="serverless-event-based-architectures-with-azure-cosmos-db-and-azure-functions"></a>Azure Cosmos DB ve Azure Fonksiyonları ile sunucusuz olay tabanlı mimariler

Azure İşlevler, [değişiklik akışına](change-feed.md)bağlanmanın en basit yolunu sağlar. Azure Cosmos kapsayıcınızın değişiklik akışındaki her yeni etkinlikte otomatik olarak tetiklenecek küçük reaktif Azure Fonksiyonları oluşturabilirsiniz.

![Cosmos DB için Azure Fonksiyonları ile çalışan sunucusuz olay tabanlı fonksiyonlar tetikleyici](./media/change-feed-functions/functions.png)

[Cosmos DB için Azure Fonksiyonları tetikleyicisi](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md)ile, herhangi bir [çalışan altyapısını](./change-feed-processor.md)korumaya gerek kalmadan [Feed Processor'in](./change-feed-processor.md)ölçekleme ve güvenilir olay algılama işlevlerinden yararlanabilirsiniz. Etkinlik kaynak sağlayan boru hattının geri kalanı hakkında endişelenmeden Azure İşlevinizin mantığına odaklanmanız yeterli. Tetikleyiciyi diğer [Azure İşleme bağlamazlarıyla](../azure-functions/functions-triggers-bindings.md#supported-bindings)bile karıştırabilirsiniz.

> [!NOTE]
> Şu anda, Cosmos DB için Azure İşlevleri tetikleyiciyalnızca Core (SQL) API ile kullanılmak üzere desteklenir.

## <a name="requirements"></a>Gereksinimler

Sunucusuz olay tabanlı bir akış uygulamak için şunları yapmanız gerekir:

* **İzlenen kapsayıcı**: İzlenen kapsayıcı, izlenen Azure Cosmos kapsayıcısır ve değişik lik verisinin oluşturulduğu verileri depolar. İzlenen kapsayıcıdaki eklemeler, güncelleştirmeler kapsayıcının değişim akışına yansıtılır.
* **Kiralama kapsayıcısı**: Kira kapsayıcısı, birden çok ve dinamik sunucusuz Azure İşlevi örneklerinde durumu korur ve dinamik ölçekleme sağlar. Bu kiralama kapsayıcısı Cosmos DB için Azure Fonksiyonları tetikleyicisi tarafından el ile veya otomatik olarak oluşturulabilir. Kiralama kapsayıcısını otomatik olarak oluşturmak için, [yapılandırmada](../azure-functions/functions-bindings-cosmosdb-v2-trigger.md#configuration) *CreateLeaseCollectionIfNotExists* bayrağını ayarlayın. Bölümlenmiş kira kapsayıcılarının bölüm `/id` anahtarı tanımı na sahip olması gerekir.

## <a name="create-your-azure-functions-trigger-for-cosmos-db"></a>Cosmos DB için Azure İşlevlerinizi tetikleme

Cosmos DB için Azure İşlevleri tetikleyicisi ile Azure İşlevlerinizi oluşturma artık tüm Azure Fonksiyonları IDE ve CLI tümleştirmelerinde desteklenir:

* Visual Studio kullanıcıları için [Visual Studio Extension.](../azure-functions/functions-develop-vs.md)
* Visual Studio Code kullanıcıları için [Visual Studio Code Extension.](/azure/javascript/tutorial-vscode-serverless-node-01)
* Ve son olarak [core CLI bir](../azure-functions/functions-run-local.md#create-func) çapraz platform IDE agnostik deneyim için takım.

## <a name="run-your-trigger-locally"></a>Tetikleyicinizi yerel olarak çalıştırın

[Azure İşlevinizi](../azure-functions/functions-develop-local.md) Azure Aboneliği olmadan veya herhangi bir maliyete maruz kalmadan sunucusuz olay tabanlı akışlarınızı oluşturmak ve geliştirmek için Azure İşlevinizi [Azure Cosmos DB Emulatörü](./local-emulator.md) ile yerel olarak çalıştırabilirsiniz.

Bulutta canlı senaryoları test etmek istiyorsanız, herhangi bir kredi kartı veya Azure aboneliği gerektirmeden [Cosmos DB'yi ücretsiz olarak deneyebilirsiniz.](https://azure.microsoft.com/try/cosmosdb/)

## <a name="next-steps"></a>Sonraki adımlar

Artık aşağıdaki makalelerde değişiklik akışı hakkında daha fazla bilgi edinmeye devam edebilirsiniz:

* [Değişiklik akışına genel bakış](change-feed.md)
* [Değişiklik akışını okuma yolları](read-change-feed.md)
* [Değişiklik beslemeişlemci kitaplığını kullanma](change-feed-processor.md)
* [Değişiklik besleme işlemcikitaplığı ile çalışma](change-feed-processor.md)
* [Azure Cosmos DB ve Azure Fonksiyonlarını kullanarak sunucusuz veritabanı hesaplama](serverless-computing-database.md)
