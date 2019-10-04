---
title: "Azure Cosmos DB: SQL .NET Core API 'SI, SDK & kaynakları"
description: Sürüm tarihleri, kullanımdan kaldırma tarihleri ve Azure Cosmos DB .NET Core SDK her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL .NET Core API 'SI ve SDK hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2018
ms.author: sngun
ms.openlocfilehash: 325e84ec4489eb2f1f530585af7d3c4bc4b201fc
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949563"
---
# <a name="azure-cosmos-db-net-core-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için Azure Cosmos DB .NET Core SDK: sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [.NET Standard](sql-api-sdk-dotnet-standard.md)
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik akışı](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Zaman uyumsuz Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST kaynak sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

| |  |
|---|---|
|**SDK indirmesi**| [NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.Core/)|
|**API belgeleri**|[.NET API başvuru belgeleri](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Örnekler**|[.NET kod örnekleri](sql-api-dotnet-samples.md)|
|**Kullanmaya başlama**|[Azure Cosmos DB .NET ile çalışmaya başlama](sql-api-sdk-dotnet.md)|
|**Web uygulaması öğreticisi**|[Azure Cosmos DB ile Web uygulaması geliştirme](sql-api-dotnet-application.md)|
|**Desteklenen geçerli çerçeve**|[.NET Standard 1,6 ve .NET Standard 1,5](https://www.nuget.org/packages/NETStandard.Library)|

## <a name="release-notes"></a>Release Notes (Sürüm Notları)

> [!NOTE]
> .NET Core kullanıyorsanız, lütfen .NET Standard hedefleyen [.NET SDK 'sının](sql-api-sdk-dotnet-standard.md)en son sürüm 3. x sürümünü inceleyin. 

### <a name="a-name270270"></a><a name="2.7.0"/>2.7.0

* Sorgular tarafından sırasıyla diziler ve nesneler için destek eklendi
* Etkili bölüm anahtarı çakışmalarını işle
* ThenBy işleciyle birden çok OrderBy işleci için LINQ desteği eklendi
* Özel serileştirme ayarları artık tüm upsert ve değiştirme işlemlerine uygulandı
* Tek iş parçacıklı bir Görev Zamanlayıcısı ile çalışabilmesi için sabit bir bellek kilitlenme sorunu düzeltildi

### <a name="a-name260260"></a><a name="2.6.0"/>2.6.0

* ConnectionPolicy 'e PortReusePolicy eklendi
* Ntdll! düzeltildi! Bir UWP uygulamasında SDK kullanıldığında RtlGetVersion TypeLoadException sorunu

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

* System .net. Security, 4.3.2 ile güncelleştirildi.

### <a name="a-name212212"></a><a name="2.1.2"/>2.1.2

* Tanılama izleme iyileştirmeleri.

### <a name="a-name211211"></a><a name="2.1.1"/>2.1.1

* Çok bölgeli istek geçici hatalarıyla daha fazla esnekliği eklendi.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0

* Çok bölgeli yazma desteği eklendi.
* ÜST ve MaxBufferedItemCount ile çapraz bölüm sorgu performansı iyileştirmeleri.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0

* İstek iptali desteği eklendi.
* Bölgeye göre tercih edilen konumları otomatik olarak dolduran ConnectionPolicy öğesine SetCurrentLocation eklendi.
* Minimum/Max ve tek bir bölümdeki hiçbir belge olmadan eşleşen bir filtre içeren çapraz bölüm sorgularında hata düzeltildi.
* DocumentClient yöntemlerinde artık ıdocumentclient ile eşlik vardır.
* Kurulan bağlantı sayısını azaltmak için doğrudan TCP Aktarım yığını güncelleştirildi.
* Windows olmayan istemciler için doğrudan mod TCP desteği eklendi.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* İstek iptali desteği eklendi.
* Bölgeye göre tercih edilen konumları otomatik olarak dolduran ConnectionPolicy öğesine SetCurrentLocation eklendi.
* Minimum/Max ve tek bir bölümdeki hiçbir belge olmadan eşleşen bir filtre içeren çapraz bölüm sorgularında hata düzeltildi.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-Preview

* DocumentClient yöntemlerinde artık ıdocumentclient ile eşlik vardır.
* Kurulan bağlantı sayısını azaltmak için doğrudan TCP Aktarım yığını güncelleştirildi.
* Windows olmayan istemciler için doğrudan mod TCP desteği eklendi.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0

* Feest Level özelliği, Feedoseçenekleri 'ne eklendi.
* RequestOptions ve Feedoseçenekleri 'ne JsonSerializerSettings eklendi.
* ConnectionPolicy 'e EnableReadRequestsFallback eklendi.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1

* Köşe durumlarında, çapraz bölüm sıralaması için sabit KeyNotFoundException.
* LINQ sorguları için select yan tümcesinde JsonProperty özniteliğinin kabul edildiği düzeltilmedi.

### <a name="a-name182182"></a><a name="1.8.2"/>1.8.2

* Belirli yarış koşullarında oluşan düzeltilen hata, bu durum aralıklı olarak "Microsoft. Azure. Documents. NotFoundException: oturum tutarlılığı düzeyi kullanılırken okuma oturumu, giriş oturum belirteci için kullanılamıyor" hataları ile sonuçlanır.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1

* Feedotik. Maxıtemcount =-1 bir System. ArithmeticException tarafından oluşturulduğu ve sayfa boyutu negatif olan gerileme düzeltildi.
* Queryölçümler 'e yeni bir ToString () işlevi eklendi.
* Koleksiyonları okurken sunulan Bölüm istatistikleri.
* Changefeedoseçenekler 'e PartitionKey özelliği eklendi.
* Küçük hata düzeltmeleri.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
 
 * DocumentCollection üzerinde UniqueKeyPolicy özelliğini kullanarak belgeler için benzersiz dizinler belirtme yeteneği ekler.
 * Bazı sorgular ve saklı yordam yürütmesi için özel JsonSerializer ayarlarının kabul edildiği bir hata düzeltildi.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
 
 * Azure DocumentDB 'den, API başvuru belgeleri, derlemelerdeki meta veri bilgileri ve NuGet paketindeki Azure Cosmos DB için marka değişikliği.
 * Doğrudan bağlantı moduyla gönderilen isteklerin yanıtından tanılama bilgilerini ve gecikme süresini kullanıma sunun. Özellik adları, Resourdiagnosticsstring ve RequestLatency for Resourceresbir Class.
 * Bu SDK sürümü, Azure Cosmos DB öykünücüsü 'nin https://aka.ms/cosmosdb-emulator ' dan indirileceği en son sürümünü gerektirir.
 
### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0

* Çeşitli güvenilirlik düzeltmeleri ve iyileştirmeler eklendi.

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1 

* [Microsoft. Azure. Graf](https://docs.microsoft.com/azure/cosmos-db/graph-sdk-dotnet) desteği ile ilgili dahili değişiklikler

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0 

* Belirli bir bölüm anahtar aralığı değerine sorgu sonuçlarının kapsamını belirlemek için bir FeedOption olarak Partitionkeyrangeıd desteği eklendi.
* Bu süreden sonra değişiklikleri aramaya başlamak için bir ChangeFeedOption olarak StartTime desteği eklendi.

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1

*   JsonSerializable sınıfında yığın taşması özel durumuna neden olabilecek bir sorun düzeltildi.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0

*   Bir [Documentclient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet) örneği örneği oluşturulurken özel JsonSerializerSettings belirtilmesine yönelik destek eklendi.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2

*   Hedef çerçevelerinden biri olarak .NET Standard 1,5 ' i destekleme.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1

*   SSE4 yönergesini desteklemeyen ve Azure Cosmos DB sorguları çalıştırırken bir şehir özel durumu oluşturan x64 makinelerin etkilendiğine ilişkin bir sorun düzeltildi.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0

*   Tutarlık ön eki olarak adlandırılan yeni bir tutarlılık düzeyi için destek eklendi.
*   Ayrı bölümlere yönelik sorgu ölçümleri için destek eklendi.
*   Sorgular için devamlılık belirtecinin boyutunu sınırlandırmaya yönelik destek eklendi.
*   Başarısız isteklere yönelik daha ayrıntılı izleme için destek eklendi.
*   SDK 'da bazı performans geliştirmeleri yapılmıştır.

### <a name="a-name122122"></a><a name="1.2.2"/>1.2.2

* Toplama sorguları için Feedotik değerleri içinde belirtilen PartitionKey değerini yoksayan bir sorun düzeltildi.
* Sorgu yürütmeye göre orta kol çapraz bölüm sıralaması sırasında bölüm yönetiminin saydam işlenmesinde sorun düzeltildi.

### <a name="a-name121121"></a><a name="1.2.1"/>1.2.1

* ASP.NET bağlamında kullanıldığında zaman uyumsuz API 'lerde kilitlenmeleri neden olan bir sorun düzeltildi.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0

* Belirli koşullar altında SDK 'Yı otomatik yük devretmeye daha dayanıklı hale getirmek için düzeltmeler.

### <a name="a-name112112"></a><a name="1.1.2"/>1.1.2

* Her zaman bir WebException neden olan bir sorun için çözüm: uzak ad çözümlenemedi.
* Readbelgesiyle Tasync API 'sine yeni aşırı yüklemeler ekleyerek, yazılı bir belgeyi doğrudan okuma desteği eklendi.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1

* Toplama sorguları için LINQ desteği eklendi (COUNT, MIN, MAX, SUM ve AVG).
* Olay işleyicisinin kullanılması nedeniyle ConnectionPolicy nesnesi için bellek sızıntısı sorunu için çözüm.
* ETag kullanılırken UpsertAttachmentAsync 'nin çalışmamasıyla ilgili bir sorunu giderme.
* Dize alanına göre sıralama yapılırken, çapraz bölüm sırası ile sorgu devamlılığını devam ettirme sorunu için çözüm çalışmıyor.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0

* Toplama sorguları (COUNT, MIN, MAX, SUM ve AVG) için destek eklendi. Bkz. [toplama desteği](sql-query-aggregates.md).
* 10.100 RU/sn 'den 2500 RU/sn 'ye kadar bölümlenmiş koleksiyonlar üzerinde en düşük aktarım hızını düşürdü.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0

Azure Cosmos DB .NET Core SDK, Windows, Mac ve Linux 'ta çalışacak hızlı, platformlar arası [ASP.NET Core](https://www.asp.net/core) ve [.NET Core](https://www.microsoft.com/net/core#windows) uygulamaları oluşturmanıza olanak sağlar. Azure Cosmos DB .NET Core SDK en son sürümü tamamen [Xamarin](https://www.xamarin.com) uyumludur ve IOS, Android ve Mono (Linux) hedefleyen uygulamalar oluşturmak için kullanılır.  

### <a name="a-name010-preview010-preview"></a><a name="0.1.0-preview"/>0.1.0-Preview

Azure Cosmos DB .NET Core Preview SDK 'Sı, Windows, Mac ve Linux 'ta çalışacak hızlı, platformlar arası [ASP.NET Core](https://www.asp.net/core) ve [.NET Core](https://www.microsoft.com/net/core#windows) uygulamaları oluşturmanıza olanak sağlar.

Azure Cosmos DB .NET Core Preview SDK 'sının, [Azure Cosmos DB .NET SDK 'sının](sql-api-sdk-dotnet.md) en son sürümü ile özellik eşliği vardır ve şunları destekler:
* Tüm [bağlantı modları](performance-tips.md#networking): ağ geçidi modu, doğrudan TCP ve doğrudan https.
* Tüm [tutarlılık düzeyleri](consistency-levels.md): güçlü, oturum, sınırlanmış Eskime durumu ve nihai.
* [Bölümlenmiş koleksiyonlar](partition-data.md).
* [Çok bölgeli veritabanı hesapları ve coğrafi çoğaltma](distribute-data-globally.md).

Bu SDK ile ilgili sorularınız varsa, [StackOverflow](https://stackoverflow.com/questions/tagged/azure-documentdb)' a gönderin veya [GitHub deposunda](https://github.com/Azure/azure-documentdb-dotnet/issues)bir sorun yapın.

## <a name="release--retirement-dates"></a>Yayın & kullanımdan kaldırma tarihleri
Microsoft, daha yeni/desteklenen bir sürüme geçişi düzgünleştirmek için SDK 'nın devre dışı bırakılmasının ardından en az **12 ay** önce bildirim sağlar.

Yeni özellikler ve işlevler ve iyileştirmeler yalnızca geçerli SDK 'ya eklenir, bu nedenle en son SDK sürümüne her zaman olabildiğince erken yükseltmeniz önerilir. 

Kullanımdan kaldırılan bir SDK kullanarak Azure Cosmos DB yönelik istekler hizmet tarafından reddedilir.

> [!WARNING]
> SQL API için .NET Core SDK tüm **1. x** sürümleri **30 Ağustos 2020**tarihinde kullanımdan kaldırılacaktır.
> 
>
<br/>


| Version | Yayın Tarihi | Emeklilik tarihi |
| --- | --- | --- |
| [2.7.0](#2.7.0) |23 Eylül 2019 |--- |
| [2.6.0](#2.6.0) |30 Ağustos 2019 |--- |
| [2.5.1](#2.5.1) |02 Temmuz 2019 |--- |
| [2.4.1](#2.4.1) |20 Haziran 2019 |--- |
| [2.4.0](#2.4.0) |05 Mayıs 2019 |--- |
| [2.3.0](#2.3.0) |04 Nisan 2019 |--- |
| [2.2.3](#2.2.3) |11 Mart 2019 |--- |
| [2.2.2](#2.2.2) |06 Şubat 2019 |--- |
| [2.2.1](#2.2.1) |24 Aralık 2018 |--- |
| [2.2.0](#2.2.0) |07 Aralık 2018 |--- |
| [2.1.3](#2.1.3) |15 Ekim 2018 |--- |
| [2.1.2 'yi](#2.1.2) |04 Ekim 2018 |--- |
| [2.1.1](#2.1.1) |27 Eylül 2018 |--- |
| [2.1.0](#2.1.0) |21 Eylül 2018 |--- |
| [2.0.0](#2.0.0) |Eylül 07, 2018 |--- |
| [1.9.1](#1.9.1) |09 Mart 2018 |30 Ağustos 2020 |
| [1.8.2](#1.8.2) |21 Şubat 2018 |30 Ağustos 2020 |
| [1.8.1](#1.8.1) |05 Şubat 2018 |30 Ağustos 2020 |
| [1.7.1](#1.7.1) |16 Kasım 2017 |30 Ağustos 2020 |
| [1.7.0](#1.7.0) |10 Kasım 2017 |30 Ağustos 2020 |
| [1.6.0](#1.6.0) |17 Ekim 2017 |30 Ağustos 2020 |
| [1.5.1](#1.5.1) |02 Ekim 2017 |30 Ağustos 2020 |
| [1.5.0](#1.5.0) |10 Ağustos 2017 |30 Ağustos 2020 | 
| [1.4.1](#1.4.1) |07 Ağustos 2017 |30 Ağustos 2020 |
| [1.4.0](#1.4.0) |02 Ağustos 2017 |30 Ağustos 2020 |
| [1.3.2](#1.3.2) |12 Haziran 2017 |30 Ağustos 2020 |
| [1.3.1](#1.3.1) |23 Mayıs 2017 |30 Ağustos 2020 |
| [1.3.0](#1.3.0) |10 Mayıs 2017 |30 Ağustos 2020 |
| [1.2.2](#1.2.2) |19 Nisan 2017 |30 Ağustos 2020 |
| [1.2.1'i](#1.2.1) |29 Mart 2017 |30 Ağustos 2020 |
| [1.2.0](#1.2.0) |25 Mart 2017 |30 Ağustos 2020 |
| [1.1.2](#1.1.2) |20 Mart 2017 |30 Ağustos 2020 |
| [1.1.1](#1.1.1) |14 Mart 2017 |30 Ağustos 2020 |
| [1.1.0](#1.1.0) |16 Şubat 2017 |30 Ağustos 2020 |
| [1.0.0](#1.0.0) |21 Aralık 2016 |30 Ağustos 2020 |
| [0.1.0-Önizleme](#0.1.0-preview) |15 Kasım 2016 |31 Aralık 2016 |

## <a name="see-also"></a>Ayrıca Bkz.
Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.

