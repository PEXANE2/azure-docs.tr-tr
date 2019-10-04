---
title: 'Azure Cosmos DB: SQL .NET API, SDK & kaynakları'
description: Sürüm tarihleri, kullanımdan kaldırma tarihleri ve Azure Cosmos DB .NET SDK 'nın her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL .NET API 'SI ve SDK hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/09/2018
ms.author: sngun
ms.openlocfilehash: f8c38c46bd60834c166721f62088d8edb2c722a9
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2019
ms.locfileid: "71949588"
---
# <a name="azure-cosmos-db-net-sdk-for-sql-api-download-and-release-notes"></a>SQL API için Azure Cosmos DB .NET SDK: Indirme ve sürüm notları
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET](sql-api-sdk-dotnet-standard.md)
> * [.NET değişiklik akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
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
|**SDK indirmesi**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)|
|**API belgeleri**|[.NET API başvuru belgeleri](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Örnekler**|[.NET kod örnekleri](https://github.com/Azure/azure-cosmos-dotnet-v2/tree/master/samples)|
|**Kullanmaya başlama**|[Azure Cosmos DB .NET SDK ile çalışmaya başlama](sql-api-get-started.md)|
|**Web uygulaması öğreticisi**|[Azure Cosmos DB ile Web uygulaması geliştirme](sql-api-dotnet-application.md)|
|**Desteklenen geçerli çerçeve**|[Microsoft .NET Framework 4,5](https://www.microsoft.com/download/details.aspx?id=30653)|

## <a name="release-notes"></a>Sürüm notları

> [!NOTE]
> .NET Framework kullanıyorsanız, lütfen .NET Standard hedefleyen [.NET SDK 'sının](sql-api-sdk-dotnet-standard.md)en son sürüm 3. x sürümünü inceleyin. 

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

* Tanılama izleme iyileştirmeleri

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

### <a name="a-name12201220"></a><a name="1.22.0"/>1.22.0

* Feest Level özelliği, Feedoseçenekleri 'ne eklendi.
* RequestOptions ve Feedoseçenekleri 'ne JsonSerializerSettings eklendi.
* ConnectionPolicy 'e EnableReadRequestsFallback eklendi.

### <a name="a-name12111211"></a><a name="1.21.1"/>1.21.1

* Köşe durumlarında, çapraz bölüm sıralaması için sabit KeyNotFoundException.
* LINQ sorguları için select yan tümcesinde JsonProperty özniteliğinin kabul edildiği düzeltilmedi.

### <a name="a-name12021202"></a><a name="1.20.2"/>1.20.2

* Belirli yarış koşullarında oluşan düzeltilen hata, bu durum aralıklı olarak "Microsoft. Azure. Documents. NotFoundException: oturum tutarlılığı düzeyi kullanılırken okuma oturumu, giriş oturum belirteci için kullanılamıyor" hataları ile sonuçlanır.

### <a name="a-name12011201"></a><a name="1.20.1"/>1.20.1

* Feedotik. Maxıtemcount =-1 bir System. ArithmeticException tarafından oluşturulduğu ve sayfa boyutu negatif olan gerileme düzeltildi.
* Queryölçümler 'e yeni bir ToString () işlevi eklendi.
* Koleksiyonları okurken sunulan Bölüm istatistikleri.
* Changefeedoseçenekler 'e PartitionKey özelliği eklendi.
* Küçük hata düzeltmeleri.

### <a name="a-name11911191"></a><a name="1.19.1"/>1.19.1

* DocumentCollection üzerinde UniqueKeyPolicy özelliğini kullanarak belgeler için benzersiz dizinler belirtme yeteneği ekler.
* Bazı sorgular ve saklı yordam yürütmesi için özel JsonSerializer ayarlarının kabul edildiği bir hata düzeltildi.

### <a name="a-name11901190"></a><a name="1.19.0"/>1.19.0

* Azure DocumentDB 'den, API başvuru belgeleri, derlemelerdeki meta veri bilgileri ve NuGet paketindeki Azure Cosmos DB için marka değişikliği. 
* Doğrudan bağlantı moduyla gönderilen isteklerin yanıtından tanılama bilgilerini ve gecikme süresini kullanıma sunun. Özellik adları, Resourdiagnosticsstring ve RequestLatency for Resourceresbir Class.
* Bu SDK sürümü, Azure Cosmos DB öykünücüsü 'nin https://aka.ms/cosmosdb-emulator ' dan indirileceği en son sürümünü gerektirir. 

### <a name="a-name11811181"></a><a name="1.18.1"/>1.18.1 

* Microsoft arkadaş Derlemeleriyle ilgili dahili değişiklikler.

### <a name="a-name11801180"></a><a name="1.18.0"/>1.18.0 

* Çeşitli güvenilirlik düzeltmeleri ve iyileştirmeler eklendi.

### <a name="a-name11701170"></a><a name="1.17.0"/>1.17.0 

* Belirli bir bölüm anahtar aralığı değerine sorgu sonuçlarının kapsamını belirlemek için bir FeedOption olarak Partitionkeyrangeıd desteği eklendi. 
* Bu süreden sonra değişiklikleri aramaya başlamak için bir ChangeFeedOption olarak StartTime desteği eklendi.

### <a name="a-name11611161"></a><a name="1.16.1"/>1.16.1
* JsonSerializable sınıfında yığın taşması özel durumuna neden olabilecek bir sorun düzeltildi.

### <a name="a-name11601160"></a><a name="1.16.0"/>1.16.0
*   JsonSerializerSettings 'in DocumentClient oluşturucusunda isteğe bağlı bir parametre olarak eklenmesi nedeniyle uygulamanın yeniden derlenmesi için gereken bir sorun düzeltildi.
* DocumentClient Oluşturucusu, jsonserializersettings parametresinde geçiş yaparken varsayılan ConnectionPolicy ve en fazla bağlantı parametrelerine izin veren son parametre olarak JsonSerializerSettings 'i zorunlu olarak işaretledi.

### <a name="a-name11501150"></a><a name="1.15.0"/>1.15.0
*   [Documentclient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet)örneği oluşturulurken özel JsonSerializerSettings belirtilmesine yönelik destek eklendi.

### <a name="a-name11411141"></a><a name="1.14.1"/>1.14.1
*   SSE4 yönergesini desteklemeyen ve Azure Cosmos DB SQL sorguları çalıştırırken bir şehir özel durumu oluşturan x64 makinelerin etkilendiğine ilişkin bir sorun düzeltildi.

### <a name="a-name11401140"></a><a name="1.14.0"/>1.14.0
*   Tutarlık ön eki olarak adlandırılan yeni bir tutarlılık düzeyi için destek eklendi.
*   Ayrı bölümlere yönelik sorgu ölçümleri için destek eklendi.
*   Sorgular için devamlılık belirtecinin boyutunu sınırlandırmaya yönelik destek eklendi.
*   Başarısız isteklere yönelik daha ayrıntılı izleme için destek eklendi.
*   SDK 'da bazı performans geliştirmeleri yapılmıştır.

### <a name="a-name11341134"></a><a name="1.13.4"/>1.13.4
* 1\.13.3 ile aynı işlev. Bazı iç değişiklikler yapıldı.

### <a name="a-name11331133"></a><a name="1.13.3"/>1.13.3
* 1\.13.2 ile aynı işlev. Bazı iç değişiklikler yapıldı.

### <a name="a-name11321132"></a><a name="1.13.2"/>1.13.2
* Toplama sorguları için Feedotik değerleri içinde belirtilen PartitionKey değerini yoksayan bir sorun düzeltildi.
* Sorgu yürütmeye göre orta kol çapraz bölüm sıralaması sırasında bölüm yönetiminin saydam işlenmesinde sorun düzeltildi.

### <a name="a-name11311131"></a><a name="1.13.1"/>1.13.1
* ASP.NET bağlamında kullanıldığında zaman uyumsuz API 'lerde kilitlenmeleri neden olan bir sorun düzeltildi.

### <a name="a-name11301130"></a><a name="1.13.0"/>1.13.0
* Belirli koşullar altında SDK 'Yı otomatik yük devretmeye daha dayanıklı hale getirmek için düzeltmeler.

### <a name="a-name11221122"></a><a name="1.12.2"/>1.12.2
* Her zaman bir WebException neden olan bir sorun için çözüm: uzak ad çözümlenemedi.
* Readbelgesiyle Tasync API 'sine yeni aşırı yüklemeler ekleyerek, yazılı bir belgeyi doğrudan okuma desteği eklendi.

### <a name="a-name11211121"></a><a name="1.12.1"/>1.12.1
* Toplama sorguları için LINQ desteği eklendi (COUNT, MIN, MAX, SUM ve AVG).
* Olay işleyicisinin kullanılması nedeniyle ConnectionPolicy nesnesi için bellek sızıntısı sorunu için çözüm.
* ETag kullanılırken UpsertAttachmentAsync 'nin çalışmamasıyla ilgili bir sorunu giderme.
* Dize alanına göre sıralama yapılırken, çapraz bölüm sırası ile sorgu devamlılığını devam ettirme sorunu için çözüm çalışmıyor.

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Toplama sorguları (COUNT, MIN, MAX, SUM ve AVG) için destek eklendi. Bkz. [toplama desteği](sql-query-aggregates.md).
* 10.100 RU/sn 'den 2500 RU/sn 'ye kadar bölümlenmiş koleksiyonlar üzerinde en düşük aktarım hızını düşürdü.

### <a name="a-name11141114"></a><a name="1.11.4"/>1.11.4
* 32 bit ana bilgisayar işleminde bazı çapraz bölüm sorgularının başarısız olduğu bir sorunu giderme.
* Oturum kapsayıcısının Ağ Geçidi modundaki başarısız isteklerin belirteciyle güncelleştirilmemiş bir sorun için çözüm.
* Bazı durumlarda projeksiyondaki UDF çağrılarına sahip bir sorgunun başarısız olduğu bir sorun için çözüm.
* İsteklerin okuma ve yazma verimini artırmak için istemci tarafı performans düzeltmeleri.

### <a name="a-name11131113"></a><a name="1.11.3"/>1.11.3
* Oturum kapsayıcısının başarısız istekler için belirteçle güncelleştirilmemiş bir sorun için çözüm.
* SDK 'nın 32 bitlik bir ana bilgisayar işleminde çalışması için destek eklendi. Çapraz bölüm sorguları kullanıyorsanız, gelişmiş performans için 64 bit ana bilgisayar işlemenin önerildiğini unutmayın.
* Bir ın ifadesinde çok sayıda bölüm anahtarı değeri olan sorgular içeren senaryolar için iyileştirilmiş performans.
* Populatequotaınfo istek seçeneği ayarlandığında belge koleksiyonu okuma istekleri için Resourceresıstadaki çeşitli kaynak kotası istatistikleri dolduruldu.

### <a name="a-name11111111"></a><a name="1.11.1"/>1.11.1
* 1\.11.0 ' de tanıtılan CreateDocumentCollectionIfNotExistsAsync API 'sinin düşük performans düzeltmesini.
* Yüksek derecede eşzamanlı istek içeren senaryolar için SDK 'da performans onarımı.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Bir koleksiyon içindeki belgelerin [değişiklik akışını](change-feed.md) işlemeye yönelik yeni sınıflar ve yöntemler için destek.
* Bölümler arası sorgu devamlılığını ve çapraz bölüm sorguları için bazı performans iyileştirmeleri için destek.
* CreateDatabaseIfNotExistsAsync ve CreateDocumentCollectionIfNotExistsAsync yöntemlerinin eklenmesi.
* Sistem işlevleri için LINQ desteği: IsDefined, IsNull ve ısilkel.
* NuGet paketini Project. JSON araçları olan projelerle birlikte kullanırken, Microsoft. Azure. Documents. Serviceınterop. dll ve DocumentDB. uzamsal. Sql. dll derlemelerinin uygulamanın bin klasörüne otomatik olarak yerleştirilmesi için çözüm yapın.
* Hata ayıklama senaryolarında yararlı olabilecek istemci tarafı ETW izlemelerini yayma desteği.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Bölümlenmiş koleksiyonlar için doğrudan bağlantı desteği eklendi.
* Sınırlanmış Stalet tutarlılığı düzeyi için iyileştirilmiş performans.
* Coğrafi olarak uzamsal sorgular için koleksiyon dizin oluşturma ilkesi belirtilirken Çokgen ve LineString veri türleri eklendi.
* Koşullar çevrilirken StringEnumConverter, IsoDateTimeConverter ve UnixDateTimeConverter için LINQ desteği eklendi.
* Çeşitli SDK hata düzeltmeleri.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Şu NotFoundException neden olan bir sorun düzeltildi: giriş oturum belirteci için oku oturumu kullanılamıyor. Bu özel durum, coğrafi olarak dağıtılmış bir hesabın okuma bölgesi sorgulanırken bazı durumlarda meydana geldi.
* Bir yanıttan temeldeki akışa doğrudan erişim sağlayan Resourceresıse sınıfında ResponseStream özelliği kullanıma sunuldu.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Resourceresrar, FeedResponse, StoredProcedureResponse ve Mediaresrarclasses ' ı, test odaklı dağıtım (TDD) için mocize erişebilmeleri için karşılık gelen ortak arabirimi uygulayacak şekilde değiştirdi.
* Verileri seri hale getirmek için özel bir JsonSerializerSettings nesnesi kullanılırken hatalı biçimlendirilmiş bölüm anahtarı başlığına neden olan bir sorun düzeltildi.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Uzun süre çalışan sorguların hata vererek başarısız olmasına neden olan bir sorun düzeltildi: geçerli zamanda yetkilendirme belirteci geçerli değil.
* Özgün SqlParameterCollection 'ı, kesit üst/sıra sorgularının içinden kaldıran bir sorun düzeltildi.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Bölümlenmiş koleksiyonlar için paralel sorgular için destek eklendi.
* Bölümler arası SıRALAMA için ve bölümlenmiş koleksiyonlar için üst sorgulara yönelik destek eklendi.
* Azure Cosmos DB NuGet paketine yönelik bir başvuruya sahip bir Azure Cosmos DB projesine başvururken gerekli olan DocumentDB. uzamsal. Sql. dll ve Microsoft. Azure. Documents. Serviceınterop. dll için eksik başvurular düzeltildi.
* LINQ içinde Kullanıcı tanımlı işlevler kullanılırken farklı türlerin parametrelerini kullanma özelliği düzeltildi. 
* Dünya dışı çağrıların yazma konumları yerine okuma konumlarına yönlendirildiği küresel olarak çoğaltılan hesaplar için bir hata düzeltildi.
* Eksik olan ıdocumentclient arabirimine Yöntemler eklendi: 
  * MediaStream ve seçenekleri parametre olarak alan UpsertAttachmentAsync yöntemi
  * Parametre olarak seçenekleri alan CreateAttachmentAsync yöntemi
  * QuerySpec 'i parametre olarak alan CreateOfferQuery yöntemi.
* Idocumentclient arabiriminde kullanıma sunulan korumasız ortak sınıflar.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Çok bölgeli veritabanı hesapları için destek eklendi.
* Kısıtlanmış isteklerde yeniden deneme desteği eklendi.  Kullanıcı, ConnectionPolicy. RetryOptions özelliğini yapılandırarak yeniden deneme sayısını ve en uzun bekleme süresini özelleştirebilir.
* Tüm DocumentClient özelliklerinin ve yöntemlerinin imzalarını tanımlayan yeni bir ıdocumentclient arabirimi eklendi.  Bu değişikliğin bir parçası olarak, IQueryable ve IOrderedQueryable oluşturan genişletme yöntemleri de DocumentClient sınıfının yöntemlerine değişti.
* Belirli bir Azure Cosmos DB uç noktası URI 'Si için ServicePoint. ConnectionLimit ayarlamak üzere yapılandırma seçeneği eklendi.  50 olarak ayarlanan varsayılan değeri değiştirmek için ConnectionPolicy. MaxConnectionLimit komutunu kullanın.
* Kullanım dışı bırakılmış IPartitionResolver ve uygulaması.  IPartitionResolver desteği artık kullanılmıyor. Daha yüksek depolama ve aktarım hızı için bölümlenmiş koleksiyonlar kullanmanız önerilir.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Bir parametre olarak RequestOptions alan URI tabanlı ExecuteStoredProcedureAsync metoduna aşırı yükleme eklendi.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Belgeler için yaşam süresi (TTL) desteği eklendi.

### <a name="a-name163163"></a><a name="1.6.3"/>1.6.3
* Azure bulut hizmeti çözümünün bir parçası olarak paketlenmesi için .NET SDK 'nın NuGet paketinizdeki bir hata düzeltildi.

### <a name="a-name162162"></a><a name="1.6.2"/>1.6.2
* [Bölümlenmiş koleksiyonlar](partition-data.md) ve [Kullanıcı tanımlı performans düzeyleri](performance-levels.md)uygulandı. 

### <a name="a-name153153"></a><a name="1.5.3"/>1.5.3
* **[Düzeltildi]** Azure Cosmos DB bitiş noktasını sorgulama: ' System .net. http. HttpRequestException: içerik bir akışa kopyalanırken hata oluştu '.

### <a name="a-name152152"></a><a name="1.5.2"/>1.5.2
* Sayfalama, Koşullu ifadeler ve Aralık karşılaştırması için yeni işleçler dahil olmak üzere genişletilmiş LINQ desteği.
  * LINQ 'te en ıyı davranışı seçme özelliğini etkinleştirmek için işleç al
  * Dize aralığı karşılaştırmaları etkinleştirmek için CompareTo işleci
  * Koşullu (?) ve birleşim işleçleri (??)
* **[Düzeltildi]** Bir LINQ sorgusunda model projeksiyonunu birleştiren ArgumentOutOfRangeException. [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* **[Düzeltildi]** Select, son ifade değilse, LINQ sağlayıcısı projeksiyonu kabul etmez ve SELECT * yanlış seçeneğini belirleyin.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Uygulanan upsert, eklenen UpsertXXXAsync yöntemleri
* Tüm istekler için performans iyileştirmeleri
* Dizeler için koşullu, birleşim ve CompareTo yöntemlerine yönelik LINQ sağlayıcı desteği
* **[Düzeltildi]** LINQ sağlayıcısı--> IEnumerable ve Array üzerinde aynı SQL ile oluşturmak için listedeki yöntemi Içerir
* **[Düzeltildi]** BackoffRetryUtility yeniden deneme sırasında yeni bir tane oluşturmak yerine aynı HttpRequestMessage 'i kullanır
* **[Geçersiz]** UriFactory. CreateCollection--> Şimdi UriFactory. CreateDocumentCollection kullanması gerekir

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1
* **[Düzeltildi]** NL-NL vb. gibi kültür olmayan bilgiler kullanılırken yerelleştirme sorunları. 

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* KIMLIK tabanlı yönlendirme eklendi
  * KIMLIK tabanlı kaynak bağlantıları oluşturmayla ilgili yardım almak için yeni UriFactory Yardımcısı
  * DocumentClient 'ta URI içinde gerçekleştirilecek yeni aşırı yüklemeler
* Jeo-uzamsal için LINQ içinde IsValid () ve ıvaliddetailed () eklendi
* LINQ sağlayıcısı desteği geliştirildi:
  * **Math** -ABS, acos, asin, atan, tavan, cos, exp, Floor, log, log10, Pow, yuvarlak, Sign, sin, sqrt, tan, Truncate
  * **Dize** -Concat, Contains, EndsWith, IndexOf, Count, ToLower, kırma, Replace, Reverse, TrimEnd, StartsWith, SUBSTRING, ToUpper
  * **Dizi** -Concat, Contains, Count
  * **In** işleci

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Dizin oluşturma ilkelerini değiştirme desteği eklendi.
  * DocumentClient 'da yeni ReplaceDocumentCollectionAsync yöntemi
  * Resourcereslation@ no__t-0T içinde yeni ındexdönüşümtionprogress özelliği, Dizin İlkesi değişikliklerinin yüzde ilerlemesini izlemek için >
  * DocumentCollection. ındexingpolicy artık değişebilir
* Uzamsal dizin oluşturma ve sorgu için destek eklendi.
  * Nokta ve çokgen gibi uzamsal türleri serileştirmek/seri durumdan çıkarmak için yeni Microsoft. Azure. Documents. uzamsal ad alanı
  * Cosmos DB depolanan GeoJSON verilerinin dizinini oluşturmak için yeni Spatialındex sınıfı
* **[Düzeltildi]** Bir LINQ ifadesinden [#38](https://github.com/Azure/azure-documentdb-net/issues/38)yanlış SQL sorgusu oluşturuldu.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Newtonsoft. JSON v 5.0.7 üzerine bir bağımlılık ekledi.
* Aşağıdakileri yaparak sıralamayı desteklemek için değişiklikler yapıldı:
  
  * OrderBy () veya OrderByDescending () için LINQ sağlayıcı desteği
  * Sıralamayı desteklemek için ındexingpolicy 
    
    **Olası Son değişiklik** 
    
    Özel bir dizin oluşturma ilkesiyle koleksiyonlar sağlayan mevcut kodunuz varsa, yeni ındexingpolicy sınıfını desteklemek için mevcut kodunuzun güncelleştirilmesi gerekir. Özel bir dizin oluşturma ilkeniz yoksa, bu değişiklik sizi etkilemez.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Yeni HashPartitionResolver ve RangePartitionResolver sınıfları ve IPartitionResolver kullanılarak bölümlendirme verileri için destek eklendi.
* DataContract serileştirme eklendi.
* LINQ sağlayıcısında GUID desteği eklendi.
* LINQ 'te UDF desteği eklendi.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK

## <a name="release--retirement-dates"></a>Yayın & kullanımdan kaldırma tarihleri
Microsoft, daha yeni/desteklenen bir sürüme geçişi düzgünleştirmek için SDK 'nın devre dışı bırakılmasının ardından en az **12 ay** önce bildirim sağlar.

Yeni özellikler ve işlevler ve iyileştirmeler yalnızca geçerli SDK 'ya eklenir, bu nedenle en son SDK sürümüne her zaman olabildiğince erken yükseltmeniz önerilir. 

Kullanımdan kaldırılan bir SDK kullanarak Azure Cosmos DB yönelik istekler hizmet tarafından reddedilir.

> [!WARNING]
> SQL API için .NET SDK 'sının tüm **1. x** sürümleri **30 Ağustos 2020**tarihinde kullanımdan kaldırılacaktır.
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
| [2.2.3](#2.2.3) |11 Şubat 2019 |--- |
| [2.2.2](#2.2.2) |06 Şubat 2019 |--- |
| [2.2.1](#2.2.1) |24 Aralık 2018 |--- |
| [2.2.0](#2.2.0) |07 Aralık 2018 |--- |
| [2.1.3](#2.1.3) |15 Ekim 2018 |--- |
| [2.1.2 'yi](#2.1.2) |04 Ekim 2018 |--- |
| [2.1.1](#2.1.1) |27 Eylül 2018 |--- |
| [2.1.0](#2.1.0) |21 Eylül 2018 |--- |
| [2.0.0](#2.0.0) |Eylül 07, 2018 |--- |
| [1.22.0](#1.22.0) |19 Nisan 2018 | 30 Ağustos 2020 |
| [1.21.1](#1.20.1) |09 Mart 2018 |30 Ağustos 2020 |
| [1.20.2](#1.20.1) |21 Şubat 2018 |30 Ağustos 2020 |
| [1.20.1](#1.20.1) |05 Şubat 2018 |30 Ağustos 2020 |
| [1.19.1](#1.19.1) |16 Kasım 2017 |30 Ağustos 2020 |
| [1.19.0](#1.19.0) |10 Kasım 2017 |30 Ağustos 2020 |
| [1.18.1](#1.18.1) |Kasım 07, 2017 |30 Ağustos 2020 |
| [1.18.0](#1.18.0) |17 Ekim 2017 |30 Ağustos 2020 |
| [1.17.0](#1.17.0) |10 Ağustos 2017 |30 Ağustos 2020 |
| [1.16.1](#1.16.1) |07 Ağustos 2017 |30 Ağustos 2020 |
| [1.16.0](#1.16.0) |02 Ağustos 2017 |30 Ağustos 2020 |
| [1.15.0](#1.15.0) |30 Haziran 2017 |30 Ağustos 2020 |
| [1.14.1](#1.14.1) |23 Mayıs 2017 |30 Ağustos 2020 |
| [1.14.0](#1.14.0) |10 Mayıs 2017 |30 Ağustos 2020 |
| [1.13.4](#1.13.4) |09 Mayıs 2017 |30 Ağustos 2020 |
| [1.13.3](#1.13.3) |06 Mayıs 2017 |30 Ağustos 2020 |
| [1.13.2](#1.13.2) |19 Nisan 2017 |30 Ağustos 2020 |
| [1.13.1](#1.13.1) |29 Mart 2017 |30 Ağustos 2020 |
| [1.13.0](#1.13.0) |24 Mart 2017 |30 Ağustos 2020 |
| [1.12.2](#1.12.2) |20 Mart 2017 |30 Ağustos 2020 |
| [1.12.1](#1.12.1) |14 Mart 2017 |30 Ağustos 2020 |
| [1.12.0](#1.12.0) |15 Şubat 2017 |30 Ağustos 2020 |
| [1.11.4](#1.11.4) |06 Şubat 2017 |30 Ağustos 2020 |
| [1.11.3](#1.11.3) |26 Ocak 2017 |30 Ağustos 2020 |
| [1.11.1](#1.11.1) |21 Aralık 2016 |30 Ağustos 2020 |
| [1.11.0](#1.11.0) |08 Aralık 2016 |30 Ağustos 2020 |
| [1.10.0](#1.10.0) |27 Eylül 2016 |30 Ağustos 2020 |
| [1.9.5](#1.9.5) |01 Eylül 2016 |30 Ağustos 2020 |
| [1.9.4](#1.9.4) |24 Ağustos 2016 |30 Ağustos 2020 |
| [1.9.3](#1.9.3) |15 Ağustos 2016 |30 Ağustos 2020 |
| [1.9.2](#1.9.2) |23 Temmuz 2016 |30 Ağustos 2020 |
| [1.8.0](#1.8.0) |14 Haziran 2016 |30 Ağustos 2020 |
| [1.7.1](#1.7.1) |06 Mayıs 2016 |30 Ağustos 2020 |
| [1.7.0](#1.7.0) |26 Nisan 2016 |30 Ağustos 2020 |
| [1.6.3](#1.6.3) |08 Nisan 2016 |30 Ağustos 2020 |
| [1.6.2](#1.6.2) |29 Mart 2016 |30 Ağustos 2020 |
| [1.5.3](#1.5.3) |19 Şubat 2016 |30 Ağustos 2020 |
| [1.5.2 planlama](#1.5.2) |14 Aralık 2015 |30 Ağustos 2020 |
| [1.5.1](#1.5.1) |23 Kasım 2015 |30 Ağustos 2020 |
| [1.5.0](#1.5.0) |05 Ekim 2015 |30 Ağustos 2020 |
| [1.4.1](#1.4.1) |25 Ağustos 2015 |30 Ağustos 2020 |
| [1.4.0](#1.4.0) |13 Ağustos 2015 |30 Ağustos 2020 |
| [1.3.0](#1.3.0) |05 Ağustos 2015 |30 Ağustos 2020 |
| [1.2.0](#1.2.0) |06 Temmuz 2015 |30 Ağustos 2020 |
| [1.1.0](#1.1.0) |30 Nisan 2015 |30 Ağustos 2020 |
| [1.0.0](#1.0.0) |08 Nisan 2015 | 30 Ağustos 2020 |


## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası. 

