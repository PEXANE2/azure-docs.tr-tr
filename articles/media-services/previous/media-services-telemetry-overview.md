---
title: Azure Media Services telemetri | Microsoft Docs
description: Bu makalede Microsoft Azure Media Services telemetriye genel bakış sunulmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 95c20ec4-c782-4063-8042-b79f95741d28
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: e2cbb36158722a47518f575b391340b5e25bd908
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74895785"
---
# <a name="azure-media-services-telemetry"></a>Azure Media Services telemetri  


> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz. [v2 'den v3 'e geçiş kılavuzu](../latest/migrate-from-v2-to-v3.md)

Azure Media Services (AMS), Hizmetleri için telemetri/ölçüm verilerine erişmenizi sağlar. AMS 'nin geçerli sürümü, canlı **Kanal**, **streamingendpoint**ve canlı **Arşiv** varlıkları için telemetri verilerini toplamanıza olanak tanır. 

Telemetri, belirttiğiniz bir Azure depolama hesabındaki bir depolama tablosuna yazılır (genellikle AMS hesabınızla ilişkili depolama hesabını kullanırsınız). 

Telemetri sistemi, veri bekletmeyi yönetmez. Depolama tablolarını silerek eski telemetri verilerini kaldırabilirsiniz.

Bu konuda AMS telemetrinin nasıl yapılandırılacağı ve kullanılacağı açıklanmaktadır.

## <a name="configuring-telemetry"></a>Telemetriyi yapılandırma

Bir bileşen düzeyinde ayrıntı düzeyi için telemetri yapılandırabilirsiniz. "Normal" ve "verbose" olmak üzere iki ayrıntı düzeyi vardır. Şu anda her iki düzey de aynı bilgileri döndürür. "Normal" kullanılması önerilir. 

Aşağıdaki konularda Telemetriyi etkinleştirme gösterilmektedir:

[.NET ile telemetri sağlama](media-services-dotnet-telemetry.md) 

[REST ile telemetri etkinleştiriliyor](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Telemetri bilgilerini kullanma

Telemetri, Media Services hesabı için telemetri yapılandırdığınızda belirttiğiniz depolama hesabındaki bir Azure depolama tablosuna yazılır. Bu bölümde ölçümler için depolama tabloları açıklanmaktadır.

Telemetri verilerini aşağıdaki yollarla kullanabilirsiniz:

- Verileri doğrudan Azure Tablo depolamadan (örn. depolama SDK 'sını kullanarak) okuyun. Telemetri depolama tablolarının açıklaması için [Bu](https://msdn.microsoft.com/library/mt742089.aspx) konudaki **telemetri bilgilerine** bakın.

Veya

- [Bu](media-services-dotnet-telemetry.md) konuda açıklandığı gibi, depolama verilerini okumak için MEDIA SERVICES .NET SDK ' daki desteği kullanın. 


Aşağıda açıklanan telemetri şeması, Azure Tablo depolama sınırları dahilinde iyi bir performans sağlamak için tasarlanmıştır:

- Veriler, her hizmetten bağımsız olarak sorgulanmasını sağlamak için hesap KIMLIĞI ve hizmet KIMLIĞI tarafından bölümlenir.
- Bölümler, bölüm boyutu üzerinde makul bir üst sınır sağlamak için tarihi içerir.
- Satır anahtarları, belirli bir hizmet için en son telemetri öğelerinin sorgulanmasını sağlamak için ters zaman sıralardır.

Bu, birçok ortak sorgunun verimli olmasını sağlamalıdır:

- Ayrı hizmetler için verilerin paralel ve bağımsız indirilmesi.
- Belirli bir hizmetin tüm verileri bir tarih aralığında alınıyor.
- Bir hizmet için en son verileri alma.

### <a name="telemetry-table-storage-output-schema"></a>Telemetri tablosu depolama çıkış şeması

Telemetri verileri, "TelemetryMetrics20160321" ("20160321") oluşturulan tablonun tarihi olmak üzere tek bir tabloda toplu olarak depolanır. Telemetri sistemi, 00:00 UTC 'ye göre her yeni gün için ayrı bir tablo oluşturur. Tablo, belirli bir zaman penceresi, gönderilen bayt vb. için alma bit hızı gibi yinelenen değerleri depolamak için kullanılır. 

Özellik|Değer|Örnekler/notlar
---|---|---
PartitionKey|{Hesap KIMLIĞI} _ {varlık KIMLIĞI}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66<br/<br/>Birden çok Media Services hesabının aynı depolama hesabına yazıldığı iş akışlarını basitleştirmek için hesap KIMLIĞI bölüm anahtarına dahil edilir.
RowKey|{saniye-gece yarısı} _ {rastgele değer}|01688_00199<br/><br/>Satır anahtarı, bir bölüm içinde ilk n stil sorgusuna izin vermek için saniye cinsinden gece yarısına kadar başlar. Daha fazla bilgi için [bu makaleye](../../cosmos-db/table-storage-design-guide.md#log-tail-pattern) bakın. 
Zaman damgası|Tarih/saat|Azure Tablo 2016 ' dan otomatik zaman damgası-09-09T22:43:42.241 Z
Tür|Telemetri verileri sağlayan varlığın türü|Channel/StreamingEndpoint/Arşivi<br/><br/>Olay türü yalnızca bir dize değeridir.
Adı|Telemetri olayının adı|Channelsinyal/StreamingEndpointRequestLog
ObservedTime|Telemetri olayının gerçekleştiği zaman (UTC)|2016-09-09T22:42:36.924 Z<br/><br/>Gözlemlenen zaman, telemetri gönderen varlık tarafından sağlanır (örneğin bir kanal). Bu değerin yaklaşık olması için bileşenler arasında zaman eşitleme sorunları olabilir
ServiceId|{hizmet KIMLIĞI}|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Varlığa özgü özellikler|Olay tarafından tanımlanan şekilde|StreamName: STREAM1, bit hızı 10123,...<br/><br/>Kalan özellikler, belirtilen olay türü için tanımlanmıştır. Azure tablo içeriği anahtar değer çiftleridir.  (diğer bir deyişle, tablodaki farklı satırların farklı özellik kümeleri vardır).

### <a name="entity-specific-schema"></a>Varlığa özgü şema

Her biri aşağıdaki sıklıkta itilmiş, varlığa özgü üç telemetri veri girişi türü vardır:

- Akış uç noktaları: 30 saniyede bir
- Canlı Kanallar: her dakika
- Canlı Arşiv: her dakika

**Akış Uç Noktası**

Özellik|Değer|Örnekler
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Zaman damgası|Zaman damgası|Azure Tablo 2016 ' dan otomatik zaman damgası-09-09T22:43:42.241 Z
Tür|Tür|Streammingendpoint
Adı|Adı|Streammingendpointrequestlog
ObservedTime|ObservedTime|2016-09-09T22:42:36.924 Z
ServiceId|Hizmet kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ana bilgisayar adı|Uç noktanın ana bilgisayar adı|builddemoserver.origin.mediaservices.windows.net
StatusCode|HTTP durumunu kaydeder|200
ResultCode|Sonuç kodu ayrıntısı|S_OK
RequestCount|Toplamada toplam istek|3
BytesSent|Gönderilen toplam bayt sayısı|2987358
Sunucugecikmesi|Ortalama sunucu gecikmesi (depolama dahil)|129
E2ELatency|Ortalama uçtan uca gecikme süresi|250

**Canlı Kanal**

Özellik|Değer|Örnekler/notlar
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Zaman damgası|Zaman damgası|Azure Tablo 2016 ' dan otomatik zaman damgası-09-09T22:43:42.241 Z
Tür|Tür|Kanal
Adı|Adı|Channelsinyal
ObservedTime|ObservedTime|2016-09-09T22:42:36.924 Z
ServiceId|Hizmet kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|Video/ses/metin izleme türü|video/ses
TrackName|İzlemenin adı|video/audio_1
Bit hızı|Bit hızını izle|785000
CustomAttributes||   
Incomingbit hızı|Gerçek gelen bit hızı|784548
Fazla atlama sayısı|Alma sırasında örtüşme|0
DiscontinuityCount|İzleme için süreksizlik|0
LastTimestamp|Son alınan veri zaman damgası|1800488800
Nonıncreasingcount|Artmayan zaman damgası nedeniyle atılan parçaların sayısı|2
Alignedkarelerine geri dön|Ana çerçevelerin hizalanmayacağı parçaları (kalite seviyeleri arasında) almamız |Doğru
Alignedpresentationtime|Sunum zamanının Hizalanmadığını parçalar (kalite seviyeleri/parçalar arasında) almış olup olmadığı|Doğru
UnexpectedBitrate|Ses/video izleme için hesaplanmış/gerçek bit hızı > 40.000 bps ve ıncomingbit hızı = = 0 ya da ıncomingbit hızı ve gerçek bit hızı %50 oranında farklılık gösteriyorsa doğru |Doğru
Sorunsuz|Doğruysa, <br/>Fazla atlama sayısı, <br/>DiscontinuityCount, <br/>Nonıncreasingcount, <br/>Alignedana kareleri, <br/>Alignedpresentationtime, <br/>UnexpectedBitrate<br/> tümü 0 ' dır|Doğru<br/><br/>Sağlıklı, aşağıdaki koşullardan herhangi biri tutulmamışsa yanlış döndüren bileşik bir işlevdir:<br/><br/>-OverlapCount > 0<br/>-DiscontinuityCount > 0<br/>-Nonıncreasingcount > 0<br/>-Unalignedana kareleri = = true<br/>-UnalignedPresentationTime = = true<br/>-UnexpectedBitrate = = true

**Canlı Arşiv**

Özellik|Değer|Örnekler/notlar
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Zaman damgası|Zaman damgası|Azure Tablo 2016 ' dan otomatik zaman damgası-09-09T22:43:42.241 Z
Tür|Tür|Arşiv
Adı|Adı|ArchiveHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924 Z
ServiceId|Hizmet kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ManifestName|Program URL 'si|varlık-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4BD2-8c01-a92a2b38c9ba. ISM
TrackName|İzlemenin adı|audio_1
TrackType|İzleme türü|Ses/video
Yay|Aynı ada ve bit hızına sahip farklı bir izleme arasında ayrım yapan onaltılı dize (çok kamera açısı)|
Bit hızı|Bit hızını izle|785000
Sorunsuz|True, IF FragmentDiscardedCount = = 0 & & ArchiveAcquisitionError = = false|True (Bu iki değer ölçüm içinde yok ancak kaynak olayda var)<br/><br/>Sağlıklı, aşağıdaki koşullardan herhangi biri tutulmamışsa yanlış döndüren bileşik bir işlevdir:<br/><br/>-FragmentDiscardedCount > 0<br/>-ArchiveAcquisitionError = = true

## <a name="general-qa"></a>Genel soru-cevap &

### <a name="how-to-consume-metrics-data"></a>Ölçüm verileri nasıl tüketiliyor?

Ölçüm verileri, müşterinin depolama hesabında bir dizi Azure tablosu olarak depolanır. Bu veriler aşağıdaki araçları kullanarak tüketilebilir:

- AMS SDK
- Microsoft Azure Depolama Gezgini (virgülle ayrılmış değer biçimine aktarmayı destekler ve Excel 'de işlenir)
- REST API

### <a name="how-to-find-average-bandwidth-consumption"></a>Ortalama bant genişliği tüketimini bulma

Ortalama bant genişliği tüketimi, zaman aralığı boyunca BytesSent ortalaması.

### <a name="how-to-define-streaming-unit-count"></a>Akış birim sayısı nasıl tanımlanır?

Akış birimi sayısı, hizmetin akış uç noktalarından en yüksek aktarım hızı olarak bir akış uç noktasının en yüksek aktarım hızı olarak tanımlanabilir. Bir akış uç noktasının en yüksek kullanılabilir verimi 160 MB/sn 'dir.
Örneğin, bir müşterinin hizmetinden en yüksek aktarım hızı değerinin 40 MBps olduğunu varsayalım (bir zaman aralığı boyunca BytesSent maksimum değeri). Ardından, akış birimi sayısı (40 MBps) * (8 bit/bayt)/(160 Mbps) = 2 akış birimi olarak eşittir.

### <a name="how-to-find-average-requestssecond"></a>Ortalama istek/saniye nasıl bulunur?

Ortalama istek sayısını/saniyeyi bulmak için, bir zaman aralığı boyunca ortalama istek sayısını (RequestCount) hesaplayın.

### <a name="how-to-define-channel-health"></a>Kanal sistem durumu nasıl tanımlanır?

Kanal sistem durumu, aşağıdaki koşullardan herhangi biri tutulmadığında false olması için bir bileşik Boole işlevi olarak tanımlanabilir:

- Fazla atlama sayısı > 0
- DiscontinuityCount > 0
- Nonıncreasingcount > 0
- Unalignedana kareleri = = true 
- UnalignedPresentationTime = = true 
- UnexpectedBitrate = = true


### <a name="how-to-detect-discontinuities"></a>Süreksizlik nasıl algılanır?

Süreksizlikleri algılamak için, DiscontinuityCount 0 > tüm kanal veri girişlerini bulun. Karşılık gelen ObservedTime zaman damgası, Boşların gerçekleştiği zamanları gösterir.

### <a name="how-to-detect-timestamp-overlaps"></a>Zaman damgası ile örtüşmeler nasıl algılanır?

Zaman damgası örtüşmelerini algılamak için, OverlapCount 0 > tüm kanal veri girişlerini bulun. Karşılık gelen ObservedTime zaman damgası, zaman damgasının çakıştığı zamanları gösterir.

### <a name="how-to-find-streaming-request-failures-and-reasons"></a>Akış isteği hatalarının ve nedenlerini bulma

Akış isteği başarısızlıklarını ve nedenlerini bulmak için, ResultCode 'un S_OK eşit olmadığı tüm akış uç noktası veri girdilerini bulun. Karşılık gelen StatusCode alanı, istek hatasının nedenini gösterir.

### <a name="how-to-consume-data-with-external-tools"></a>Dış araçlarla verileri kullanma

Telemetri verileri aşağıdaki araçlarla işlenebilir ve görselleştirilebilir:

- PowerBI
- Application Insights
- Azure Izleyici (eski adıyla Showebox)
- AMS canlı Pano
- Azure portalı (yayım bekleniyor)

### <a name="how-to-manage-data-retention"></a>Veri saklama nasıl yönetilir?

Telemetri sistemi, veri saklama yönetimi sağlamaz veya eski kayıtları otomatik olarak silmez. Bu nedenle, eski kayıtları depolama tablosundan el ile yönetmeniz ve silmeniz gerekir. Depolama SDK 'sına başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirim sağlayın

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
