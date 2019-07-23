---
title: Azure Cosmos DB için Node. js örnekleri
description: CRUD işlemleri gibi Azure Cosmos DB'deki yaygın görevler için Github'da Node.js örnekleri bulabilirsiniz.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 07/23/2019
ms.author: dech
ms.openlocfilehash: 640f6a12f3df1bb8ded69c66eda3b307e6bdb74c
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "68384865"
---
# <a name="azure-cosmos-db-nodejs-examples"></a>Azure Cosmos DB Node.js örnekleri
> [!div class="op_single_selector"]
> * [.NET Örnekleri](sql-api-dotnet-samples.md)
> * [Java Örnekleri](sql-api-java-samples.md)
> * [Async Java Örnekleri](sql-api-async-java-samples.md)
> * [Node.js Örnekleri](sql-api-nodejs-samples.md)
> * [Python Örnekleri](sql-api-python-samples.md)
> * [Azure Kod Örneği Galerisi](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

Azure Cosmos DB kaynaklarında CRUD işlemlerini ve diğer yaygın işlemleri gerçekleştiren örnek çözümler, [azure-cosmos-js](https://github.com/Azure/azure-cosmos-js/tree/master/samples) GitHub deposuna eklenmiştir. Bu makalede aşağıdakiler sunulmaktadır:

* Node.js örnek proje dosyalarının her birindeki görevlere bağlantılar.
* İlgili API başvurusu içeriğine bağlantılar.

**Önkoşullar**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [Visual Studio abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, ücretli Azure hizmetleri için kullanabileceğiniz her ay kredileri sağlar.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[JavaScript SDK'sına](sql-api-sdk-node.md) da ihtiyacınız vardır.
   
   > [!NOTE]
   > Örnekler birbirinden bağımsızdır, kendi kendine ayarlanır ve sonra kendini temizler. Böyle olunca, örneklerde birden çok [Containers.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest) çağrısı yapılır. Bu her yapıldığında, oluşturulan kapsayıcının performans katmanı başına aboneliğiniz 1 saatlik kullanım için faturalandırılır.
   > 
   > 

## <a name="database-examples"></a>Veritabanı örnekleri
[DatabaseManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/DatabaseManagement) projesinin [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos veritabanları hakkında bilgi edinmek için bkz. [veritabanları, kapsayıcılar ve öğelerle çalışma](databases-containers-items.md) kavramsal makalesi. 

| Görev | API başvurusu |
| --- | --- |
| [Yoksa bir veritabanı oluşturma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L35-L37) |[Databases.createIfNotExists](https://docs.microsoft.com/javascript/api/%40azure/cosmos/databases?view=azure-node-latest) |
| [Hesabın veritabanlarını listeleme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L40-L42) |[Databases.readAll](https://docs.microsoft.com/javascript/api/%40azure/cosmos/databases?view=azure-node-latest) |
| [Kimliğe göre veritabanını okuma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L40-L42) |[Database.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/database?view=azure-node-latest) |
| [Veritabanı silme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/DatabaseManagement/app.js#L57-L60) |[Database.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/database?view=azure-node-latest) |

## <a name="container-examples"></a>Kapsayıcı örnekleri
[ContainerManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ItemManagement) projesinin [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos koleksiyonları hakkında bilgi edinmek için bkz. [veritabanları, kapsayıcılar ve öğelerle çalışma](databases-containers-items.md) kavramsal makalesi. 

| Görev | API başvurusu |
| --- | --- |
| [Yoksa bir kapsayıcı oluşturma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L36-L37) |[Containers.createIfNotExists](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest) |
| [Bir hesaptaki kapsayıcıları listeleme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L36-L37) |[Containers.readAll](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest) |
| [Kimliğe göre koleksiyon okuma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L47-L51) |[Container.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/container?view=azure-node-latest) |
| [Kapsayıcı silme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ContainerManagement/app.js#L54-L55) |[Container.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/container?view=azure-node-latest) |

## <a name="item-examples"></a>Öğe örnekleri
[ItemManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ItemManagement) projesinin [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos belgeleri hakkında bilgi edinmek için bkz. [veritabanları, kapsayıcılar ve öğelerle çalışma](databases-containers-items.md) kavramsal makalesi. 

| Görev | API başvurusu |
| --- | --- |
| [Öğe oluşturma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L49-L56) |[Items.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest) |
| [Bir kapsayıcıdaki tüm öğeleri okuma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L59-L64) |[Items.readAll](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest) |
| [Kimliğe göre öğe okuma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L59-L64) |[Item.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest) |
| [Yalnızca değişen öğeleri okuma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L73-L94) |[Item.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#accesscondition) |
| [Belgeler için sorgu](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L97-L118) |[Items.query](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest) |
| [Bir öğeyi değiştirme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L131-L136) |[Item.replace](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest) |
| [Koşullu ETag denetimiyle öğeyi değiştirme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L139-L160) |[Item.replace](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#accesscondition) |
| [Öğeyi silme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ItemManagement/app.js#L162-L164) |[Item.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest) |

## <a name="indexing-examples"></a>Dizin oluşturma örnekleri
[IndexManagement](https://github.com/Azure/azure-cosmos-js/tree/master/samples/IndexManagement) projesinin [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir.  Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB Dizin oluşturma hakkında bilgi edinmek için bkz. [Dizin oluşturma ilkeleri](index-policy.md), [Dizin oluşturma türleri](index-types.md)ve [Dizin oluşturma yolları](index-paths.md) kavramsal makaleler. 

| Görev | API başvurusu |
| --- | --- |
| [Belirli bir belgeyi el ile dizine alma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L135-L177) |[RequestOptions.indexingDirective: 'include'](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#indexingdirective) |
| [Belirli bir belgeyi el ile dizinin dışında bırakma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L90-L131) |[RequestOptions.indexingDirective: 'exclude'](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#indexingdirective) |
| [Toplu içeri aktarma için gecikmeli dizine alma kullanma veya büyük kapsayıcıları okuma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L183-L214) |[IndexingMode.Lazy](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingmode?view=azure-node-latest) |
| [Yolu dizinin dışında tutma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L352-L429) |[IndexingPolicy.ExcludedPath](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingpolicy?view=azure-node-latest#excludedpaths) |
| [Aralık işlemi sırasında bir dize yolunda taramaya izin verme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L219-L275) |[FeedOptions.EnableScanInQuery](https://docs.microsoft.com/javascript/api/%40azure/cosmos/feedoptions?view=azure-node-latest#enablescaninquery) |
| [Dize yolunda bir aralık dizini oluşturma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L281-L346) |[IndexKind.Range](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexkind?view=azure-node-latest), [IndexingPolicy](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingpolicy?view=azure-node-latest), [Items.query](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest) |
| [Varsayılan indexPolicy ile kapsayıcı oluşturma, ardından bunu çevrimiçi güncelleştirme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/IndexManagement/app.js#L435-L507) |[Containers.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest)

## <a name="server-side-programming-examples"></a>Sunucu tarafı programlama örnekleri
[ServerSideScripts](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ServerSideScripts) projesinin [app.js](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ServerSideScripts/app.js) dosyasında aşağıdaki görevlerin nasıl gerçekleştirileceği gösterilir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB içindeki sunucu tarafı programlama hakkında bilgi edinmek için bkz. [saklı yordamlar, Tetikleyiciler ve Kullanıcı tanımlı işlevler](stored-procedures-triggers-udfs.md) kavramsal makalesi. 

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ServerSideScripts/JS/upsert.js#L12-L72) |[StoredProcedures.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/storedprocedures?view=azure-node-latest) |
| [Saklı yordamı yürütme](https://github.com/Azure/azure-cosmos-js/blob/216672a679ab389e5b341280eeacab1cab3691e4/samples/ServerSideScripts/app.js#L44) |[StoredProcedure.execute](https://docs.microsoft.com/javascript/api/%40azure/cosmos/storedprocedure?view=azure-node-latest) |

Sunucu tarafı programlama hakkında daha fazla bilgi için bkz [. sunucu tarafında programlama Azure Cosmos DB. Saklı yordamlar, veritabanı Tetikleyicileri ve UDF](stored-procedures-triggers-udfs.md)'ler.

