---
title: Azure Cosmos DB .NET değişiklik akışı Işlemcisi API 'SI, SDK sürüm notları
description: Sürüm tarihleri, kullanımdan kaldırma tarihleri ve .NET değişiklik akışı Işlemci SDK 'sının her sürümü arasında yapılan değişiklikler dahil olmak üzere değişiklik akışı Işlemcisi API 'SI ve SDK 'Sı hakkında bilgi edinin.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/11/2020
ms.author: anfeldma
ms.openlocfilehash: b2c846298fecdc771dd5d9831a558b99c74b2737
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87461077"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>.NET değişiklik akışı Işlemcisi SDK 'Sı: Indirme ve sürüm notları

> [!div class="op_single_selector"]
>
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET değişiklik akışı SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK’sı v4](sql-api-sdk-java-v4.md)
> * [Zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md)
> * [Zaman uyumlu Java SDK v2](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * KALANı (/REST/API
> * [REST kaynak sağlayıcısı] (/REST/API
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**SDK indirmesi**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API belgeleri**|[Akış Işlemcisi kitaplık API 'SI başvuru belgelerini değiştirme](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Kullanmaya başlama**|[Değişiklik akışı Işlemcisi .NET SDK 'sını kullanmaya başlama](change-feed.md)|
|**Desteklenen geçerli çerçeve**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET çekirdek](https://www.microsoft.com/net/download/core) |

> [!NOTE]
> Değişiklik akışı işlemcisini kullanıyorsanız, lütfen SDK 'da yerleşik olarak bulunan değişiklik akışını içeren [.NET SDK 'sının](change-feed-processor.md)en son sürümü olan 3. x sürümünü inceleyin. 

## <a name="release-notes"></a>Sürüm notları

### <a name="v2-builds"></a>v2 derlemeleri

### <a name="231"></a><a name="2.3.1"/>2.3.1
* `FeedProcessing.ChangeFeedObserverCloseReason.Unknown` `FeedProcessing.IChangeFeedObserver.CloseAsync` Bölüm bulunamazsa veya hedef çoğaltma okuma oturumuyla güncel değilse, ' a bir kapatma nedeni gönderildiğinde bir durum düzeltildi. Bu durumlarda `FeedProcessing.ChangeFeedObserverCloseReason.ResourceGone` ve `FeedProcessing.ChangeFeedObserverCloseReason.ReadSessionNotAvailable` kapatma nedenleri artık kullanılıyor.
* `FeedProcessing.ChangeFeedObserverCloseReason.ReadSessionNotAvailable`Hedef çoğaltma, okuma oturumuna göre güncel olmadığında değişiklik akışı gözlemcisini kapatmak için gönderilen yeni bir kapatma nedeni eklendi.

### <a name="230"></a><a name="2.3.0"/>2.3.0
* Yeni bir yöntem `ChangeFeedProcessorBuilder.WithCheckpointPartitionProcessorFactory` ve karşılık gelen ortak arabirim eklendi `ICheckpointPartitionProcessorFactory` . Bu, arabiriminin bir uygulamasının `IPartitionProcessor` yerleşik checkişaretleme mekanizması kullanmasına izin verir. Yeni fabrika, `IPartitionProcessorFactory` `Create` yöntemi için de aynı zamanda parametresini de alır `ILeaseCheckpointer` .
* `ChangeFeedProcessorBuilder.WithPartitionProcessorFactory`Aynı örnek için ya da iki yöntemden yalnızca biri `ChangeFeedProcessorBuilder.WithCheckpointPartitionProcessorFactory` kullanılabilir `ChangeFeedProcessorBuilder` .

### <a name="228"></a><a name="2.2.8"></a>2.2.8
* Kararlılık ve tanılama geliştirmeleri:
  * Okuma değişikliği akışını algılamaya kadar uzun süren destek eklendi. Özelliği tarafından belirtilen değerden daha uzun sürerse `ChangeFeedProcessorOptions.ChangeFeedTimeout` , aşağıdaki adımlar alınır:
    * Sorunlu bölümdeki değişiklik akışını okuma işlemi iptal edildi.
    * Değişiklik akışı işlemcisi örneği, sorunlu kiralamanın sahipliğini bırakır. Bırakılan kira, aynı veya farklı değişiklik akışı işlemcisi örneği tarafından yapılacak bir sonraki kira alma adımı sırasında alınacaktır. Bu şekilde, değişiklik akışını okuma baştan başlayacaktır.
    * Durum izleyicisine bir sorun bildirilir. Varsayılan Hemi izleyici, bildirilen tüm sorunları izleme günlüğüne gönderir.
  * Yeni bir ortak özellik eklendi: `ChangeFeedProcessorOptions.ChangeFeedTimeout` . Bu özelliğin varsayılan değeri 10 dakikadır.
  * Yeni bir genel Enum değeri eklendi: `Monitoring.MonitoredOperation.ReadChangeFeed` . Değeri `HealthMonitoringRecord.Operation` olarak ayarlandığında `Monitoring.MonitoredOperation.ReadChangeFeed` , sistem durumu sorununun değişiklik akışını okumayla ilgili olduğunu gösterir.

### <a name="227"></a><a name="2.2.7"></a>2.2.7
* Tüm kiralamalar alınırken senaryo için Yük Dengeleme stratejisi, örneğin ağ sorunlarından dolayı kira süresi sonu aralığından daha uzun sürer:
  * Bu senaryoda yük dengeleme algoritması, kiraları süresi dolmak üzere, etkin sahiplerin kira çalmasını sağlamak için kullanılır. Bu, çok sayıda kiralamaların gereksiz şekilde yeniden dengelenmesini tetikleyebilir.
  * Bu sorun, bu sürümde, sahibin değiştiği süre dolmadan ve bir sonraki yük dengeleme yinelemesine süresinin dolma süresini erteleyerek, bu yayında giderilir.

### <a name="226"></a><a name="2.2.6"></a>2.2.6
* Gözlemci özel durumlarının işlenmesi geliştirildi.
* Gözlemci hatalarıyla ilgili daha zengin bilgi:
  * Gözlemcinin ProcessChangesAsync tarafından oluşturulan bir özel durum nedeniyle gözlemci kapatıldığında, CloseAsync artık ChangeFeedObserverCloseReason. ObserverError olarak ayarlanan Reason parametresini alacaktır.
  * Bir gözlemci içindeki Kullanıcı kodundaki hataları belirlemek için izlemeler eklendi.

### <a name="225"></a><a name="2.2.5"></a>2.2.5
* Paylaşılan veritabanı işleme kullanan koleksiyonlarda bölünmüş işleme desteği eklendi.
  * Bu sürüm, iki yerine yalnızca bir alt bölüm anahtar aralığı oluşturma ile bölüm yeniden dengelemeye neden olduğunda, koleksiyonları, paylaşılan veritabanı aktarım hızı kullanan koleksiyonlara bölme sırasında ortaya çıkabilecek bir sorunu düzeltir. Bu durumda, akış Işlemcisini Değiştir, eski bölüm anahtarı aralığı için kirayı silmeyi ve yeni kiralamalar oluşturmamak için kalmış olabilir. Bu sürümde sorun düzeltildi.

### <a name="224"></a><a name="2.2.4"></a>2.2.4
* Değişiklik akışını istek devamlılık belirtecinden başlatmayı desteklemek için yeni ChangeFeedProcessorOptions. Startdevamlılık özelliği eklendi. Bu yalnızca kira koleksiyonu boş olduğunda veya bir kiralamanın Devamııtoken kümesine sahip olmadığı durumlarda kullanılır. Devamlılık belirteci ayarlanmış kira koleksiyonundaki kiralamalar için, ContinuationToken kullanılır ve ChangeFeedProcessorOptions. Startcontinuyoksayıldı.

### <a name="223"></a><a name="2.2.3"></a>2.2.3
* Bölüm başına devamlılık belirteçlerini sürdürmek için özel depo kullanma desteği eklendi.
  * Örneğin, özel bir kiralama deposu herhangi bir özel şekilde bölümlenmiş Azure Cosmos DB kira koleksiyonu olabilir.
  * Özel Kiralama depoları, yeni genişletilebilirlik noktası ChangeFeedProcessorBuilder. WithLeaseStoreManager (ILeaseStoreManager) ve ILeaseStoreManager ortak arabirimini kullanabilir.
  * ILeaseManager arabirimini birden çok rol arabirimine yeniden düzenlenmiş.
* Küçük bir değişiklik: ChangeFeedProcessorBuilder. WithLeaseManager (ILeaseManager) genişletilebilirlik noktası kaldırıldı, bunun yerine ChangeFeedProcessorBuilder. WithLeaseStoreManager (ILeaseStoreManager) kullanın.

### <a name="222"></a><a name="2.2.2"></a>2.2.2
* Bu sürüm, izlenen bir koleksiyonu işleme ve bölümlenmiş bir kira koleksiyonu kullanma sırasında oluşan bir sorunu düzeltir. Bölünmüş bölüm için bir kira işlenirken, bu bölüme karşılık gelen kira silinmeyebilir. Bu sürümde sorun düzeltildi.

### <a name="221"></a><a name="2.2.1"></a>2.2.1
* Çoklu ana hesaplar ve yeni oturum belirteci biçimi için sabit Estimator hesaplaması.

### <a name="220"></a><a name="2.2.0"></a>2.2.0
* Bölümlenmiş kira koleksiyonları için destek eklendi. Bölüm anahtarının/ıdolarak tanımlanması gerekir.
* Küçük değişiklik: ıchanneldocumentclient arabiriminin ve ChangeFeedDocumentClient sınıfının yöntemleri, RequestOptions ve CancellationToken parametrelerini içerecek şekilde değiştirilmiştir. Ichanneldocumentclient, örnek izleme, hata işleme, vb. gibi belge Istemcisinin değişiklik akışı Işlemcisi ile birlikte kullanılması için özel uygulama sağlamanıza olanak tanıyan gelişmiş bir genişletilebilirlik noktasıdır. Bu güncelleştirmeyle, ıchanneldocumentclient uygulayan kodun, uygulamadaki yeni parametreleri içerecek şekilde değiştirilmesi gerekir.
* Küçük tanılama geliştirmeleri.

### <a name="210"></a><a name="2.1.0"></a>2.1.0
* Yeni API eklendi, görev &lt; IReadOnlyList &lt; RemainingPartitionWork &gt; &gt; IRemainingWorkEstimator. GetEstimatedRemainingWorkPerPartitionAsync (). Bu, her bölüm için tahmini çalışma almak üzere kullanılabilir.
* Microsoft.Azure.DocumentDB SDK 2,0 ' i destekler. Microsoft.Azure.DocumentDB 2,0 veya üstünü gerektirir.

### <a name="206"></a><a name="2.0.6"></a>2.0.6
* V1 uyumluluğu için ChangeFeedEventHost. HostName ortak özelliği eklendi.

### <a name="205"></a><a name="2.0.5"></a>2.0.5
* Bölüm ayırma sırasında oluşan bir yarış durumu düzeltildi. Yarış durumu kira elde edebilir ve bölüm bölme sırasında hemen kaybı ve çekişmeye neden olabilir. Yarış durumu sorunu bu sürümle düzeltilmiştir.

### <a name="204"></a><a name="2.0.4"></a>2.0.4
* GA SDK

### <a name="203-prerelease"></a><a name="2.0.3-prerelease"></a>2.0.3-ön sürüm
* Aşağıdaki sorunlar çözülmüştür:
  * Bölüm bölünmesi gerçekleştiğinde, bölme öncesinde değiştirilen belgelerin yinelenen işlenmesi olabilir.
  * GetEstimatedRemainingWork API 'SI, kira koleksiyonunda hiçbir kira sunulmadan 0 döndürdü.

* Aşağıdaki özel durumlar ortak hale getirilir. Ipartitionprocessor uygulayan uzantılar bu özel durumları oluşturabilir.
  * Microsoft.Azure.Documlar. ChangeFeedProcessor. Exceptions. LeaseLostException. 
  * Microsoft.Azure.Documlar. ChangeFeedProcessor. Exceptions. PartitionException. 
  * Microsoft.Azure.Documlar. ChangeFeedProcessor. Exceptions. PartitionNotFoundException.
  * Microsoft.Azure.Documlar. ChangeFeedProcessor. Exceptions. Partitionsptexception. 

### <a name="202-prerelease"></a><a name="2.0.2-prerelease"></a>2.0.2-ön sürüm
* Küçük API değişiklikleri:
  * Eski olarak işaretlenen ChangeFeedProcessorOptions. ısoto Checkpointenabled kaldırıldı.

### <a name="201-prerelease"></a><a name="2.0.1-prerelease"></a>2.0.1-ön sürüm
* Kararlılık geliştirmeleri:
  * Kira deposunun başlatılmasının daha iyi işlenmesi. Kira deposu boş olduğunda, işlemcinin yalnızca bir örneği başlatabilir, diğerleri bekleyecektir.
  * Daha kararlı/verimli kira yenileme/yayınlama. Bir kiralamanın yenilenmesi ve serbest bırakılması, diğerlerinin yenilenmesinin bağımsız olduğu bir bölümdür. Tüm bölümler için sırayla yapılan v1 'de.
* Yeni v2 API 'SI:
  * İşlemcinin esnek olarak oluşturulması için Oluşturucu deseninin: ChangeFeedProcessorBuilder sınıfı.
    * , Parametrelerin herhangi bir birleşimini alabilir.
    * , İzleme ve/veya kira koleksiyonu (v1 'de kullanılamaz) için DocumentClient örneği alabilir.
  * Ichangefeedobserver. ProcessChangesAsync artık CancellationToken 'ı alıyor.
  * IRemainingWorkEstimator-kalan iş tahmin aracı, işlemciden ayrı olarak kullanılabilir.
  * Yeni genişletilebilirlik noktaları:
    * IPartitionLoadBalancingStrategy-işlemci örnekleri arasında bölümlerin özel yük dengelemesi için.
    * Ileao, ILeaseManager-Özel Kiralama Yönetimi için.
    * Ipartitionprocessor-bir bölümdeki özel işleme değişiklikleri için.
* Günlüğe kaydetme- [Liblog](https://github.com/damianh/LibLog) kitaplığını kullanır.
* %100 v1 API 'siyle geriye dönük olarak uyumludur.
* Yeni kod tabanı.
* [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.21.1 ve üzeri sürümleriyle uyumludur.

### <a name="v1-builds"></a>V1 derlemeleri

### <a name="133"></a><a name="1.3.3"></a>1.3.3
* Daha fazla günlüğe kaydetme eklendi.
* Bekleyen iş tahmini birden çok kez çağrılırken bir DocumentClient sızıntısı düzeltildi.

### <a name="132"></a><a name="1.3.2"></a>1.3.2
* Bekleyen iş tahmininizdeki düzeltmeler.

### <a name="131"></a><a name="1.3.1"></a>1.3.1
* Kararlılık geliştirmeleri.
  * Bazı bölümlerde durdurulmuş gözlemcilerin 'a neden olabilecek iptal edilen görevleri işleme sorununu giderme.
* El ile checkişaretleyici desteği.
* [SQL .NET SDK](sql-api-sdk-dotnet.md) 1,21 ve üzeri sürümleriyle uyumludur.

### <a name="120"></a><a name="1.2.0"></a>1.2.0
* 2,0 .NET Standard için destek ekler. Paket artık `netstandard2.0` ve `net451` framework takma adlarını destekler.
* [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.17.0 ve üzeri sürümleriyle uyumludur.
* 1.5.1 ve üzeri [SQL .NET Core SDK](sql-api-sdk-dotnet-core.md) sürümleriyle uyumludur.

### <a name="111"></a><a name="1.1.1"></a>1.1.1
* Değişiklik akışı boş olduğunda veya hiçbir iş beklenmediği zaman, kalan çalışmanın tahmini hesaplamasına ilişkin bir sorunu düzeltir.
* [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.13.2 ve üzeri sürümleriyle uyumludur.

### <a name="110"></a><a name="1.1.0"></a>1.1.0
* Değişiklik akışında işlenecek kalan çalışmanın tahminini elde etmek için bir yöntem eklenmiştir.
* [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.13.2 ve üzeri sürümleriyle uyumludur.

### <a name="100"></a><a name="1.0.0"></a>1.0.0
* GA SDK
* [SQL .NET SDK](sql-api-sdk-dotnet.md) 1.14.1 ve altı sürümleriyle uyumludur.

## <a name="release--retirement-dates"></a>Yayın & kullanımdan kaldırma tarihleri

Microsoft, daha yeni/desteklenen bir sürüme geçişi düzgünleştirmek için SDK 'nın devre dışı bırakılmasının ardından en az **12 ay** önce bildirim sağlayacaktır. Yeni özellikler ve işlevler ve iyileştirmeler yalnızca geçerli SDK 'ya eklenir, bu nedenle en son SDK sürümüne her zaman olabildiğince erken yükseltmeniz önerilir.

> [!WARNING]
> 31 Ağustos 2022 ' den sonra Azure Cosmos DB artık hata düzeltmeleri yapamayacak, yeni özellikler ekleyecek ve SQL API 'SI için Azure Cosmos DB .NET veya .NET Core SDK sürümleri için destek sağlamalıdır. Yükseltmemeyi tercih ediyorsanız, SDK 'nın 1. x sürümünden gönderilen istekler Azure Cosmos DB hizmeti tarafından sunulmayı sürdürmeye devam edecektir.

<br/>

| Sürüm | Yayın Tarihi | Kullanımdan Kaldırma Tarihi |
| --- | --- | --- |
| [2.3.1](#2.3.1) |30 Temmuz 2020 |--- |
| [2.3.0](#2.3.0) |2 Nisan 2020 |--- |
| [2.2.8](#2.2.8) |28 Ekim 2019 |--- |
| [2.2.7](#2.2.7) |14 Mayıs 2019 |--- |
| [2.2.6](#2.2.6) |29 Ocak 2019 |--- |
| [2.2.5](#2.2.5) |13 Aralık 2018 |--- |
| [2.2.4](#2.2.4) |29 Kasım 2018 |--- |
| [2.2.3](#2.2.3) |19 Kasım 2018 |--- |
| [2.2.2](#2.2.2) |31 Ekim 2018 |--- |
| [2.2.1](#2.2.1) |24 Ekim 2018 |--- |
| [1.3.3](#1.3.3) |08 Mayıs 2018 |--- |
| [1.3.2](#1.3.2) |18 Nisan 2018 |--- |
| [1.3.1](#1.3.1) |13 Mart 2018 |--- |
| [1.2.0](#1.2.0) |31 Ekim 2017 |--- |
| [1.1.1](#1.1.1) |29 Ağustos 2017 |--- |
| [1.1.0](#1.1.0) |13 Ağustos 2017 |--- |
| [1.0.0](#1.0.0) |07 Temmuz 2017 |--- |

## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.

Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.
