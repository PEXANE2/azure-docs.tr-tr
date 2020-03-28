---
title: 'Azure Cosmos DB: .NET (Microsoft.Azure.Cosmos) SQL API örnekleri'
description: Azure Cosmos DB SQL API'yi kullanarak sık kullanılan görevler için GitHub'daki C# .NET V3 SDK örneklerini bulun.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 10/07/2019
ms.author: sngun
ms.openlocfilehash: 2a33a59ae0184e6c41fe7121560bc5df3a69cffd
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73888944"
---
# <a name="azure-cosmos-dbnet-v3-sdk-microsoftazurecosmos-examples-for-the-sql-api"></a>SQL API için Azure Cosmos DB.NET V3 SDK (Microsoft.Azure.Cosmos) örnekleri

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

[Azure-cosmos-dotnet-v3](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage) GitHub deposu, Azure Cosmos DB kaynaklarında CRUD ve diğer yaygın işlemleri gerçekleştirmek için en son .NET örnek çözüm çözümlerini içerir. .NET SDK'nın önceki sürümüne aşinaysanız, terimlerin toplanması ve belgeiçin kullanılabilirsiniz. Azure Cosmos DB birden çok API modelini desteklediğinden, .NET SDK'nın 3.0 sürümü genel "kapsayıcı" ve "öğe" terimlerini kullanır. Bir kapsayıcı koleksiyon, grafik veya tablo olabilir. Öğe de kapsayıcının içinde bulunan belge, kenar/köşe veya satır olabilir. Bu makalede aşağıdakiler sunulmaktadır:

* Örnek C# proje dosyalarının her birindeki görevlere bağlantılar.
* İlgili API başvurusu içeriğine bağlantılar.

## <a name="prerequisites"></a>Ön koşullar

Azure geliştirme iş akışı yüklü Visual Studio 2019

- Ücretsiz Visual Studio **free** [2019 Community Edition'ı](https://www.visualstudio.com/downloads/)indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

   [Microsoft.Azure.cosmos NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.cosmos/)

Azure aboneliği veya ücretsiz Cosmos DB deneme hesabı

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- Visual [Studio abone avantajlarını etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, ücretli Azure hizmetleri için kullanabileceğiniz her ay kredi verir.
- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

> [!NOTE]
> Örnekler kendi kendine yeten, ve kurmak ve kendi lerinden sonra temizleyin. Her olay, kabınızın performans katmanında bir saatlik kullanım için aboneliğinizi faturalar.
> 

## <a name="database-examples"></a>Veritabanı örnekleri

Örnek *DatabaseManagement* projesinin [RunDatabaseDemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L65-L91) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos veritabanları hakkında bilgi edinmek için [veritabanları, kapsayıcılar ve öğelerle çalışma'ya](databases-containers-items.md)bakın.

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L68) |[CosmosClient.CreateDatabaseIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.cosmosclient.createdatabaseifnotexistsasync?view=azure-dotnet) |
| [Veritabanını kimlikle okuma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L80) |[Database.ReadAsync](/dotnet/api/microsoft.azure.cosmos.database.readasync?view=azure-dotnet) |
| [Bir hesap için tüm veritabanlarını okuyun](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L96) |[CosmosClient.GetDatabaseQueryIterator](/dotnet/api/microsoft.azure.cosmos.cosmosclient.getdatabasequeryiterator?view=azure-dotnet) |
| [Veritabanı silme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L106) |[Database.DeleteAsync](/dotnet/api/microsoft.azure.cosmos.database.deleteasync?view=azure-dotnet) |

## <a name="container-examples"></a>Kapsayıcı örnekleri

Örnek *ContainerManagement* projesinin [RunContainerDemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L69-L89) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos kapsayıcıları hakkında bilgi edinmek için [veritabanları, kapsayıcılar ve öğelerle çalışma'ya](databases-containers-items.md)bakın.

| Görev | API başvurusu |
| --- | --- |
| [Bir kapsayıcı oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L97-L107) |[Database.CreateContainerIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync?view=azure-dotnet) |
| [Özel dizin ilkesine sahip bir kapsayıcı oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L111-L127) |[Database.CreateContainerIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync?view=azure-dotnet) |
| [Bir kapsayıcının yapılandırılmış performansını değiştirme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L149-L171) |[Konteyner.ReplaceThroughputAsync](/dotnet/api/microsoft.azure.cosmos.container.replacethroughputasync?view=azure-dotnet) |
| [Kimlikle bir konteyner alma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L176-L185) |[Konteyner.ReadContainerAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.container.readcontainerasync?view=azure-dotnet) |
| [Veritabanındaki tüm kapsayıcıları okuyun](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L193-L205) |[Database.GetContainerQueryIterator](/dotnet/api/microsoft.azure.cosmos.database.getcontainerqueryiterator?view=azure-dotnet) |
| [Kapsayıcı silme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L213-L2018) |[Konteyner.DeleteContainerAsync](/dotnet/api/microsoft.azure.cosmos.container.deletecontainerasync?view=azure-dotnet) |

## <a name="item-examples"></a>Öğe örnekleri

Örnek *ItemManagement* projesinin [RunItemsDemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L119-L130) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos öğeleri hakkında bilgi edinmek için [veritabanları, kapsayıcılar ve öğelerle çalışma'ya](databases-containers-items.md)bakın.

| Görev | API başvurusu |
| --- | --- |
| [Öğe oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L161-L200) |[Container.CreateItemAsync](/dotnet/api/microsoft.azure.cosmos.container.readitemasync?view=azure-dotnet) |
| [Kimliğe göre öğe okuma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L203-L241) |[Kapsayıcı. ReadItemAsync](/dotnet/api/microsoft.azure.cosmos.container.readitemasync?view=azure-dotnet) |
| [Öğeler için sorgu](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L244-L320) |[Kapsayıcı. GetItemQueryIterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator?view=azure-dotnet) |
| [Bir öğeyi değiştirme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L323-L363) |[Kapsayıcı. YerineItemAsync](/dotnet/api/microsoft.azure.cosmos.container.replaceitemasync?view=azure-dotnet) |
| [Öğeyi yükseltme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L366-L411) |[Kapsayıcı. UpsertItemAsync](/dotnet/api/microsoft.azure.cosmos.container.upsertitemasync?view=azure-dotnet) |
| [Öğeyi silme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L414-L424) |[Kapsayıcı. DeleteItemAsync](/dotnet/api/microsoft.azure.cosmos.container.deleteitemasync?view=azure-dotnet) |
| [Bir öğeyi koşullu ETag denetimiyle değiştirme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L585-L674) |[RequestOptions.IfMatchEtag](/dotnet/api/microsoft.azure.cosmos.requestoptions.ifmatchetag?view=azure-dotnet) |

## <a name="indexing-examples"></a>Dizin örnekleri

Örnek *IndexManagement* projesinin [RunIndexDemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L108-L122) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB'de dizin oluşturma hakkında bilgi edinmek için [dizin ilkelerine, dizin](index-policy.md) [türlerine](index-types.md)ve [dizin yollarına](index-paths.md)bakın. 

| Görev | API başvurusu |
| --- | --- |
| [Bir öğeyi dizinden hariç tutma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L130-L186) |[IndexingDirective.Exclude](/dotnet/api/microsoft.azure.cosmos.indexingdirective?view=azure-dotnet) |
| [Tembel dizini kullanma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L198-L220) |[IndexingPolicy.IndexingMode](/dotnet/api/microsoft.azure.cosmos.indexingpolicy.indexingmode?view=azure-dotnet) |
| [Belirtilen madde yollarını dizinden hariç tutma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L230-L297) |[IndexingPolicy.ExcludedPaths](/dotnet/api/microsoft.azure.cosmos.indexingpolicy.excludedpaths?view=azure-dotnet) |

## <a name="query-examples"></a>Sorgu örnekleri

Örnek *Sorgular* projesinin [RunDemoAsync](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L76-L96) yöntemi, SQL sorgu gramerini, sorgulu LINQ sağlayıcısını ve Lambda'yı kullanarak aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB'deki SQL sorgu başvurusu hakkında bilgi edinmek [için Azure Cosmos DB için SQL sorgu örneklerine](how-to-sql-query.md)bakın.

| Görev | API başvurusu |
| --- | --- |
| [Öğeleri tek bölümden sorgula](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L154-L186) |[Kapsayıcı. GetItemQueryIterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator?view=azure-dotnet) |
| [Birden çok bölümden öğeleri sorgula](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L215-L275) |[Kapsayıcı. GetItemQueryIterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator?view=azure-dotnet) |
| [SQL deyimini kullanarak sorgu](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L189-L212) |[Kapsayıcı. GetItemQueryIterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator?view=azure-dotnet) |

## <a name="change-feed-examples"></a>Akış örneklerini değiştirme

Örnek *ChangeFeed* projesinin [RunBasicChangeFeed](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L91-L119) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB'deki akışı değiştirme hakkında bilgi edinmek [Change feed processor](change-feed-processor.md)için [bkz.](read-change-feed.md)

| Görev | API başvurusu |
| --- | --- |
| [Temel değişiklik akışı işlevselliği](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L91-L119) |[Container.GetChangeFeedProcessorBuilder](/dotnet/api/microsoft.azure.cosmos.container.getchangefeedprocessorbuilder?view=azure-dotnet) |
| [Belirli bir zamana ait değişiklik akışını okuma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L127-L162) |[Container.GetChangeFeedProcessorBuilder](/dotnet/api/microsoft.azure.cosmos.container.getchangefeedprocessorbuilder?view=azure-dotnet) |
| [Değişiklik akışını baştan okuma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L170-L198) |[ChangeFeedProcessorBuilder.WithStartTime(DateTime)](/dotnet/api/microsoft.azure.cosmos.changefeedprocessorbuilder.withstarttime?view=azure-dotnet) |
| [V3 SDK'da beslemeyi değiştirmek için besleme işlemcisini değiştirme mIgrate](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L256-L333) |[Container.GetChangeFeedProcessorBuilder](/dotnet/api/microsoft.azure.cosmos.container.getchangefeedprocessorbuilder?view=azure-dotnet) |

## <a name="server-side-programming-examples"></a>Sunucu tarafı programlama örnekleri

Örnek *ServerSideScripts* projesinin [RunDemoAsync](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L72-L102) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB'de sunucu tarafı programlama hakkında bilgi edinmek [için, Saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler](stored-procedures-triggers-udfs.md)bkz.

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L116) |[Scripts.CreateStoredProcedureAsync](/dotnet/api/microsoft.azure.cosmos.scripts.scripts.createstoredprocedureasync?view=azure-dotnet) |
| [Saklı yordamı yürütme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L135) |[Scripts.ExecuteStoredProcedureAsync](/dotnet/api/microsoft.azure.cosmos.scripts.scripts.executestoredprocedureasync?view=azure-dotnet) |
| [Depolanan yordamı silme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L351) |[Scripts.DeleteStoredProcedureAsync](/dotnet/api/microsoft.azure.cosmos.scripts.scripts.deletestoredprocedureasync?view=azure-dotnet) |