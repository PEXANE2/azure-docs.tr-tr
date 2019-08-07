---
title: 'Azure Cosmos DB: SQL .NET Core API, SDK & kaynakları'
description: Tüm SQL .NET Core API ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB .NET Core SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2018
ms.author: sngun
ms.openlocfilehash: 58db76401439befd476bc78c1a1a20eeed5c2342
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779159"
---
# <a name="azure-cosmos-db-net-core-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için Azure Cosmos DB .NET Core SDK: Sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [.NET Standard](sql-api-sdk-dotnet-standard.md)
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik akışı](sql-api-sdk-dotnet-changefeed.md)
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
|**SDK'sını indirme**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)|
|**API belgeleri**|[.NET API başvuru belgeleri](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Örnekler**|[.NET kodu örnekleri](sql-api-dotnet-samples.md)|
|**Kullanmaya başlama**|[Azure Cosmos DB .NET ile çalışmaya başlama](sql-api-sdk-dotnet.md)|
|**Web uygulaması Öğreticisi**|[Azure Cosmos DB ile Web uygulaması geliştirme](sql-api-dotnet-application.md)|
|**Geçerli desteklenen çerçevesi**|[.NET standard 1.6 ve .NET standart 1.5](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>Sürüm Notları

> [!NOTE]
> .NET Core kullanıyorsanız, lütfen .NET Standard hedefleyen [.NET SDK 'sının](sql-api-sdk-dotnet-standard.md)en son sürüm 3. x sürümünü inceleyin. 

### <a name="a-name251251"></a><a name="2.5.1"/>2.5.1

* SDK 'nın System .net. http sürümü artık NuGet paketinde tanımlananla eşleşiyor.
* Özgün diğeri başarısız olursa, yazma isteklerinin farklı bir bölgeye geri dönüş yapmasına izin verin.
* Yazma isteği için oturum yeniden deneme ilkesi ekleyin.

### <a name="a-name241241"></a><a name="2.4.1"/>2.4.1

* Boş sayfalara neden olan sorgular için izleme yarış durumunu düzeltir

### <a name="a-name240240"></a><a name="2.4.0"/>2.4.0

* LINQ sorguları için artırılmış ondalık duyarlık boyutu.
* Yeni sınıflar CompositePath, CompositePathSortOrder, SpatialSpec, SpatialType ve PartitionKeyDefinitionVersion eklendi
* DocumentCollection 'a TimeToLivePropertyPath eklendi
* Indexpolicy 'e Compositeındexes ve Spatıalındexes eklendi
* PartitionKeyDefinition 'a eklenen sürüm
* PartitionKey 'e None eklendi

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0

 * ConnectionPolicy için ıdletcpconnectiontimeout, OpenTcpConnectionTimeout, MaxRequestsPerTcpConnection ve MaxTcpConnectionsPerEndpoint eklendi.
 
### <a name="a-name223223"></a><a name="2.2.3"/>2.2.3

* Tanılama geliştirmeleri

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2

* "POCOSerializationOnly" ortam değişkeni ayarı eklendi.

* DocumentDB. uzamsal. Sql. dll kaldırıldı ve artık Microsoft. Azure. Documents. Serviceınterop. dll dosyasına eklendi

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1

* StoredProcedure yürütme çağrılarına yük devretme sırasında yeniden deneme mantığı geliştirme.

* DocumentClientEventSource Singleton yapıldı. 

* GatewayAddressCache zaman aşımını çözme, ConnectionPolicy RequestTimeout özelliğini karşılamıyor.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0

* Doğrudan/TCP Aktarım tanılaması için SDK 'nın iç özel durum türü TransportException eklenmiştir. Özel durum iletilerinde mevcut olduğunda, bu tür istemci bağlantı sorunlarını gidermek için ek bilgileri yazdırır.

* HttpClient istekleri (ör. HttpClientHandler) göndermek için kullanılacak bir HTTP işleyici yığını olan bir HttpMessageHandler 'ı alan yeni Oluşturucu aşırı yüklemesi eklendi.

* Null değerli üstbilginin düzgün işlenmediğinden oluşan hatayı düzeltir.

* Geliştirilmiş koleksiyon önbelleği doğrulaması.

### <a name="a-name213213"></a><a name="2.1.3"/>2.1.3

* Güncelleştirilmiş System.Net.Security 4.3.2 için.

### <a name="a-name212212"></a><a name="2.1.2"/>2.1.2'yi

* Tanılama izleme geliştirmeleri.

### <a name="a-name211211"></a><a name="2.1.1"/>2.1.1

* Çok bölgeli isteği geçici hatalara karşı daha fazla esneklik eklenmiştir.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0

* Eklenen çok bölgeli yazma desteği.
* Bölüm sorgu performansı geliştirmelerinin üst ve MaxBufferedItemCount çapraz.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0

* Eklenen isteği iptal etme desteği.
* Eklenen SetCurrentLocation için form veya ConnectionPolicy tercih edilen konumlar bölgeye göre otomatik olarak doldurur.
* Çapraz bölüm Min/Maks ve sorgular tek bir bölüme belge eşleşen filtre hata düzeltildi.
* DocumentClient yöntemleri artık IDocumentClient ile eşlik vardır.
* Kurulan bağlantı sayısını azaltmak için güncelleştirilmiş doğrudan TCP taşıma yığını.
* Windows olmayan istemciler için eklenen destek doğrudan modu TCP için.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Eklenen isteği iptal etme desteği.
* Eklenen SetCurrentLocation için form veya ConnectionPolicy tercih edilen konumlar bölgeye göre otomatik olarak doldurur.
* Çapraz bölüm Min/Maks ve sorgular tek bir bölüme belge eşleşen filtre hata düzeltildi.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-Preview

* DocumentClient yöntemleri artık IDocumentClient ile eşlik vardır.
* Kurulan bağlantı sayısını azaltmak için güncelleştirilmiş doğrudan TCP taşıma yığını.
* Windows olmayan istemciler için eklenen destek doğrudan modu TCP için.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0

* FeedOptions için ConsistencyLevel özelliği eklendi.
* Eklenen JsonSerializerSettings RequestOptions ve FeedOptions.
* Eklenen EnableReadRequestsFallback ConnectionPolicy için.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1

* Çapraz bölüm sıralama ölçütü sorguları köşe durumlarda sabit KeyNotFoundException için.
* LINQ sorguları için select yan tümcesinde JsonProperty özniteliğinin kabul edildiği düzeltilmedi.

### <a name="a-name182182"></a><a name="1.8.2"/>1.8.2

* Belirli yarış koşullarında oluşan hata düzeltildi, bu durum aralıklı olarak "Microsoft. Azure. Documents. NotFoundException" ile sonuçlanır: Oturum tutarlılığı düzeyi kullanılırken, okuma oturumu, giriş oturumu belirtecinin hataları için kullanılamaz.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1

* Gerilemesi düzeltildi burada FeedOptions.MaxItemCount = -1 bir System.ArithmeticException oluşturdu: sayfa boyutu negatif ise.
* Yeni bir ToString() işlevini QueryMetrics için eklendi.
* Bölüm istatistikleri okuma koleksiyonlar üzerinde açık.
* ChangeFeedOptions için PartitionKey özelliği eklendi.
* Küçük hata düzeltmeleri.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
 
 * Belgeler için benzersiz dizinler hakkında DocumentCollection UniqueKeyPolicy özelliğini kullanarak belirtme olanağı ekler.
 * İçinde özel JsonSerializer ayarları bazı sorguları ve saklı yordam yürütme için kabul değil, bir hata düzeltildi.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
 
 * Azure Cosmos DB API Başvurusu'ndaki Azure documentdb'den bir değişiklik, belgeler, meta veri bilgilerini derlemelerde ve NuGet paketini marka.
 * Tanılama bilgileri ve gecikme süresi ile doğrudan bağlantı modunu gönderilen isteklerin yanıtından kullanıma sunar. Özellik adlarını, ResourceResponse sınıfı üzerinde RequestDiagnosticsString ve RequestLatency bulunur.
 * Bu SDK sürüm Merkezi'nden Azure Cosmos DB Emulator kullanılabilir en son sürümünü gerektirir. https://aka.ms/cosmosdb-emulator.
 
### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Birçok güvenilirlik düzeltmeleri ve geliştirmeleri eklendi.

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1 

* İç değişiklik ilgili destek [Microsoft.Azure.Graphs](https://docs.microsoft.com/azure/cosmos-db/graph-sdk-dotnet)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Sorgu Sonuçları belirli bir bölüm anahtar aralığı değeri için kapsam için bir FeedOption olarak PartitionKeyRangeId desteği eklendi.
* Bundan sonra değişikliklerin bakmaya başlamak için bir ChangeFeedOption olarak StartTime desteği eklendi.

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   Bir yığın taşması özel durumuna neden olabilir JsonSerializable sınıfında bir sorun düzeltildi.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Özel bir JsonSerializerSettings örneği oluşturulurken belirtme desteği eklendi bir [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) örneği.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   .NET Standard 1.5 hedef çerçeveleri biri olarak destekleme.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   X64 etkilenen bir sorun düzeltildi SSE4 yönerge desteği yoktur ve Azure Cosmos DB sorgu çalıştırırken SEHException throw makineleri.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Yeni bir tutarlılık düzeyi için destek eklendi ConsistentPrefix çağrılır.
*   Tek tek bölümler için sorgu ölçümler için destek eklendi.
*   Sorgular için devamlılık belirteci boyutunu sınırlamak için destek eklendi.
*   Başarısız istekler için ayrıntılı izleme desteği eklendi.
*   SDK'ın bazı performans iyileştirmeleri yapıldı.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Toplam sorgularında FeedOptions içinde sağlanan PartitionKey değeri yok sayıldı bir sorun düzeltildi.
* Bölümler arası sorgu yürütme Order By Orta uçuş sırasında bölüm yönetimi saydam işlemede bir sorun düzeltildi.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* Bazı zaman uyumsuz API'leri ASP.NET bağlam içinde kullanıldığında kilitlenmeleri neden bir sorun düzeltildi.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Belirli koşullar altında otomatik yük devretme karşı daha dayanıklı SDK yapmak için düzeltmeleri.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Bazen WebException neden olan bir sorun için çözüm: Uzak ad çözümlenemedi.
* Doğrudan ReadDocumentAsync API'sine yeni aşırı yüklemeler ekleyerek türü belirtilmiş bir belgeyi okumak için destek eklendi.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) için LINQ desteği eklendi.
* Olay işleyicisi kullanımından kaynaklanan ConnectionPolicy nesne için bir bellek sızıntısı sorunu düzeltildi.
* ETag kullanıldığında burada görüntülerle UpsertAttachmentAsync çalıştığı olmayan bir sorunu düzeltin.
* Burada görüntülerle sorgu devamlılığı sırası tarafından çapraz bölüm sıralarken dize alanı üzerinde çalıştığı değil bir sorun düzeltildi.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) için destek eklendi. Bkz: [toplama Destek](sql-query-aggregates.md).
* Bölümlenmiş koleksiyonlardan 10,100 RU/sn 2500 RU/sn için en düşük aktarım hızını düşürdü.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

Azure Cosmos DB .NET Core SDK'sı hızlı, platformlar arası oluşturmanıza olanak sağlayan [ASP.NET Core](https://www.asp.net/core) ve [.NET Core](https://www.microsoft.com/net/core#windows) uygulamaları Windows, Mac ve Linux üzerinde çalıştırılacak. Azure Cosmos DB .NET Core SDK'ın en son sürümünü tam olarak olan [Xamarin](https://www.xamarin.com) uyumlu ve iOS, Android ve Mono (Linux) hedefleyen uygulamalar oluşturmak için kullanılabilir.  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview

Azure Cosmos DB .NET Core Önizleme SDK'sı hızlı, platformlar arası oluşturmanıza olanak sağlayan [ASP.NET Core](https://www.asp.net/core) ve [.NET Core](https://www.microsoft.com/net/core#windows) uygulamaları Windows, Mac ve Linux üzerinde çalıştırılacak.

Azure Cosmos DB .NET Core Önizleme SDK'sı özellik eşliği ile en son sürümüne sahip [Azure Cosmos DB .NET SDK'sı](sql-api-sdk-dotnet.md) ve aşağıdakileri destekler:
* Tüm [bağlantı modları](performance-tips.md#networking): Ağ Geçidi modu, doğrudan TCP ve doğrudan HTTPs.
* Tüm [tutarlılık düzeyleri](consistency-levels.md): Güçlü, oturum, sınırlanmış Eskime durumu ve nihai.
* [Bölümlenmiş koleksiyonları](partition-data.md).
* [Çoklu bölge veritabanı hesapları ve coğrafi çoğaltma](distribute-data-globally.md).

Bu SDK ile ilgili sorularınız varsa, [StackOverflow](https://stackoverflow.com/questions/tagged/azure-documentdb)' a gönderin veya [GitHub deposunda](https://github.com/Azure/azure-documentdb-dotnet/issues)bir sorun yapın.

## <a name="release--retirement-dates"></a>Yayın ve sona erme tarihlerini

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [2.5.1](#2.5.1) |02 Temmuz 2019 |--- |
| [2.4.1](#2.4.1) |20 Haziran 2019 |--- |
| [2.4.0](#2.4.0) |05 Mayıs 2019 |--- |
| [2.3.0](#2.3.0) |04 Nisan 2019 |--- |
| [2.2.3](#2.2.3) |11 Mart 2019 |--- |
| [2.2.2](#2.2.2) |06 Şubat 2019 |--- |
| [2.2.1](#2.2.1) |24 Aralık 2018 |--- |
| [2.2.0](#2.2.0) |07 Aralık 2018 |--- |
| [2.1.3](#2.1.3) |15 Ekim 2018 |--- |
| [2.1.2'yi](#2.1.2) |04 Ekim 2018 |--- |
| [2.1.1](#2.1.1) |27 Eylül 2018'den |--- |
| [2.1.0](#2.1.0) |21 Eylül 2018 |--- |
| [2.0.0](#2.0.0) |07 Eylül 2018'den |--- |
| [1.9.1](#1.9.1) |09 Mart 2018 |--- |
| [1.8.2](#1.8.2) |21 Şubat 2018 |--- |
| [1.8.1](#1.8.1) |05 Şubat 2018 |--- |
| [1.7.1](#1.7.1) |16 Kasım 2017 |--- |
| [1.7.0](#1.7.0) |10 Kasım 2017 |--- |
| [1.6.0](#1.6.0) |17 Ekim 2017 |--- |
| [1.5.1](#1.5.1) |02 Ekim 2017 |--- |
| [1.5.0](#1.5.0) |10 Ağustos 2017 |--- | 
| [1.4.1](#1.4.1) |07 Ağustos 2017 |--- |
| [1.4.0](#1.4.0) |02 Ağustos 2017 |--- |
| [1.3.2](#1.3.2) |12 Haziran 2017 |--- |
| [1.3.1](#1.3.1) |23 Mayıs 2017 |--- |
| [1.3.0](#1.3.0) |10 Mayıs 2017 |--- |
| [1.2.2](#1.2.2) |19 Nisan 2017 |--- |
| [1.2.1](#1.2.1) |29 Mart 2017 |--- |
| [1.2.0](#1.2.0) |25 Mart 2017 |--- |
| [1.1.2](#1.1.2) |20 Mart 2017 |--- |
| [1.1.1](#1.1.1) |14 Mart 2017 |--- |
| [1.1.0](#1.1.0) |16 Şubat 2017 |--- |
| [1.0.0](#1.0.0) |21 aralık 2016 |--- |
| [0.1.0-Preview](#0.1.0-preview) |15 Kasım 2016 |31 Aralık 2016 |

## <a name="see-also"></a>Ayrıca Bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmeti sayfası.

