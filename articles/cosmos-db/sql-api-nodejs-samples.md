---
title: Azure Cosmos veritabanındaki verileri yönetmek için düğüm.js örnekleri
description: CRUD işlemleri gibi Azure Cosmos DB'deki yaygın görevler için Github'da Node.js örnekleri bulabilirsiniz.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 08/23/2019
ms.author: dech
ms.openlocfilehash: 4ac3d74cb76a936cc88261a2fedf16764e8ae52f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76718302"
---
# <a name="nodejs-examples-to-manage-data-in-azure-cosmos-db"></a>Azure Cosmos DB'deki verileri yönetmek için düğüm.js örnekleri

> [!div class="op_single_selector"]
> * [.NET V2 SDK Örnekleri](sql-api-dotnet-samples.md)
> * [.NET V3 SDK Örnekleri](sql-api-dotnet-v3sdk-samples.md)
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

**Ön koşullar**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [Visual Studio abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, her ay size ücretli Azure hizmetleri için kullanabileceğiniz krediler verir.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[JavaScript SDK'sına](sql-api-sdk-node.md) da ihtiyacınız vardır.
   
   > [!NOTE]
   > Örnekler birbirinden bağımsızdır, kendi kendine ayarlanır ve sonra kendini temizler. Böyle olunca, örneklerde birden çok [Containers.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest) çağrısı yapılır. Bu her yapıldığında, oluşturulan kapsayıcının performans katmanı başına aboneliğiniz 1 saatlik kullanım için faturalandırılır.
   > 
   > 

## <a name="database-examples"></a>Veritabanı örnekleri

[DatabaseManagement](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts) dosyası, veritabanındaki CRUD işlemlerinin nasıl gerçekleştirilini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos veritabanları hakkında bilgi edinmek için [bkz.](databases-containers-items.md) 

| Görev | API başvurusu |
| --- | --- |
| [Yoksa bir veritabanı oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts#L12-L14) |[Databases.createIfNotExists](/javascript/api/@azure/cosmos/databases?view=azure-node-latest#createifnotexists-databaserequest--requestoptions-) |
| [Hesabın veritabanlarını listeleme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts#L16-L18) |[Databases.readAll](/javascript/api/@azure/cosmos/databases?view=azure-node-latest#readall-feedoptions-) |
| [Veritabanını kimlikle okuma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts#L20-L29) |[Database.read](/javascript/api/@azure/cosmos/database?view=azure-node-latest#read-requestoptions-) |
| [Veritabanı silme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts#L31-L32) |[Database.delete](/javascript/api/@azure/cosmos/database?view=azure-node-latest#delete-requestoptions-) |

## <a name="container-examples"></a>Kapsayıcı örnekleri

[ContainerManagement](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts) dosyası, konteynerdeki CRUD işlemlerinin nasıl gerçekleştirilini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos koleksiyonları hakkında bilgi edinmek için [veritabanları, kapsayıcılar ve öğelerkavramsal makaleyle çalışma](databases-containers-items.md) başlıklı makaleye bakın. 

| Görev | API başvurusu |
| --- | --- |
| [Yoksa bir kapsayıcı oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts#L14-L15) |[Containers.createIfNotExists](/javascript/api/@azure/cosmos/containers?view=azure-node-latest#createifnotexists-containerrequest--requestoptions-) |
| [Bir hesaptaki kapsayıcıları listeleme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts#L17-L21) |[Containers.readAll](/javascript/api/@azure/cosmos/containers?view=azure-node-latest#readall-feedoptions-) |
| [Kapsayıcı tanımını okuma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts#L23-L26) |[Container.read](/javascript/api/@azure/cosmos/container?view=azure-node-latest#read-requestoptions-) |
| [Kapsayıcı silme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts#L28-L30) |[Container.delete](/javascript/api/@azure/cosmos/container?view=azure-node-latest#delete-requestoptions-) |

## <a name="item-examples"></a>Öğe örnekleri

[ItemManagement](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts) dosyası, öğedeki CRUD işlemlerinin nasıl gerçekleştirilini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos belgeleri hakkında bilgi edinmek için [bkz.](databases-containers-items.md) 

| Görev | API başvurusu |
| --- | --- |
| [Öğe oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L18-L21) |[Items.create](/javascript/api/@azure/cosmos/items?view=azure-node-latest#create-t--requestoptions-) |
| [Bir kapsayıcıdaki tüm öğeleri okuma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L23-L28) |[Items.readAll](/javascript/api/@azure/cosmos/items?view=azure-node-latest#readall-feedoptions-) |
| [Kimliğe göre öğe okuma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L30-L33) |[Item.read](/javascript/api/@azure/cosmos/item?view=azure-node-latest#read-requestoptions-) |
| [Yalnızca değişen öğeleri okuma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L45-L56) |[Item.read](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#accesscondition) |
| [Belgeler için sorgu](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L58-L79) |[Items.query](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest) |
| [Bir öğeyi değiştirme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L81-L96) |[Item.replace](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest) |
| [Koşullu ETag denetimiyle öğeyi değiştirme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L98-L135) |[Item.replace](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest)<br/>[RequestOptions.accessCondition](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#accesscondition) |
| [Öğeyi silme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L137-L140) |[Item.delete](https://docs.microsoft.com/javascript/api/%40azure/cosmos/item?view=azure-node-latest) |

## <a name="indexing-examples"></a>Dizin örnekleri

IndexManagement dosyası [dizin](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts) oluşturmanın nasıl yönetilmeye ne kadar iyi olduğunu gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB'de dizin oluşturma hakkında bilgi edinmek için, [dizin oluşturma ilkeleri,](index-policy.md) [dizin oluşturma türleri](index-types.md)ve [dizine](index-paths.md) alma yolları kavramsal makalelerbakın. 

| Görev | API başvurusu |
| --- | --- |
| [Belirli bir öğeyi el ile dizine](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L52-L75) |[RequestOptions.indexingDirective: 'include'](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#indexingdirective) |
| [Belirli bir öğeyi dizinden el ile hariç tutma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L17-L29) |[RequestOptions.indexingDirective: 'exclude'](https://docs.microsoft.com/javascript/api/%40azure/cosmos/requestoptions?view=azure-node-latest#indexingdirective) |
| [Yolu dizinin dışında tutma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L142-L167) |[IndexingPolicy.ExcludedPath](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingpolicy?view=azure-node-latest#excludedpaths) |
| [Dize yolunda bir aralık dizini oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L87-L112) |[IndexKind.Range](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexkind?view=azure-node-latest), [IndexingPolicy](https://docs.microsoft.com/javascript/api/%40azure/cosmos/indexingpolicy?view=azure-node-latest), [Items.query](https://docs.microsoft.com/javascript/api/%40azure/cosmos/items?view=azure-node-latest) |
| [Varsayılan indexPolicy ile kapsayıcı oluşturma, ardından bunu çevrimiçi güncelleştirme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L13-L15) |[Containers.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/containers?view=azure-node-latest)

## <a name="server-side-programming-examples"></a>Sunucu tarafı programlama örnekleri

[ServerSideScripts](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ServerSideScripts) projesinin [index.ts](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ServerSideScripts/index.ts) dosyası aşağıdaki görevlerin nasıl gerçekleştirilini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB'deki Sunucu tarafı programlama hakkında bilgi edinmek [için, Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler](stored-procedures-triggers-udfs.md) kavramsal makalesine bakın. 

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ServerSideScripts/upsert.js) |[StoredProcedures.create](https://docs.microsoft.com/javascript/api/%40azure/cosmos/storedprocedures?view=azure-node-latest) |
| [Saklı yordamı yürütme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ServerSideScripts/index.ts) |[StoredProcedure.execute](https://docs.microsoft.com/javascript/api/%40azure/cosmos/storedprocedure?view=azure-node-latest) |

Sunucu tarafı programlaması hakkında daha fazla bilgi için bkz. [Azure Cosmos DB sunucu tarafı programlaması: Saklı yordamlar, veritabanı tetikleyicileri ve UDF'leri](stored-procedures-triggers-udfs.md).

