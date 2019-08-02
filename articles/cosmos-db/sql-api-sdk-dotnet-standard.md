---
title: 'Azure Cosmos DB: SQL .NET Standard API, SDK & kaynakları'
description: Sürüm tarihleri, kullanımdan kaldırma tarihleri ve Azure Cosmos DB .NET SDK 'nın her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL API ve .NET SDK hakkında bilgi edinin.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 07/12/2019
ms.author: dech
ms.openlocfilehash: 0368e99135dd6e377dd2820b3e673c55182319b9
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663806"
---
# <a name="azure-cosmos-db-net-standard-sdk-for-sql-api-download-and-release-notes"></a>SQL API için .NET Standard SDK Azure Cosmos DB: Notları indir ve serbest bırak
> [!div class="op_single_selector"]
> * [.NET Standard](sql-api-sdk-dotnet-standard.md)
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**SDK'sını indirme**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/)|
|**API belgeleri**|[.NET API başvuru belgeleri](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Örnekler**|[.NET kodu örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/CodeSamples)|
|**Kullanmaya başlama**|[Azure Cosmos DB .NET SDK ile çalışmaya başlama](sql-api-get-started.md)|
|**Web uygulaması Öğreticisi**|[Azure Cosmos DB ile Web uygulaması geliştirme](sql-api-dotnet-application.md)|
|**Geçerli desteklenen çerçevesi**|[Microsoft .NET Standart 2,0](/dotnet/standard/net-standard)|

## <a name="release-notes"></a>Sürüm notları
### <a name="a-name310310"></a><a name="3.1.0"/>3.1.0
#### <a name="added"></a>Eklendi
- [#541](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/541) İstemci ve sorgu seçeneklerine tutarlılık düzeyi eklendi
- [#544](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/544) LINQ için devamlılık belirteci desteği eklendi
- [#557](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/557) Öğe isteği seçeneklerine tetikleyici seçenekleri eklendi
- [#571](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/571) İsteğe bağlı ayarlarla varsayılan bir JSON.net serileştirici eklendi
- [#572](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/572) Createcontainerıfnotexistsasync üzerine bölüm anahtarı doğrulaması eklendi
- [#581](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/581) LINQ to QueryDefinition API 'SI ekleniyor
- [#592](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/592) Container Builder 'a CreateIfNotExistsAsync eklendi
- [#597](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/597) ResponseMessage 'a devamlılık belirteci özelliği eklendi
- [#604](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/604) LINQ Tostreamıterator genişletme yöntemi eklendi

#### <a name="fixed"></a>Sabit
- [#548](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/548) CosmosException. ToString (); içinde yanlış yazılmış ileti düzeltildi
- [#558](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/558) LocationCache ConcurrentDict kilit çakışması onarımı
- [#561](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/561) Getıtemlinqqueryable artık null sorgu ile çalışmaktadır
- [#567](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/567) Sorgu farklı dil kültürlerini doğru şekilde işliyor
- [#574](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/574) Sorgu ayrıştırma beklenmeyen bir özel durumla başarısız olursa, düzeltilen boş hata iletisi
- [#576](https://github.com/Azure/azure-cosmos-dotnet-v3/pull/576) Sorgu girişi bir akışa doğru bir şekilde seri hale getirir

### <a name="a-name300300"></a><a name="3.0.0"/>3.0.0 
* .NET SDK 'nın [sürüm 3.0.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) genel kullanılabilirliği
* .NET Framework 4.6.1 + ve .NET Core 2.0 + destekleyen .NET Standard 2,0 hedefleri
* En üst düzey CosmosClient ve yöntemlerle ilgili veritabanı ve kapsayıcı sınıflarında bölünmüş yeni nesne modeli
* Yeni yüksek performanslı akış API 'Leri
* Değişiklik akışı işlemcisi API 'Leri için yerleşik destek
* CosmosClient, kapsayıcı ve değişiklik akışı işlemcisi için akıcı Oluşturucu API 'Leri
* Deyim işleme yönetimi API 'Leri
* Veritabanı, kapsayıcı, öğe, sorgu ve aktarım hızı istekleri için parçalı RequestOptions ve ResponseTypes
* Bölümlendirilmemiş kapsayıcıları ölçeklendirebilme 
* Genişletilebilir ve özelleştirilebilir seri hale getirici
* Özel işleyiciler desteğiyle Genişletilebilir istek işlem hattı


## <a name="release--retirement-dates"></a>Yayın & kullanımdan kaldırma tarihleri
Microsoft, daha yeni/desteklenen bir sürüme geçişi düzgünleştirmek için SDK 'nın devre dışı bırakılmasının ardından en az **12 ay** önce bildirim sağlar.

Yeni özellikler ve işlevler ve iyileştirmeler yalnızca geçerli SDK 'ya eklenir, bu nedenle en son SDK sürümüne her zaman olabildiğince erken yükseltmeniz önerilir. 

Kullanımdan kaldırılan bir SDK kullanarak Azure Cosmos DB yönelik istekler hizmet tarafından reddedilir.

<br/>

| Version | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [3.1.0](#3.1.0) |29 Temmuz 2019 |--- |
| [3.0.0](#3.0.0) |15 Temmuz 2019 |--- |


## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmeti sayfası. 

