---
title: Azure Medya Hizmetleri Telemetri | Microsoft Dokümanlar
description: Bu makalede, Microsoft Azure Medya Hizmetleri telemetrisi hakkında genel bir bakış sağlanmaktadır.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74895785"
---
# <a name="azure-media-services-telemetry"></a>Azure Medya Hizmetleri telemetrisi  


> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Azure Medya Hizmetleri (AMS), hizmetleri için telemetri/metrik verilerine erişmenizi sağlar. AMS'in geçerli sürümü, canlı **Kanal**, **StreamingEndpoint**ve canlı **Arşiv** varlıkları için telemetri verileri toplamanızı sağlar. 

Telemetri, belirttiğiniz bir Azure Depolama hesabındaki bir depolama tablosuna yazılır (genellikle AMS hesabınızla ilişkili depolama hesabını kullanırsınız). 

Telemetri sistemi veri saklamayı yönetmez. Depolama tablolarını silerek eski telemetri verilerini kaldırabilirsiniz.

Bu konu, AMS telemetrisinin nasıl yapılandırılabildiğini ve tüketilenleri tartışır.

## <a name="configuring-telemetry"></a>Telemetrinin yapılandırılması

Bir bileşen düzeyinde tanecikli üzerinde telemetri yapılandırabilirsiniz. "Normal" ve "Verbose" olmak için iki ayrıntı düzeyi vardır. Şu anda, her iki düzey de aynı bilgileri döndürebilir. Bu "Normal kullanılması tavsiye edilir. 

Aşağıdaki konular telemetrinin nasıl etkinleştirilen gösteriş:

[.NET ile telemetriyi etkinleştirme](media-services-dotnet-telemetry.md) 

[REST ile telemetriyi etkinleştirme](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Telemetri bilgilerini tüketme

Telemetri, Medya Hizmetleri hesabı için telemetri yapılandırırken belirttiğiniz depolama hesabındaki bir Azure Depolama Tablosuna yazılır. Bu bölümde ölçümler için depolama tabloları açıklanmaktadır.

Telemetri verilerini aşağıdaki yollardan birinde tüketebilirsiniz:

- Verileri doğrudan Azure Tablo Depolama'dan okuyun (örn. Depolama SDK'sını kullanma). Telemetri depolama tablolarının açıklaması için [bu](https://msdn.microsoft.com/library/mt742089.aspx) **konudaki Tüketen telemetri bilgilerine** bakın.

Veya

- [Bu](media-services-dotnet-telemetry.md) konuda açıklandığı gibi depolama verilerini okumak için Medya Hizmetleri .NET SDK'daki desteği kullanın. 


Aşağıda açıklanan telemetri şeması, Azure Tablo Depolama sınırları içinde iyi performans sağlamak üzere tasarlanmıştır:

- Veriler, her hizmetten telemetrinin bağımsız olarak sorgulanmasına izin vermek için hesap kimliği ve hizmet kimliğiyle bölümlenir.
- Bölümler, bölüm boyutunda makul bir üst sınır vermek için tarih içerir.
- Satır anahtarları, en son telemetri öğelerinin belirli bir hizmet için sorgulanmasına izin vermek için ters zaman sırasına göre dir.

Bu, sık karşılaşılan sorguların çoğunun verimli olmasını sağlar:

- Paralel, ayrı hizmetler için veri bağımsız indirme.
- Belirli bir hizmetiçin tüm verileri tarih aralığında alma.
- Bir hizmet için en son verileri alma.

### <a name="telemetry-table-storage-output-schema"></a>Telemetri tablo depolama çıkış şeması

Telemetri verileri, oluşturulan tablonun "20160321" tarihini içeren "TelemetryMetrics20160321" adlı bir tabloda toplu olarak saklanır. Telemetri sistemi, UTC'de 00:00'da her yeni gün için ayrı bir tablo oluşturur. Tablo, belirli bir zaman penceresinde bitrate yutma, gönderilen baytlar vb. gibi yinelenen değerleri depolamak için kullanılır. 

Özellik|Değer|Örnekler/notlar
---|---|---
PartitionKey|{hesap kimliği}_{entity ID}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66<br/<br/>Hesap kimliği, birden çok Medya Hizmeti hesabının aynı depolama hesabına yazıldığı iş akışlarını basitleştirmek için bölüm anahtarına dahildir.
RowKey|{saniye den gece yarısına}_{rasgele değer}|01688_00199<br/><br/>Satır tuşu, bir bölüm içindeki en üst n stili sorgularına izin vermek için gece yarısına saniye sayısıyla başlar. Daha fazla bilgi için [bu makaleye](../../cosmos-db/table-storage-design-guide.md#log-tail-pattern) bakın. 
Zaman damgası|Tarih/Saat|Azure tablosundan otomatik zaman damgası 2016-09-09T22:43:42.241Z
Tür|Telemetri verileri sağlayan varlığın türü|Kanal/StreamingEndpoint/Arşiv<br/><br/>Olay türü sadece bir dize değeridir.
Adı|Telemetri olayının adı|KanalKalp Atışı/StreamingEndpointRequestLog
Gözlenen Zaman|Telemetri olayının oluştuğu saat (UTC)|2016-09-09T22:42:36.924Z<br/><br/>Gözlenen süre, telemetriyi gönderen varlık tarafından sağlanır (örneğin bir kanal). Bileşenler arasında zaman eşitleme sorunları olabilir, bu nedenle bu değer yaklaşık
ServiceID|{hizmet kimliği}|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Varlığa özgü özellikler|Olay tarafından tanımlandığı gibi|StreamName: stream1, Bitrate 10123, ...<br/><br/>Kalan özellikler verilen olay türü için tanımlanır. Azure Tablo içeriği anahtar değer çiftleridir.  (diğer bir deyişle, tablodaki farklı satırlar farklı özellik kümelerine sahiptir).

### <a name="entity-specific-schema"></a>Varlığa özgü şema

Her biri aşağıdaki frekansla itilen varlığa özgü telemetrik veri girişlerinin üç türü vardır:

- Akış uç noktaları: Her 30 saniyede bir
- Canlı kanallar: Her dakika
- Canlı arşiv: Her dakika

**Akış Bitiş Noktası**

Özellik|Değer|Örnekler
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Zaman damgası|Zaman damgası|Azure Tablodan otomatik zaman damgası 2016-09-09T22:43:42.241Z
Tür|Tür|AkışBitiş Noktası
Adı|Adı|StreamingEndpointRequestLog
Gözlenen Zaman|Gözlenen Zaman|2016-09-09T22:42:36.924Z
ServiceID|Hizmet Kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Ana bilgisayar adı|Bitiş noktasının ana bilgisayarı adı|builddemoserver.origin.mediaservices.windows.net
Statuscode|Kayıtları HTTP durumu|200
Resultcode|Sonuç kodu detayı|S_OK
İstek Sayısı|Toplamada toplam istek|3
Baytgönderildi|Gönderilen toplu baytlar|2987358
ServerLatency|Ortalama sunucu gecikmesi (depolama alanı dahil)|129
E2ELatency|Ortalama uçuçgecikme|250

**Canlı kanal**

Özellik|Değer|Örnekler/notlar
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Zaman damgası|Zaman damgası|Azure tablosundan otomatik zaman damgası 2016-09-09T22:43:42.241Z
Tür|Tür|Kanal
Adı|Adı|KanalKalp Atışı
Gözlenen Zaman|Gözlenen Zaman|2016-09-09T22:42:36.924Z
ServiceID|Hizmet Kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|Parça video/ses/metin türü|video/ses
Parça Adı|Parçanın adı|video/audio_1
Bitrate|Bit hızını izleme|785000
Customattributes||   
GelenBitrate|Gerçek gelen bithızı|784548
Çakışma Sayısı|Yutulmasında çakışma|0
Süreksizlik Sayısı|Parça için süreksizlik|0
Son Zaman Damgası|Son yutulan veri zaman damgası|1800488800
Artmayan Sayı|Artan zaman damgası nedeniyle atılan parçaların sayısı|2
UnalignedKeyFrames|Anahtar çerçevelerin hizalanmadığı parça(lar) (kalite düzeyleri arasında) alıp almadığımız |True
UnalignedPresentationTime|Sunum süresinin hizalanmadığı parça(lar) (kalite düzeyleri/parçaları arasında) alıp almadığımız|True
BeklenmeyenBitrate|Doğru, 40.000 bps ve IncomingBitrate > ses/video izleme için hesaplanan /gerçek bitrate == 0 VEYA IncomingBitrate ve actualBitrate% 50 farklı |True
Sağlam|True <br/>çakışmaSayısı, <br/>Süreksizlik Sayısı, <br/>Artmayan Sayısı, <br/>UnalignedKeyFrames, <br/>UnalignedPresentationTime, <br/>BeklenmeyenBitrate<br/> tüm 0 vardır|True<br/><br/>Sağlıklı, aşağıdaki koşullardan herhangi biri tutarken false döndüren bileşik bir işlevdir:<br/><br/>- Çakışma Sayısı > 0<br/>- Süreksizlik Sayısı > 0<br/>- Artmayan Sayı > 0<br/>- UnalignedKeyFrames == Doğru<br/>- UnalignedPresentationTime == Doğru<br/>- BeklenmeyenBitrate == Doğru

**Canlı arşiv**

Özellik|Değer|Örnekler/notlar
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
Zaman damgası|Zaman damgası|Azure tablosundan otomatik zaman damgası 2016-09-09T22:43:42.241Z
Tür|Tür|Arşiv
Adı|Adı|ArşivHeartbeat
Gözlenen Zaman|Gözlenen Zaman|2016-09-09T22:42:36.924Z
ServiceID|Hizmet Kimliği|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ManifestName|Program url'si|varlık-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4bd2-8c01-a92a2b38c9ba.ism
Parça Adı|Parçanın adı|audio_1
TrackType|Parçanın türü|Ses / video
CustomAttribute|Aynı ad ve bit hızı (çoklu kamera açısı) ile farklı parça arasında ayrım yapan Hex dizesi|
Bitrate|Bit hızını izleme|785000
Sağlam|Doğru, eğer FragmentDiscardedCount == 0 && ArchiveAcquisitionError == False|True (bu iki değer metrikte bulunmaz, ancak kaynak olayda bulunur)<br/><br/>Sağlıklı, aşağıdaki koşullardan herhangi biri tutarken false döndüren bileşik bir işlevdir:<br/><br/>- FragmentDiscardedSayısı > 0<br/>- ArchiveAcquisitionError == True

## <a name="general-qa"></a>Genel Q&A

### <a name="how-to-consume-metrics-data"></a>Metrik verileri nasıl tüketilir?

Metrik veriler müşterinin depolama hesabında bir dizi Azure Tabloları olarak depolanır. Bu veriler aşağıdaki araçlar kullanılarak tüketilebilir:

- AMS SDK
- Microsoft Azure Depolama Gezgini (virgülle ayrılmış değer biçimine dışa aktarmayı destekler ve Excel'de işlenir)
- REST API

### <a name="how-to-find-average-bandwidth-consumption"></a>Ortalama bant genişliği tüketimi nasıl bulunur?

Ortalama bant genişliği tüketimi, BytesSent'in bir zaman aralığındaki ortalamasıdır.

### <a name="how-to-define-streaming-unit-count"></a>Akış birimi sayısı nasıl tanımlanır?

Akış birimi sayısı, hizmetin akış uç noktalarından bir akış bitiş noktasının en yüksek iş akışına bölünen tepe iş akışı olarak tanımlanabilir. Bir akış uç noktasının kullanılabilir en yüksek iş akışı 160 Mbps'dir.
Örneğin, bir müşterinin hizmetinden elde edilen en yüksek verimin 40 MBps olduğunu varsayalım (bir süre içinde BytesSent'in maksimum değeri). Daha sonra, akış birimi sayısı (40 MBps)*(8 bit/bayt)/(160 Mbps) = 2 akış birimine eşittir.

### <a name="how-to-find-average-requestssecond"></a>Ortalama istekler/saniye nasıl bulunur?

Ortalama istek/saniye sayısını bulmak için, bir zaman aralığında ortalama istek sayısını (RequestCount) hesapla.

### <a name="how-to-define-channel-health"></a>Kanal durumu nasıl tanımlanır?

Kanal durumu, aşağıdaki koşullardan herhangi biri tutarken yanlış olan bileşik boolean işlevi olarak tanımlanabilir:

- Çakışma Sayısı > 0
- SüreksizlikSayısı > 0
- Artmayan Sayı > 0
- UnalignedKeyFrames == Doğru 
- UnalignedPresentationTime == Doğru 
- BeklenmeyenBitrate == Doğru


### <a name="how-to-detect-discontinuities"></a>Süreksizlikler nasıl algılanır?

Süreksizlikleri algılamak için, Süreksizlik Sayısının 0'> tüm Kanal veri girişlerini bulun. İlgili ObservedTime zaman damgası, süreksizliklerin oluştuğu zamanı gösterir.

### <a name="how-to-detect-timestamp-overlaps"></a>Zaman damgası çakışmaları nasıl algılanır?

Zaman damgası çakışmalarını algılamak için, Çakışma Sayısının 0'> tüm Kanal veri girişlerini bulun. İlgili ObservedTime zaman damgası, zaman damgasının çakışan zamanları gösterir.

### <a name="how-to-find-streaming-request-failures-and-reasons"></a>Akış isteği hataları ve nedenleri nasıl bulunur?

Akış isteği hatalarını ve nedenlerini bulmak için, ResultCode'un S_OK eşit olmadığı tüm Akış Uç Nokta veri girişlerini bulun. İlgili StatusCode alanı istek hatasının nedenini gösterir.

### <a name="how-to-consume-data-with-external-tools"></a>Harici araçlarla veri nasıl tüketilir?

Telemetrik veriler aşağıdaki araçlarla işlenebilir ve görselleştirilebilir:

- PowerBI
- Application Insights
- Azure Monitör (eski adıyla Shoebox)
- AMS Canlı Pano
- Azure Portalı (sürüm bekleyen)

### <a name="how-to-manage-data-retention"></a>Veri saklama nasıl yönetilir?

Telemetri sistemi veri saklama yönetimi veya eski kayıtların otomatik silme sağlamaz. Bu nedenle, eski kayıtları depolama tablosundan el ile yönetmeniz ve silmeniz gerekir. Nasıl yapılacağınız için depolama SDK'ya başvurabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
