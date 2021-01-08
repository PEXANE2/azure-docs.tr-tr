---
title: Azure Cosmos veritabanındaki verileri yönetmek için örnek Node.js
description: CRUD işlemleri gibi Azure Cosmos DB'deki yaygın görevler için Github'da Node.js örnekleri bulabilirsiniz.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 08/23/2019
ms.author: dech
ms.custom: devx-track-js
ms.openlocfilehash: 192af33c6f07d38daef3a183fa8d746ff082ce2b
ms.sourcegitcommit: 42a4d0e8fa84609bec0f6c241abe1c20036b9575
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98018981"
---
# <a name="nodejs-examples-to-manage-data-in-azure-cosmos-db"></a>Azure Cosmos DB’de verileri yönetmeye yönelik Node.js örnekleri
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

> [!div class="op_single_selector"]
> * [.NET v2 SDK örnekleri](sql-api-dotnet-samples.md)
> * [.NET v3 SDK örnekleri](sql-api-dotnet-v3sdk-samples.md)
> * [Java v4 SDK örnekleri](sql-api-java-sdk-samples.md)
> * [Spring Data v3 SDK örnekleri](sql-api-spring-data-sdk-samples.md)
> * [Node.js Örnekleri](sql-api-nodejs-samples.md)
> * [Python Örnekleri](sql-api-python-samples.md)
> * [Azure Kod Örneği Galerisi](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

Azure Cosmos DB kaynaklarında CRUD işlemlerini ve diğer yaygın işlemleri gerçekleştiren örnek çözümler [Azure-Cosmos-js](https://github.com/Azure/azure-cosmos-js/tree/master/samples) GitHub deposuna dahil edilmiştir. Bu makalede aşağıdakiler sunulmaktadır:

* Node.js örnek proje dosyalarının her birindeki görevlere bağlantılar.
* İlgili API başvurusu içeriğine bağlantılar.

**Önkoşullar**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- [Visual Studio abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, her ay size ücretli Azure hizmetleri için kullanabileceğiniz krediler verir.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[JavaScript SDK'sına](sql-api-sdk-node.md) da ihtiyacınız vardır.
   
   > [!NOTE]
   > Örnekler birbirinden bağımsızdır, kendi kendine ayarlanır ve sonra kendini temizler. Böyle olunca, örneklerde birden çok [Containers.create](/javascript/api/%40azure/cosmos/containers?preserve-view=true&view=azure-node-latest) çağrısı yapılır. Bu her yapıldığında, oluşturulan kapsayıcının performans katmanı başına aboneliğiniz 1 saatlik kullanım için faturalandırılır.
   > 
   > 

## <a name="database-examples"></a>Veritabanı örnekleri

[Databasemanagement](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts) dosyası veritabanında CRUD işlemlerinin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos veritabanları hakkında bilgi edinmek için bkz. [veritabanları, kapsayıcılar ve öğelerle çalışma](account-databases-containers-items.md) kavramsal makalesi. 

| Görev | API başvurusu |
| --- | --- |
| [Yoksa bir veritabanı oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts#L12-L14) |[Databases.createIfNotExists](/javascript/api/@azure/cosmos/databases?view=azure-node-latest&preserve-view=true#createifnotexists-databaserequest--requestoptions-) |
| [Hesabın veritabanlarını listeleme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts#L16-L18) |[Databases.readAll](/javascript/api/@azure/cosmos/databases?view=azure-node-latest&preserve-view=true#readall-feedoptions-) |
| [KIMLIĞE göre bir veritabanı oku](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts#L20-L29) |[Database.read](/javascript/api/@azure/cosmos/database?view=azure-node-latest&preserve-view=true#read-requestoptions-) |
| [Veritabanı silme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/DatabaseManagement.ts#L31-L32) |[Database.delete](/javascript/api/@azure/cosmos/database?view=azure-node-latest&preserve-view=true#delete-requestoptions-) |

## <a name="container-examples"></a>Kapsayıcı örnekleri

[Containermanagement](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts) dosyası, kapsayıcıda CRUD işlemlerinin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos koleksiyonları hakkında bilgi edinmek için bkz. [veritabanları, kapsayıcılar ve öğelerle çalışma](account-databases-containers-items.md) kavramsal makalesi. 

| Görev | API başvurusu |
| --- | --- |
| [Yoksa bir kapsayıcı oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts#L14-L15) |[Containers.createIfNotExists](/javascript/api/@azure/cosmos/containers?view=azure-node-latest&preserve-view=true#createifnotexists-containerrequest--requestoptions-) |
| [Bir hesaptaki kapsayıcıları listeleme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts#L17-L21) |[Containers.readAll](/javascript/api/@azure/cosmos/containers?view=azure-node-latest&preserve-view=true#readall-feedoptions-) |
| [Kapsayıcı tanımını okuma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts#L23-L26) |[Container.read](/javascript/api/@azure/cosmos/container?view=azure-node-latest&preserve-view=true#read-requestoptions-) |
| [Kapsayıcı silme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ContainerManagement.ts#L28-L30) |[Container.delete](/javascript/api/@azure/cosmos/container?view=azure-node-latest&preserve-view=true#delete-requestoptions-) |

## <a name="item-examples"></a>Öğe örnekleri

[Itemmanagement](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts) dosyası öğede CRUD işlemlerinin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos belgeleri hakkında bilgi edinmek için bkz. [veritabanları, kapsayıcılar ve öğelerle çalışma](account-databases-containers-items.md) kavramsal makalesi. 

| Görev | API başvurusu |
| --- | --- |
| [Öğe oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L18-L21) |[Items.create](/javascript/api/@azure/cosmos/items?view=azure-node-latest&preserve-view=true#create-t--requestoptions-) |
| [Bir kapsayıcıdaki tüm öğeleri okuma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L23-L28) |[Items.readAll](/javascript/api/@azure/cosmos/items?view=azure-node-latest&preserve-view=true#readall-feedoptions-) |
| [Kimliğe göre öğe okuma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L30-L33) |[Item.read](/javascript/api/@azure/cosmos/item?view=azure-node-latest&preserve-view=true#read-requestoptions-) |
| [Yalnızca değişen öğeleri okuma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L45-L56) |[Item.read](/javascript/api/%40azure/cosmos/item?preserve-view=true&view=azure-node-latest)<br/>[RequestOptions.accessCondition](/javascript/api/%40azure/cosmos/requestoptions?preserve-view=true&view=azure-node-latest#accesscondition) |
| [Belgeler için sorgu](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L58-L79) |[Items.query](/javascript/api/%40azure/cosmos/items?preserve-view=true&view=azure-node-latest) |
| [Bir öğeyi değiştirme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L81-L96) |[Item.replace](/javascript/api/%40azure/cosmos/item?preserve-view=true&view=azure-node-latest) |
| [Koşullu ETag denetimiyle öğeyi değiştirme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L98-L135) |[Item.replace](/javascript/api/%40azure/cosmos/item?preserve-view=true&view=azure-node-latest)<br/>[RequestOptions.accessCondition](/javascript/api/%40azure/cosmos/requestoptions?preserve-view=true&view=azure-node-latest#accesscondition) |
| [Öğeyi silme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ItemManagement.ts#L137-L140) |[Item.delete](/javascript/api/%40azure/cosmos/item?preserve-view=true&view=azure-node-latest) |

## <a name="indexing-examples"></a>Dizin örnekleri

[Indexmanagement](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts) dosyası dizin oluşturma işleminin nasıl yönetileceğini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB Dizin oluşturma hakkında bilgi edinmek için bkz. [Dizin oluşturma ilkeleri](index-policy.md), [Dizin oluşturma türleri](index-overview.md#index-types)ve [Dizin oluşturma yolları](index-policy.md#include-exclude-paths) kavramsal makaleler. 

| Görev | API başvurusu |
| --- | --- |
| [Belirli bir öğeyi el ile dizin oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L52-L75) |[RequestOptions.indexingDirective: 'include'](/javascript/api/%40azure/cosmos/requestoptions?preserve-view=true&view=azure-node-latest#indexingdirective) |
| [Belirli bir öğeyi dizinden el ile dışlama](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L17-L29) |[RequestOptions.indexingDirective: 'exclude'](/javascript/api/%40azure/cosmos/requestoptions?preserve-view=true&view=azure-node-latest#indexingdirective) |
| [Yolu dizinin dışında tutma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L142-L167) |[IndexingPolicy.ExcludedPath](/javascript/api/%40azure/cosmos/indexingpolicy?preserve-view=true&view=azure-node-latest#excludedpaths) |
| [Dize yolunda bir aralık dizini oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L87-L112) |[IndexKind.Range](/javascript/api/%40azure/cosmos/indexkind?preserve-view=true&view=azure-node-latest), [IndexingPolicy](/javascript/api/%40azure/cosmos/indexingpolicy?preserve-view=true&view=azure-node-latest), [Items.query](/javascript/api/%40azure/cosmos/items?preserve-view=true&view=azure-node-latest) |
| [Varsayılan indexPolicy ile kapsayıcı oluşturma, ardından bunu çevrimiçi güncelleştirme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/IndexManagement.ts#L13-L15) |[Containers.create](/javascript/api/%40azure/cosmos/containers?preserve-view=true&view=azure-node-latest)

## <a name="server-side-programming-examples"></a>Sunucu tarafı programlama örnekleri

[Serversıdescripts](https://github.com/Azure/azure-cosmos-js/tree/master/samples/ServerSideScripts) projesinin [index. TS](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ServerSideScripts/index.ts) dosyası, aşağıdaki görevlerin nasıl gerçekleştirileceğini gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB içindeki sunucu tarafı programlama hakkında bilgi edinmek için bkz. [saklı yordamlar, Tetikleyiciler ve Kullanıcı tanımlı işlevler](stored-procedures-triggers-udfs.md) kavramsal makalesi. 

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ServerSideScripts/upsert.js) |[StoredProcedures.create](/javascript/api/%40azure/cosmos/storedprocedures?preserve-view=true&view=azure-node-latest) |
| [Saklı yordamı yürütme](https://github.com/Azure/azure-cosmos-js/blob/master/samples/ServerSideScripts/index.ts) |[StoredProcedure.execute](/javascript/api/%40azure/cosmos/storedprocedure?preserve-view=true&view=azure-node-latest) |

Sunucu tarafı programlaması hakkında daha fazla bilgi için bkz. [Azure Cosmos DB sunucu tarafı programlaması: Saklı yordamlar, veritabanı tetikleyicileri ve UDF'leri](stored-procedures-triggers-udfs.md).
