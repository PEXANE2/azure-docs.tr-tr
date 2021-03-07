---
title: 'Azure Cosmos DB: .NET (Microsoft. Azure. Cosmos) SQL API örnekleri'
description: Azure Cosmos DB SQL API 'sini kullanarak genel görevler için GitHub 'da C# .NET v3 SDK örnekleri bulabilirsiniz.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: sample
ms.date: 10/07/2019
ms.author: sngun
ms.custom: devx-track-dotnet
ms.openlocfilehash: 30ccd5783b3fa9524072235a58b3c8708962ef1c
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102434024"
---
# <a name="azure-cosmos-dbnet-v3-sdk-microsoftazurecosmos-examples-for-the-sql-api"></a>SQL API 'SI için Azure Cosmos DB.NET v3 SDK (Microsoft. Azure. Cosmos) örnekleri
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

[Azure-Cosmos-DotNet-v3](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage) GitHub deposu, Azure Cosmos DB kaynakları üzerinde CRUD ve diğer yaygın işlemleri gerçekleştirmeye yönelik en son .NET örnek çözümlerini içerir. .NET SDK 'sının önceki sürümü hakkında bilgi sahibiyseniz, şartlar koleksiyonu ve belgesi için de kullanılabilir. Azure Cosmos DB birden çok API modelini desteklediğinden, .NET SDK 'nın 3,0 sürümü genel "kapsayıcı" ve "öğe" terimlerini kullanır. Bir kapsayıcı koleksiyon, grafik veya tablo olabilir. Öğe de kapsayıcının içinde bulunan belge, kenar/köşe veya satır olabilir. Bu makalede aşağıdakiler sunulmaktadır:

* Örnek C# proje dosyalarının her birindeki görevlere bağlantılar.
* İlgili API başvurusu içeriğine bağlantılar.

## <a name="prerequisites"></a>Önkoşullar

Azure geliştirme iş akışı yüklü Visual Studio 2019

- **Ücretsiz** [Visual Studio 2019 Community Edition](https://www.visualstudio.com/downloads/)'ı indirebilir ve kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

   [Microsoft. Azure. Cosmos NuGet paketi](https://www.nuget.org/packages/Microsoft.Azure.cosmos/)

Bir Azure aboneliği veya ücretsiz Cosmos DB deneme hesabı

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- [Visual Studio abone avantajlarınızı etkinleştirebilirsiniz](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Visual Studio aboneliğiniz, ücretli Azure hizmetleri için kullanabileceğiniz her ay krediler sunar.
- [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]  

> [!NOTE]
> Örnekler, kendi kendine dahil edilir ve kendinden sonra ayarlanır ve temizler. Her oluşum, aboneliğinizi kapsayıcının performans katmanında bir saatlik kullanım için faturalandırır.
> 

## <a name="database-examples"></a>Veritabanı örnekleri

Örnek *Databasemanagement* projesinin [rundatabasedemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L65-L91) yöntemi, aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos veritabanları hakkında bilgi edinmek için bkz. [iş veritabanları, kapsayıcılar ve öğelerle çalışma](account-databases-containers-items.md).

| Görev | API başvurusu |
| --- | --- |
| [Veritabanı oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L68) |[CosmosClient. CreateDatabaseIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.cosmosclient.createdatabaseifnotexistsasync) |
| [KIMLIĞE göre bir veritabanı oku](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L80) |[Database. ReadAsync](/dotnet/api/microsoft.azure.cosmos.database.readasync) |
| [Bir hesabın tüm veritabanlarını okuyun](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L96) |[CosmosClient. Getdatabasequeryıterator](/dotnet/api/microsoft.azure.cosmos.cosmosclient.getdatabasequeryiterator) |
| [Veritabanı silme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/DatabaseManagement/Program.cs#L106) |[Database. DeleteAsync](/dotnet/api/microsoft.azure.cosmos.database.deleteasync) |

## <a name="container-examples"></a>Kapsayıcı örnekleri

Örnek *Containermanagement* projesinin [runcontainerdemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L69-L89) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos kapsayıcıları hakkında bilgi edinmek için bkz. [iş veritabanları, kapsayıcılar ve öğelerle çalışma](account-databases-containers-items.md).

| Görev | API başvurusu |
| --- | --- |
| [Kapsayıcı oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L97-L107) |[Database. CreateContainerIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync) |
| [Özel dizin ilkesiyle kapsayıcı oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L161-L178) |[Database. CreateContainerIfNotExistsAsync](/dotnet/api/microsoft.azure.cosmos.database.createcontainerifnotexistsasync) |
| [Bir kapsayıcının yapılandırılmış performansını değiştirme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L198-L223) |[Container. ReplaceThroughputAsync](/dotnet/api/microsoft.azure.cosmos.container.replacethroughputasync) |
| [KIMLIĞE göre kapsayıcı al](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L225-L236) |[Container. Readcontainsilinebilir eşitleme](/dotnet/api/microsoft.azure.cosmos.container.readcontainerasync) |
| [Bir veritabanındaki tüm kapsayıcıları okuyun](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L242-L258) |[Database. Getcontainerqueryıterator](/dotnet/api/microsoft.azure.cosmos.database.getcontainerqueryiterator) |
| [Kapsayıcı silme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ContainerManagement/Program.cs#L264-L270) |[Container. Deletecontainsilsync](/dotnet/api/microsoft.azure.cosmos.container.deletecontainerasync) |

## <a name="item-examples"></a>Öğe örnekleri

Örnek *ıtemmanagement* projesinin [runıtemdemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L119-L130) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos öğeleri hakkında bilgi edinmek için bkz. [iş veritabanları, kapsayıcılar ve öğelerle çalışma](account-databases-containers-items.md).

| Görev | API başvurusu |
| --- | --- |
| [Öğe oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L161-L200) |[Container. Createıtemasync](/dotnet/api/microsoft.azure.cosmos.container.createitemasync) |
| [Kimliğe göre öğe okuma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L203-L241) |[kapsayıcı. ReadItemAsync](/dotnet/api/microsoft.azure.cosmos.container.readitemasync) |
| [Öğeler için sorgu](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L244-L320) |[kapsayıcı. Getıtemqueryıterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator) |
| [Bir öğeyi değiştirme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L415-L456) |[kapsayıcı. ReplaceItemAsync](/dotnet/api/microsoft.azure.cosmos.container.replaceitemasync) |
| [Bir öğeyi yukarı Ekle](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L458-L509) |[kapsayıcı. UpsertItemAsync](/dotnet/api/microsoft.azure.cosmos.container.upsertitemasync) |
| [Öğeyi silme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L511-L522) |[kapsayıcı. DeleteItemAsync](/dotnet/api/microsoft.azure.cosmos.container.deleteitemasync) |
| [Bir öğeyi koşullu ETag denetimi ile değiştirme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L682-L772) |[RequestOptions. IfMatchEtag](/dotnet/api/microsoft.azure.cosmos.requestoptions.ifmatchetag) |

## <a name="indexing-examples"></a>Dizin örnekleri

Örnek *ındexmanagement* projesinin [runındexdemo](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L108-L122) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB Dizin oluşturma hakkında bilgi edinmek için bkz. [Dizin ilkeleri](index-policy.md), [Dizin türleri](index-overview.md#index-types)ve [dizin yolları](index-policy.md#include-exclude-paths). 

| Görev | API başvurusu |
| --- | --- |
| [Dizinden bir öğeyi dışlama](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L130-L186) |[IndexingDirective.Exclude](/dotnet/api/microsoft.azure.cosmos.indexingdirective) |
| [Geç dizin oluşturma kullanma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L198-L220) |[Indexingpolicy. ındexingmode](/dotnet/api/microsoft.azure.cosmos.indexingpolicy.indexingmode) |
| [Belirtilen öğe yollarını dizinden Dışla](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/IndexManagement/Program.cs#L230-L297) |[IndexingPolicy.ExcludedPaths](/dotnet/api/microsoft.azure.cosmos.indexingpolicy.excludedpaths) |

## <a name="query-examples"></a>Sorgu örnekleri

Örnek *sorgular* projesinin [rundemoasync](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L76-L96) yöntemi, SQL sorgu DILBILGISI, sorgu Içeren LINQ sağlayıcısı ve lambda kullanılarak aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB SQL sorgu başvurusu hakkında bilgi edinmek için, bkz. [SQL sorgu örnekleri Azure Cosmos DB](./sql-query-getting-started.md).

| Görev | API başvurusu |
| --- | --- |
| [Öğeleri tek bir bölümden sorgula](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L154-L186) |[kapsayıcı. Getıtemqueryıterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator) |
| [Birden çok bölümden öğe sorgula](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L215-L275) |[kapsayıcı. Getıtemqueryıterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator) |
| [SQL ifadesini kullanarak sorgulama](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/Queries/Program.cs#L189-L212) |[kapsayıcı. Getıtemqueryıterator](/dotnet/api/microsoft.azure.cosmos.container.getitemqueryiterator) |

## <a name="change-feed-examples"></a>Akış örneklerini değiştirme

Örnek *changefeed* projesinin [runbasicchangefeed](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L91-L119) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB akışı değiştirme hakkında bilgi edinmek için bkz. [okuma Azure Cosmos DB değiştirme akışı](read-change-feed.md) ve [akış işlemcisini değiştirme](change-feed-processor.md).

| Görev | API başvurusu |
| --- | --- |
| [Temel değişiklik akışı işlevselliği](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L91-L119) |[Container. GetChangeFeedProcessorBuilder](/dotnet/api/microsoft.azure.cosmos.container.getchangefeedprocessorbuilder) |
| [Belirli bir zamandan değişiklik akışını okuma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L127-L162) |[Container. GetChangeFeedProcessorBuilder](/dotnet/api/microsoft.azure.cosmos.container.getchangefeedprocessorbuilder) |
| [Baştan başlayarak değişiklik akışını okuma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L170-L198) |[ChangeFeedProcessorBuilder. WithStartTime (TarihSaat)](/dotnet/api/microsoft.azure.cosmos.changefeedprocessorbuilder.withstarttime) |
| [V3 SDK 'sında değişiklik akışı işlemcisinden akışa geçiş akışını geçirme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs#L256-L333) |[Container. GetChangeFeedProcessorBuilder](/dotnet/api/microsoft.azure.cosmos.container.getchangefeedprocessorbuilder) |

## <a name="server-side-programming-examples"></a>Sunucu tarafı programlama örnekleri

Örnek *Serversıdescripts* projesinin [rundemoasync](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L72-L102) yöntemi aşağıdaki görevlerin nasıl yapılacağını gösterir. Aşağıdaki örnekleri çalıştırmadan önce Azure Cosmos DB sunucu tarafı programlama hakkında bilgi edinmek için, bkz. [saklı yordamlar, Tetikleyiciler ve Kullanıcı tanımlı işlevler](stored-procedures-triggers-udfs.md).

| Görev | API başvurusu |
| --- | --- |
| [Saklı yordam oluşturma](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L116) |[Scripts. CreateStoredProcedureAsync](/dotnet/api/microsoft.azure.cosmos.scripts.scripts.createstoredprocedureasync) |
| [Saklı yordamı yürütme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L135) |[Scripts.ExecuteStoredProcedureAsync](/dotnet/api/microsoft.azure.cosmos.scripts.scripts.executestoredprocedureasync) |
| [Saklı yordamı silme](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ServerSideScripts/Program.cs#L351) |[Scripts. DeleteStoredProcedureAsync](/dotnet/api/microsoft.azure.cosmos.scripts.scripts.deletestoredprocedureasync) |
