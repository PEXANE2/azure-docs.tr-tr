---
title: Azure Cosmos DB .NET değişiklik beslemesi Işlemci API, SDK sürüm notları
description: .NET Change Feed Processor SDK'nın her sürümü arasında yapılan sürüm tarihleri, emeklilik tarihleri ve değişiklikler de dahil olmak üzere Feed İşlemci API'si ve SDK hakkında her şeyi öğrenin.
author: ealsur
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 01/30/2019
ms.author: maquaran
ms.openlocfilehash: 9252e3e41d0c639231a2abe20202499c6b3ee32a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75444851"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>.NET Değişim Beslemeİşlemi SDK: Notları indirin ve yayınla

> [!div class="op_single_selector"]
>
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET Değişim Akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [Geri kalanı](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu uygulayıcı - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu uygulayıcı - Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**SDK indir**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API belgeleri**|[Özet Akışı İşlemci kitaplığı API başvuru belgelerini değiştirme](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Kullanmaya başlayın**|[Akış İşlemcisi .NET SDK'yı Değiştir ile başlayın](change-feed.md)|
|**Geçerli desteklenen çerçeve**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

> [!NOTE]
> Besleme işlemcisini değiştirin, lütfen [.NET SDK'nın 3.x](change-feed-processor.md)en son sürümüne bakın. 

## <a name="release-notes"></a>Sürüm notları

### <a name="v2-builds"></a>v2 oluşturur

### <a name="228"></a><a name="2.2.8"/>2.2.8
* Kararlılık ve tanıyauygunluk iyileştirmeleri:
  * Okuma değişikliği akışının uzun sürdüğünü algılamak için destek eklendi. `ChangeFeedProcessorOptions.ChangeFeedTimeout` Özellik tarafından belirtilen değerden daha uzun sürdüğünde, aşağıdaki adımlar atılır:
    * Sorunlu bölüm üzerindeki değişiklik akışını okuma işlemi iptal edildi.
    * Değişiklik akışı işlemci örneği sorunlu kira sahipliğini düşürür. Bırakılan kira, aynı veya farklı değişiklik besleme işlemcisi örneği tarafından yapılacak bir sonraki kira alma adımı sırasında alınır. Bu şekilde, okuma değişikliği beslemesi baştan başlayacaktır.
    * Bir sorun sağlık monitörüne bildirilir. Varsayılan sağlık monitörü, bildirilen tüm sorunları izleme günlüğüne gönderir.
  * Yeni bir kamu `ChangeFeedProcessorOptions.ChangeFeedTimeout`malı eklendi: . Bu özelliğin varsayılan değeri 10 dakikadır.
  * Yeni bir public enum `Monitoring.MonitoredOperation.ReadChangeFeed`değeri eklendi: . Değeri `HealthMonitoringRecord.Operation` `Monitoring.MonitoredOperation.ReadChangeFeed`ayarlandığında, sistem durumu sorununun okuma değişikliği akışıyla ilişkili olduğunu gösterir.

### <a name="227"></a><a name="2.2.7"/>2.2.7
* Tüm kiralamaları alırken senaryo için geliştirilmiş yük dengeleme stratejisi, örneğin ağ sorunları nedeniyle, kira son kullanma aralığından daha uzun sürer:
  * Bu senaryoda, kiralamaların süresi dolmuş olarak yanlış olarak kabul etmek için kullanılan yük dengeleme algoritması, etkin sahiplerden kira çalmaya neden olur. Bu, birçok kiralamayı gereksiz yeniden dengelemeyi tetikleyebilir.
  * Bu sorun, sahibi değişmedi süresi dolmuş kira satın alma ve sonraki yük dengeleme yineleme için bir sonraki yük dengeleme için süresi dolmuş kira edinme posponing sırasında çakışma yeniden kaçınarak bu sürümde giderilir.

### <a name="226"></a><a name="2.2.6"/>2.2.6
* Observer özel durumlarının daha iyi işlenmesi.
* Gözlemci hataları hakkında daha zengin bilgi:
  * Bir Observer, Observer'ın ProcessChangesAsync tarafından atılan bir özel durum nedeniyle kapatıldığında, CloseAsync şimdi ChangeFeedObserverCloseReason.ObserverError için ayarlanan neden parametresini alır.
  * Bir Observer'da kullanıcı kodu içindeki hataları tanımlamak için izlemeler eklendi.

### <a name="225"></a><a name="2.2.5"/>2.2.5
* Paylaşılan veritabanı iş veritabanı iş için bölme işleme desteği eklendi.
  * Bu sürüm, sonucu iki yerine oluşturulan tek bir alt bölüm anahtar aralığıyla yeniden bölme ye bölmeye bölündüğünde paylaşılan veritabanı iş verisini kullanarak koleksiyonlarda bölme sırasında oluşabilecek bir sorunu giderir. Bu durumda, Özet Akışı İşlemcisi Değiştir, eski bölüm anahtar aralığının kirasını serken ve yeni kiralamalar oluşturmayabilir. Sorun bu sürümde giderilmiştir.

### <a name="224"></a><a name="2.2.4"/>2.2.4
* İstek devamı belirteci değişiklik akışı başlatmak desteklemek için yeni özellik ChangeFeedProcessorOptions.StartDevamı eklendi. Bu yalnızca kira tahsilatı boşsa veya kira devam verme kümesi yoksa kullanılır. ContinuationToken kümesi olan kira koleksiyonundaki kiralamalarda ContinuationToken kullanılır ve ChangeFeedProcessorOptions.StartContinuation göz ardı edilir.

### <a name="223"></a><a name="2.2.3"/>2.2.3
* Bölüm başına devam belirteçlerini kalıcı olarak sürdürmek için özel depo kullanmak için destek eklendi.
  * Örneğin, özel bir kiralama mağazası Azure Cosmos DB kiralama koleksiyonu herhangi bir özel şekilde bölümlenmiş olabilir.
  * Özel kiralama mağazaları yeni genişletilebilirlik noktası ChangeFeedProcessorBuilder.WithLeaseStoreManager (ILeaseStoreManager) ve ILeaseStoreManager ortak arabirimi kullanabilirsiniz.
  * ILeaseManager arabirimini birden çok rol arabirimine dönüştürdü.
* Küçük kırılma değişikliği: kaldırılan genişletilebilirlik noktası ChangeFeedProcessorBuilder.WithLeaseManager(ILeaseManager), yerine ChangeFeedProcessorBuilder.WithLeaseStoreManager(ILeaseStoreManager) kullanın.

### <a name="222"></a><a name="2.2.2"/>2.2.2
* Bu sürüm, izlenen koleksiyondaki bir bölmeyi işlerken ve bölümlenmiş bir kira koleksiyonu kullanırken oluşan bir sorunu giderir. Bölünmüş bölüm için kira işlemi yaparken, bu bölüme karşılık gelen kira silinmeyebilir. Sorun bu sürümde giderilmiştir.

### <a name="221"></a><a name="2.2.1"/>2.2.1
* Multi Master hesapları ve yeni Oturum Belirteç biçimi için Tahmin doğrulayıcı hesaplaması.

### <a name="220"></a><a name="2.2.0"/>2.2.0
* Bölümlenmiş kira koleksiyonları için destek eklendi. Bölüm anahtarı /id olarak tanımlanmalıdır.
* Küçük kırılma değişimi: IChangeFeedDocumentClient arabiriminin yöntemleri ve ChangeFeedDocumentClient sınıfı RequestOptions ve CancellationToken parametrelerini içerecek şekilde değiştirildi. IChangeFeedDocumentClient, Belge İstemci'sinin Değişim Akışı İşlemcisi ile özel bir uygulamasını sağlamanıza olanak tanıyan gelişmiş bir genişletilebilirlik noktasıdır, örneğin DocumentClient'ı dekore edin ve ekstra izleme, hata işleme yapmak için tüm çağrıları engelleme Vb. Bu güncelleştirmeyle, IChangeFeedDocumentClient'ı uygulayan kodun uygulamaya yeni parametreler içerecek şekilde değiştirilmesi gerekir.
* Küçük tanı lama iyileştirmeleri.

### <a name="210"></a><a name="2.1.0"/>2.1.0
* Yeni API&lt;eklendi, Görev&lt;IReadOnlyList RemainingPartitionWork&gt; &gt; IRemainingWorkEstimator.GetEstimatedRemainingWorkPerPartitionAsync(). Bu, her bölüm için tahmini çalışma almak için kullanılabilir.
* Microsoft.Azure.DocumentDB SDK 2.0 destekler. Microsoft.Azure.DocumentDB 2.0 veya sonrası gerektirir.

### <a name="206"></a><a name="2.0.6"/>2.0.6
* V1 ile uyumluluk için ChangeFeedEventHost.HostName ortak özelliği eklendi.

### <a name="205"></a><a name="2.0.5"/>2.0.5
* Bölme bölünmesi sırasında oluşan bir yarış koşulu düzeltildi. Yarış koşulu kira satın alma ve hemen bölme bölünme sırasında kaybetme ve çekişmeneden yol açabilir. Yarış koşulu sorunu bu sürümle giderilmiştir.

### <a name="204"></a><a name="2.0.4"/>2.0.4
* GA SDK

### <a name="203-prerelease"></a><a name="2.0.3-prerelease"/>2.0.3-ön sürüm
* Aşağıdaki sorunlar çözülmüştür:
  * Bölme bölünmesi gerçekleştiğinde, bölmeden önce değiştirilen belgelerin yinelenen işlemesi olabilir.
  * GetEstimatedRemainingWork API, kira koleksiyonunda kira yokken 0 döndürdü.

* Aşağıdaki istisnalar herkese açıklanır. IPartitionProcessor uygulayan uzantılar bu özel durumları atabilir.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-ön sürüm
* Küçük API değişiklikleri:
  * Kaldırılan ChangeFeedProcessorOptions.IsAutoCheckpointEnabled eski olarak işaretlenmiş.

### <a name="201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-ön sürüm
* İstikrar iyileştirmeleri:
  * Kira deposu başlatma nın daha iyi işlenmesi. Kira deposu boşolduğunda, yalnızca bir işlemci örneği onu açabilir, diğerleri bekler.
  * Daha kararlı/verimli kira yenileme/serbest bırakma. Bir bölümün yenilenmesi ve serbest bırakılması, diğerbölümleri yenilemekten bağımsızdır. V1 tüm bölümler için sırayla yapıldı.
* Yeni v2 API:
  * İşlemcinin esnek yapısı için oluşturucu deseni: ChangeFeedProcessorBuilder sınıfı.
    * Parametrelerin herhangi bir kombinasyonu alabilir.
    * İzleme ve/veya kira toplama için DocumentClient örneğini alabilir (v1'de kullanılamaz).
  * IChangeFeedObserver.ProcessChangesAsync şimdi CancellationToken alır.
  * IRemainingWorkEstimator - kalan iş tahmincisi işlemciden ayrı olarak kullanılabilir.
  * Yeni genişletilebilirlik noktaları:
    * IPartitionLoadBalancingStrategy - işlemci örnekleri arasındaki bölümlerin özel yük dengeleme için.
    * ILease, ILeaseManager - özel kira yönetimi için.
    * IPartitionProcessor - bir bölüm üzerinde özel işleme değişiklikleri için.
* Günlük - [LibLog](https://github.com/damianh/LibLog) kitaplığı kullanır.
* % 100 geriye v1 API ile uyumlu.
* Yeni kod tabanı.
* SQL [.NET SDK](sql-api-sdk-dotnet.md) sürümleri 1.21.1 ve üzeri ile uyumludur.

### <a name="v1-builds"></a>v1 oluşturur

### <a name="133"></a><a name="1.3.3"/>1.3.3
* Daha fazla günlük eklendi.
* Bekleyen iş tahminini birden çok kez ararken DocumentClient sızıntısı düzeltildi.

### <a name="132"></a><a name="1.3.2"/>1.3.2
* Bekleyen iş tahmininde düzeltmeler.

### <a name="131"></a><a name="1.3.1"/>1.3.1
* İstikrar iyileştirmeleri.
  * Bazı bölümlerde durdurulan gözlemcilere yol açabilecek iptal edilen görevleri işleme düzeltme sorunu.
* Manuel denetim noktası desteği.
* SQL [.NET SDK](sql-api-sdk-dotnet.md) sürümleri 1.21 ve üzeri ile uyumludur.

### <a name="120"></a><a name="1.2.0"/>1.2.0
* .NET Standard 2.0 için destek ekler. Paket şimdi `netstandard2.0` destekler `net451` ve çerçeve monikers.
* SQL [.NET SDK](sql-api-sdk-dotnet.md) sürümleri 1.17.0 ve üzeri ile uyumludur.
* SQL [.NET Core SDK](sql-api-sdk-dotnet-core.md) sürümleri 1.5.1 ve üzeri ile uyumludur.

### <a name="111"></a><a name="1.1.1"/>1.1.1
* Değişiklik Akışı boşken veya bekleyen çalışma olmadığında kalan çalışma tahmininin hesaplanmasıyla ilgili bir sorunu giderir.
* SQL [.NET SDK](sql-api-sdk-dotnet.md) sürümleri 1.13.2 ve üzeri ile uyumludur.

### <a name="110"></a><a name="1.1.0"/>1.1.0
* Değişiklik Akışı'nda işlenecek kalan çalışmanın tahminini elde etmek için bir yöntem eklendi.
* SQL [.NET SDK](sql-api-sdk-dotnet.md) sürümleri 1.13.2 ve üzeri ile uyumludur.

### <a name="100"></a><a name="1.0.0"/>1.0.0
* GA SDK
* SQL [.NET SDK](sql-api-sdk-dotnet.md) sürümleri 1.14.1 ve altında uyumludur.

## <a name="release--retirement-dates"></a>Emeklilik tarihleri & yayın

Microsoft, daha yeni/desteklenen bir sürüme geçişi kolaylaştırmak için bir SDK'nın emekliye geçmesinden en az **12 ay** önce bildirim de verecektir.

Yeni özellikler, işlevsellik ve optimizasyonlar yalnızca geçerli SDK'ya eklenir, bu nedenle her zaman en son SDK sürümüne mümkün olduğunca erken yükseltmeniz önerilir. 

Cosmos DB'ye emekli bir SDK kullanarak yapılan tüm istekler hizmet tarafından reddedilecektir.

<br/>

| Sürüm | Yayın Tarihi | Emeklilik Tarihi |
| --- | --- | --- |
| [2.2.8](#2.2.8) |Ekim 28, 2019 |--- |
| [2.2.7](#2.2.7) |14 Mayıs 2019 |--- |
| [2.2.6](#2.2.6) |Ocak 29, 2019 |--- |
| [2.2.5](#2.2.5) |13 Aralık 2018 |--- |
| [2.2.4](#2.2.4) |Kasım 29, 2018 |--- |
| [2.2.3](#2.2.3) |19 Kasım 2018 |--- |
| [2.2.2](#2.2.2) |31 Ekim 2018 |--- |
| [2.2.1](#2.2.1) |Ekim 24, 2018 |--- |
| [1.3.3](#1.3.3) |08 Mayıs 2018 |--- |
| [1.3.2](#1.3.2) |18 Nisan 2018 |--- |
| [1.3.1](#1.3.1) |13 Mart 2018 |--- |
| [1.2.0](#1.2.0) |31 Ekim 2017 |--- |
| [1.1.1](#1.1.1) |Ağustos 29, 2017 |--- |
| [1.1.0](#1.1.0) |Ağustos 13, 2017 |--- |
| [1.0.0](#1.0.0) |Temmuz 07, 2017 |--- |

## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.

Cosmos DB hakkında daha fazla bilgi edinmek için [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfasına bakın.
