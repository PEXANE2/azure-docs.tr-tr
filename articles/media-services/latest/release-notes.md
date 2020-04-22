---
title: Azure Medya Hizmetleri v3 sürüm notları | Microsoft Dokümanlar
description: En son gelişmelerden haberdar olmak için bu makale, Azure Medya Hizmetleri v3 ile ilgili en son güncelleştirmeleri sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: na
ms.topic: article
ms.date: 04/20/2020
ms.author: juliako
ms.openlocfilehash: b4849b4fbfdbaece46f5669f4c242e864b1ca533
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81769763"
---
# <a name="azure-media-services-v3-release-notes"></a>Azure Media Services v3 sürüm notları

>RsS özet akışı okuyucunuza bu URL'yi `https://docs.microsoft.com/api/search/rss?search=%22Azure+Media+Services+v3+release+notes%22&locale=en-us` kopyalayıp yapıştırarak güncellemeler için bu sayfayı ne zaman tekrar ziyaret edeceğiz konusunda bilgilendirilin.

En son gelişmelerden haberdar olmak için bu makalede, aşağıdakiler hakkında bilgi verilmektedir:

* En son sürümler
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlevsellik

## <a name="known-issues"></a>Bilinen sorunlar

> [!NOTE]
> [Azure portalını](https://portal.azure.com/) v3 Live [Events'i](live-events-outputs-concept.md)yönetmek, v3 [Varlıklarını](assets-concept.md)görüntülemek, API'lere erişim hakkında bilgi almak için kullanabilirsiniz. Diğer tüm yönetim görevleri için (örneğin, Dönüşümler ve İşler), [REST API,](https://aka.ms/ams-v3-rest-ref) [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK'lardan](media-services-apis-overview.md#sdks)birini kullanın.

Daha fazla bilgi için, [Medya Hizmetleri v2'den v3'e geçmek için Geçiş kılavuzuna](migrate-from-v2-to-v3.md#known-issues)bakın.

## <a name="april-2020"></a>Nisan 2020

### <a name="improvements-in-documentation"></a>Dokümantasyondaki gelişmeler

Azure Media Player dokümanları [Azure belgelerine](../azure-media-player/azure-media-player-overview.md)geçirildi.

## <a name="january-2020"></a>Ocak 2020

### <a name="improvements-in-media-processors"></a>Medya işlemcilerinde iyileştirmeler

- Video Analizi'nde iç içe geçmiş kaynaklar için geliştirilmiş destek – bu tür içerik, çıkarım motorlarına gönderilmeden önce artık doğru bir şekilde iç içe geçmişdurumda.
- "En Iyi" moduna sahip küçük resimler oluştururken, kodlayıcı artık tek renkli olmayan bir kare seçmek için 30 saniyenin üzerinde arama yapıyor.

### <a name="azure-government-cloud-updates"></a>Azure Devlet bulut güncellemeleri

Medya Hizmetleri GA'ed aşağıdaki Azure Devlet bölgelerinde: *USGov Arizona* ve *USGov Texas*.

## <a name="december-2019"></a>Aralık 2019

Hem canlı hem de isteğe bağlı video akışı için *Origin-Assist Prefetch* başlıkları için CDN desteği eklendi; Akamai CDN ile doğrudan sözleşme yapan müşteriler için kullanılabilir. Origin-Assist CDN-Prefetch özelliği, Akamai CDN ve Azure Media Services kökeni arasındaki aşağıdaki HTTP üstbilgi alışverişini içerir:

|HTTP üstbilgi|Değerler|Gönderen|Alıcı|Amaç|
| ---- | ---- | ---- | ---- | ----- |
|CDN-Origin-Assist-Prefetch-Enabled | 1 (varsayılan) veya 0 |CDN|Kaynak|CDN'nin ön alma etkin olduğunu belirtmek için|
|CDN-Origin-Assist-Prefetch-Path| Örnek: <br/>Parçalar(video=1400000000,format=mpd-time-cmaf)|Kaynak|CDN|CDN'ye ön alma yolu sağlamak için|
|CDN-Origin-Assist-Prefetch-İstek|1 (ön alma isteği) veya 0 (normal istek)|CDN|Kaynak|CDN'den gelen isteği belirtmek için bir ön getirme|

Üstbilgi değişiminin bir bölümünü çalışırken görmek için aşağıdaki adımları deneyebilirsiniz:

1. Bir ses veya video bölümü veya parçası için Medya Hizmetleri'ne istekte bulunmak için Postacı veya kıvırma kullanın. Üstbilgi CDN-Origin-Assist-Prefetch-Enabled: 1 istek eklediğinizden emin olun.
2. Yanıtta, üstbilgi CDN-Origin-Assist-Prefetch-Path değerini göreli bir yol ile görmeniz gerekir.

## <a name="november-2019"></a>Kasım 2019

### <a name="live-transcription-preview"></a>Canlı transkripsiyon Önizleme

Canlı transkripsiyon artık genel önizlemede ve Batı ABD 2 bölgesinde kullanıma sunulmuştur.

Canlı transkripsiyon, eklenti yeteneği olarak canlı etkinliklerle birlikte çalışacak şekilde tasarlanmıştır.  Hem geçiş hem de Standart veya Premium kodlama canlı etkinliklerinde desteklenir.  Bu özellik etkinleştirildiğinde, hizmet, gelen sesteki konuşulan sözcükleri metne dönüştürmek için Bilişsel Hizmetler'in [Metinden Metine Konuşma](../../cognitive-services/speech-service/speech-to-text.md) özelliğini kullanır. Bu metin daha sonra MPEG-DASH ve HLS protokollerinde video ve ses ile birlikte teslim için kullanılabilir hale getirilir. Faturalandırma, "Çalışan" durumundayken canlı etkinliğe ek maliyet olan yeni bir eklenti ölçere dayanır.  Canlı transkripsiyon ve faturalandırma hakkında ayrıntılı bilgi için [Canlı transkripsiyon](live-transcription.md)

> [!NOTE]
> Şu anda, canlı transkripsiyon yalnızca Batı ABD 2 bölgesinde önizleme özelliği olarak kullanılabilir. Sadece şu anda İngilizce (en-us) konuşulan kelimelerin transkripsiyonu destekler.

### <a name="content-protection"></a>İçerik koruma

Eylül ayında sınırlı bölgelerde yayımlanan *Token Tekrar Önleme* özelliği artık tüm bölgelerde kullanılabilir.
Medya Hizmetleri müşterileri artık aynı belirteç bir anahtar veya lisans istemek için kaç kez kullanılabilir bir sınır belirleyebilirsiniz. Daha fazla bilgi için [Token Replay Prevention](content-protection-overview.md#token-replay-prevention)'a bakın.

### <a name="new-recommended-live-encoder-partners"></a>Yeni önerilen canlı kodlayıcı ortakları

RTMP canlı akışı için aşağıdaki yeni önerilen iş ortağı kodlayıcıları için destek eklendi:

- [Cambria Canlı 4.3](https://www.capellasystems.net/products/cambria-live/)
- [GoPro Hero7/8 ve Max aksiyon kameraları](https://gopro.com/help/articles/block/getting-started-with-live-streaming)
- [Restream.io](https://restream.io/)

### <a name="file-encoding-enhancements"></a>Dosya Kodlama geliştirmeleri

- Yeni Bir İçerik Farkında Kodlama ön ayar artık kullanılabilir. İçerik duyarlı kodlama kullanarak GOP uyumlu bir MP4 kümesi üretir. Herhangi bir giriş içeriği göz önüne alındığında, hizmet giriş içeriğinin ilk hafif analizini gerçekleştirir. Bu sonuçları, uyarlamalı akışla teslim için en uygun katman sayısını, uygun bit hızını ve çözünürlük ayarlarını belirlemek için kullanır. Bu ön ayar, özellikle düşük karmaşıklık ve orta karmaşıklıkta videolar için etkilidir, çıktı dosyaları daha düşük bit hızlarında dır, ancak yine de görüntüleyenlere iyi bir deneyim sunan bir kalitededir. Çıkış video ve ses interleaved mp4 dosyaları içerecektir. Daha fazla bilgi için [açık API özelliklerine](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json)bakın.
- Standard Encoder'daki yeniden boyutlandırma için geliştirilmiş performans ve çoklu iş parçacığı. Belirli koşullar altında, müşteri% 5-40 VOD kodlama arasında bir performans artışı görmelisiniz. Birden çok bit oranlarına kodlanmış düşük karmaşıklık içeriği en yüksek performans artışlarını görür. 
- Standart kodlama şimdi zaman tabanlı GOP ayarı kullanırken VOD kodlama sırasında değişken kare hızı (VFR) içeriği için düzenli bir GOP cadence korur.  Bu, örneğin 15-30 fps arasında değişen karışık kare hızı içeriği gönderen müşterinin artık uyarlanabilir bitrate akış MP4 dosyaları için çıktı hesaplanan normal GOP mesafeleri görmek gerektiği anlamına gelir. Bu hls veya DASH üzerinden teslim ederken parça arasında sorunsuz geçiş yeteneğini artıracaktır. 
-  Değişken kare hızı (VFR) kaynak içeriği için geliştirilmiş AV senkronizasyonu

### <a name="video-indexer-video-analytics"></a>Video Dizinleyici, Video analizi

- VideoAnalyzer ön kümesi kullanılarak çıkarılan anahtar kareler artık yeniden boyutlandırılmaktansa videonun orijinal çözünürlüğündedir. Yüksek çözünürlüklü anahtar kare çıkarma, orijinal kalitede görüntüler sunar ve videonuzdan daha fazla bilgi edinmek için Microsoft Computer Vision ve Custom Vision hizmetleri tarafından sağlanan görüntü tabanlı yapay zeka modellerinden yararlanmanızı sağlar.

## <a name="september-2019"></a>Eylül 2019

###  <a name="media-services-v3"></a>Media Services v3  

#### <a name="live-linear-encoding-of-live-events"></a>Canlı olayların canlı doğrusal kodlaması

Medya Hizmetleri v3 canlı olayların canlı doğrusal kodlama 24 saat x 365 gün önizleme duyurdu.

###  <a name="media-services-v2"></a>Media Services v2  

#### <a name="deprecation-of-media-processors"></a>Ortam işlemcilerinin amortismanı

*Azure Media Indexer* ve Azure Media *Indexer 2 Preview'un*amortismanını duyuruyoruz. Emeklilik tarihleri [için, eski bileşenler](../previous/legacy-components.md) konusuna bakın. [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) bu eski medya işlemcilerinin yerini alır.

Daha fazla bilgi için azure [media indexer ve Azure Media Indexer 2'den Azure Media Services Video Indexer'a geçir'e](../previous/migrate-indexer-v1-v2.md)bakın.

## <a name="august-2019"></a>Ağustos 2019

###  <a name="media-services-v3"></a>Media Services v3  

#### <a name="south-africa-regional-pair-is-open-for-media-services"></a>Güney Afrika bölgesel çifti Medya Hizmetleri için açık 

Medya Hizmetleri artık Güney Afrika Kuzey ve Güney Afrika Batı bölgelerinde kullanılabilir.

Daha fazla bilgi için [bkz.](azure-clouds-regions.md)

###  <a name="media-services-v2"></a>Media Services v2  

#### <a name="deprecation-of-media-processors"></a>Ortam işlemcilerinin amortismanı

Kullanımdan kaldırılan *Windows Azure Media Encoder* (WAME) ve Azure Media *Encoder* (AME) medya işlemcilerinin amortismanını duyururuz. Emeklilik tarihleri için bu [eski bileşenler](../previous/legacy-components.md) konusuna bakın.

Ayrıntılar için [WAME'yi Media Encoder Standardına Geçirve](https://go.microsoft.com/fwlink/?LinkId=2101334) [AME'yi Media Encoder Standardına Geçirin'](https://go.microsoft.com/fwlink/?LinkId=2101335)e bakın.
 
## <a name="july-2019"></a>Temmuz 2019

### <a name="content-protection"></a>İçerik koruma

Belirteç kısıtlamasıyla korunan içerik akışı nda, son kullanıcıların anahtar teslim isteğinin bir parçası olarak gönderilen bir belirteci edinmeleri gerekir. *Token Yeniden Oynatma Önleme* özelliği, Medya Hizmetleri müşterilerinin bir anahtar veya lisans istemek için aynı belirteçlerin kaç kez kullanılabileceğini belirlemesine olanak tanır. Daha fazla bilgi için [Token Replay Prevention](content-protection-overview.md#token-replay-prevention)'a bakın.

Temmuz ayı itibariyle önizleme özelliği yalnızca US Central ve US West Central'da kullanılabilir.

## <a name="june-2019"></a>Haziran 2019

### <a name="video-subclipping"></a>Video altyazı

Artık bir [İş'i](https://docs.microsoft.com/rest/api/media/jobs)kullanarak kodlarken videoyu kırpabilir veya alt klibi kesebilirsiniz. 

Bu [işlevsellik, BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) hazır ayarları veya [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) hazır ayarları kullanılarak oluşturulan herhangi bir [Transform](https://docs.microsoft.com/rest/api/media/transforms) ile çalışır. 

Örneklere bakın:

* [.NET ile bir video alt klip](subclip-video-dotnet-howto.md)
* [REST ile bir video nun alt klibi](subclip-video-rest-howto.md)

## <a name="may-2019"></a>Mayıs 2019

### <a name="azure-monitor-support-for-media-services-diagnostic-logs-and-metrics"></a>Medya Hizmetleri tanı lama günlükleri ve ölçümleri için Azure Monitör desteği

Artık Medya Hizmetleri tarafından yayılan telemetri verilerini görüntülemek için Azure Monitor'u kullanabilirsiniz.

* Medya Hizmetleri Anahtar Teslim bitiş noktası tarafından gönderilen istekleri izlemek için Azure Monitörü tanı günlüklerini kullanın. 
* Medya Hizmetleri Akış Bitiş Noktaları tarafından yayılan [ölçümleri](streaming-endpoint-concept.md)izleyin.   

Ayrıntılar için Bkz. [Medya Hizmetleri ölçümlerini ve tanı günlüklerini izleyin.](media-services-metrics-diagnostic-logs.md)

### <a name="multi-audio-tracks-support-in-dynamic-packaging"></a>Dinamik Ambalaj'da çoklu ses parçaları desteği 

Birden çok codec ve dil içeren birden çok ses parçası olan Varlıkları akışalırken, [Dinamik Ambalaj](dynamic-packaging-overview.md) artık HLS çıkışı için birden fazla ses parçasını (sürüm 4 veya üzeri) destekler.

### <a name="korea-regional-pair-is-open-for-media-services"></a>Kore bölgesel çifti Medya Hizmetleri için açık 

Medya Hizmetleri artık Kore Orta ve Kore Güney bölgelerinde kullanılabilir. 

Daha fazla bilgi için [bkz.](azure-clouds-regions.md)

### <a name="performance-improvements"></a>Performans iyileştirmeleri

Medya Hizmetleri performans iyileştirmeleri içeren güncelleştirmeler eklendi.

* İşlem için desteklenen maksimum dosya boyutu güncelleştirildi. Bakınız, [Kotalar ve limitler.](limits-quotas-constraints.md)
* [Kodlama hızları iyileştirmeler.](media-reserved-units-cli-how-to.md#choosing-between-different-reserved-unit-types)

## <a name="april-2019"></a>Nisan 2019

### <a name="new-presets"></a>Yeni ön ayarlar

* [FaceDetectorPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#facedetectorpreset) dahili analizör ön ayarlarına eklendi.
* [ContentAwareEncodingExperimental](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset) dahili kodlayıcı ön ayarlarına eklendi. Daha fazla bilgi [için, İçerik bilgisine duyarlı kodlamaya](content-aware-encoding.md)bakın. 

## <a name="march-2019"></a>Mart 2019

Dinamik Ambalaj artık Dolby Atmos'u destekliyor. Daha fazla bilgi için dinamik [ambalajlarla desteklenen Ses kodlayıcılarına](dynamic-packaging-overview.md#audio-codecs)bakın.

Artık, Akış Bulucunuz için geçerli olacak varlık veya hesap filtrelerinin listesini belirtebilirsiniz. Daha fazla bilgi için, [Akış Lı Konumlat ile Filtreleri Ilişkilendir](filters-concept.md#associating-filters-with-streaming-locator)bölümüne bakın.

## <a name="february-2019"></a>Şubat 2019

Media Services v3 artık Azure ulusal bulutlarında desteklendi. Tüm özellikler henüz tüm bulutlarda kullanılamıyor. Ayrıntılar için, [Azure Medya Hizmetleri v3'ün bulunduğu Bulutlar ve bölgelere](azure-clouds-regions.md)bakın.

[Microsoft.Media.JobOutputProgress](media-services-event-schemas.md#monitoring-job-output-progress) etkinliği, Medya Hizmetleri için Azure Olay Izgara şemalarına eklendi.

## <a name="january-2019"></a>Ocak 2019

### <a name="media-encoder-standard-and-mpi-files"></a>Media Encoder Standart ve MPI dosyaları 

MP4 dosyası(lar) oluşturmak için Media Encoder Standard ile kodlama yaparken, yeni bir .mpi dosyası oluşturulur ve çıktı Varlığına eklenir. Bu MPI dosyası, dinamik [paketleme](dynamic-packaging-overview.md) ve akış senaryoları için performansı artırmak için tasarlanmıştır.

MPI dosyasını değiştirmemeli veya kaldırmamalısınız veya hizmetinizde böyle bir dosyanın varlığına (veya olmadığına) herhangi bir bağımlılık yapmamalısınız.

## <a name="december-2018"></a>Aralık 2018

V3 API GA sürümünden güncelleştirmeler şunlardır:
       
* **PresentationTimeRange** özellikleri artık **Varlık Filtreleri** ve **Hesap Filtreleri**için 'gerekli' değildir. 
* **İşler** ve **Dönüşümler** için $top ve $skip sorgu seçenekleri kaldırıldı ve $orderby eklendi. Yeni sipariş işlevini eklemenin bir parçası olarak, $top ve $skip seçeneklerinin uygulanmamış olsalar bile yanlışlıkla daha önce maruz kaldığı keşfedildi.
* Numaralandırma genişletilebilirliği yeniden etkinleştirildi. Bu özellik SDK'nın önizleme sürümlerinde etkinleştirildi ve GA sürümünde yanlışlıkla devre dışı bırakıldı.
* Önceden tanımlanmış iki akış ilkesi yeniden adlandırıldı. **SecureStreaming** şimdi **MultiDrmCencStreaming**olduğunu. **SecureStreamingWithFairPlay** şimdi **Predefined_MultiDrmStreaming.**

## <a name="november-2018"></a>Kasım 2018

CLI 2.0 modülü artık [Azure Media Services v3 GA](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest) - v 2.0.50 için kullanılabilir.

### <a name="new-commands"></a>Yeni komutlar

- [az ams hesabı](https://docs.microsoft.com/cli/azure/ams/account?view=azure-cli-latest)
- [az ams hesap filtresi](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest)
- [az ams varlık](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest)
- [az ams varlık filtresi](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)
- [az ams içerik-anahtar-ilke](https://docs.microsoft.com/cli/azure/ams/content-key-policy?view=azure-cli-latest)
- [az ams iş](https://docs.microsoft.com/cli/azure/ams/job?view=azure-cli-latest)
- [az ams canlı olay](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)
- [az ams canlı çıktı](https://docs.microsoft.com/cli/azure/ams/live-output?view=azure-cli-latest)
- [az ams streaming-endpoint](https://docs.microsoft.com/cli/azure/ams/streaming-endpoint?view=azure-cli-latest)
- [az ams streaming-locator](https://docs.microsoft.com/cli/azure/ams/streaming-locator?view=azure-cli-latest)
- [az ams hesabı mru](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) - Medya Ayrılmış Birimleri yönetmenize olanak sağlar. Daha fazla bilgi için Bkz. [Ölçek Medya Ayrılmış Birimler.](media-reserved-units-cli-how-to.md)

### <a name="new-features-and-breaking-changes"></a>Yeni özellikler ve kırılma değişiklikleri

#### <a name="asset-commands"></a>Varlık komutları

- ```--storage-account```ve ```--container``` argümanlar eklendi.
- Son kullanma süresi için varsayılan değerler (Şimdi+23h) ```az ams asset get-sas-url``` ve izinler (Read) komutu eklendi.

#### <a name="job-commands"></a>İş komutları

- ```--correlation-data```ve ```--label``` eklenen argümanlar
- ```--output-asset-names```olarak yeniden ```--output-assets```adlandırıldı. Şimdi 'assetName=label' biçiminde varlıkların boşluk ayrılmış bir listesini kabul eder. Etiketi olmayan bir varlık şu şekilde gönderilebilir: 'assetName='.

#### <a name="streaming-locator-commands"></a>Akış Lı Konumbelirleme komutları

- ```az ams streaming locator```taban komutu ```az ams streaming-locator```ile değiştirilir.
- ```--streaming-locator-id```ve ```--alternative-media-id support``` argümanlar eklendi.
- ```--content-keys argument```bağımsız değişken güncellendi.
- ```--content-policy-name```olarak yeniden ```--content-key-policy-name```adlandırıldı.

#### <a name="streaming-policy-commands"></a>Akış İlkesi komutları

- ```az ams streaming policy```taban komutu ```az ams streaming-policy```ile değiştirilir.
- ```az ams streaming-policy create``` Şifreleme parametreleri eklendi desteği.

#### <a name="transform-commands"></a>Komutları dönüştürme

- ```--preset-names```bağımsız değişken ```--preset```i ' ile değiştirilir. Artık yalnızca bir seferde 1 çıktı/önceden ayarlanmış (çalıştırmak ```az ams transform output add```için daha fazla eklemek için) ayarlayabilirsiniz. Ayrıca, özel JSON için yol geçerek özel StandardEncoderPreset ayarlayabilirsiniz.
- ```az ams transform output remove```çıkarmak için çıkış dizini geçerek gerçekleştirilebilir.
- ```--relative-priority, --on-error, --audio-language and --insights-to-extract```bağımsız ```az ams transform create``` değişkenler ```az ams transform output add``` eklendi ve komutlar.

## <a name="october-2018---ga"></a>Ekim 2018 - GA

Bu bölümde Azure Medya Hizmetleri (AMS) Ekim güncelleştirmeleri açıklanmaktadır.

### <a name="rest-v3-ga-release"></a>REST v3 GA sürümü

[REST v3 GA sürümü,](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) Live, Account/Asset level manifest filtreleri ve DRM desteği için daha fazla API içerir.

#### <a name="azure-resource-management"></a>Azure Kaynak Yönetimi 

Azure Kaynak Yönetimi desteği, birleşik yönetim ve operasyon API'sine (şimdi her şey tek bir yerde) olanak sağlar.

Bu sürümden başlayarak, Canlı Etkinlikler oluşturmak için Kaynak Yöneticisi şablonlarını kullanabilirsiniz.

#### <a name="improvement-of-asset-operations"></a>Varlık operasyonlarının iyileştirilmesi 

Aşağıdaki iyileştirmeler tanıtıldı:

- HTTP(ler) URL'lerinden veya Azure Blob Depolama URL'lerinden sindirin.
- Varlıklar için kapsayıcı adlarını belirtin. 
- Azure İşlevleri ile özel iş akışları oluşturmak için daha kolay çıktı desteği.

#### <a name="new-transform-object"></a>Yeni Dönüştür nesnesi

Yeni **Dönüştür** nesnesi Kodlama modelini basitleştirir. Yeni nesne, kodlama Kaynak Yöneticisi şablonları ve hazır ayarları oluşturmayı ve paylaşmayı kolaylaştırır. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Azure Etkin Dizin kimlik doğrulaması ve RBAC

Azure AD Kimlik Doğrulaması ve Role Dayalı Erişim Denetimi (RBAC), Azure AD'deki Role göre güvenli Dönüşümler, LiveEvents'ler, İçerik Anahtar İlkeleri veya Varlıklar sağlar.

#### <a name="client-sdks"></a>İstemci SDK'ları  

Medya Hizmetleri v3 desteklenen diller: .NET Core, Java, Node.js, Ruby, Typescript, Python, Go.

#### <a name="live-encoding-updates"></a>Canlı kodlama güncellemeleri

Aşağıdaki canlı kodlama güncelleştirmeleri tanıtıldı:

- Canlı (10 saniye uç-uç) için yeni düşük gecikme modu.
- Geliştirilmiş RTMP desteği (artırılmış kararlılık ve daha fazla kaynak kodlayıcı desteği).
- RTMPS güvenli yutmak.

    Bir Canlı Etkinlik oluşturduğunuzda, artık 4 yutma URL'si elde elabilirsiniz. 4 yutma URL'leri hemen hemen aynıdır, aynı akış belirteci (AppId) var, sadece bağlantı noktası numarası bölümü farklıdır. URL'lerden ikisi RTMPS için birincil ve yedektir. 
- 24 saat kodlama desteği. 
- SCTE35 ile RTMP'de geliştirilmiş reklam sinyaldesteği.

#### <a name="improved-event-grid-support"></a>Geliştirilmiş Olay Grid desteği

Aşağıdaki Olay Izgara destek geliştirmelerini görebilirsiniz:

- Logic Apps ve Azure Fonksiyonları ile daha kolay geliştirme için Azure Olay Izgara tümleştirmesi. 
- Kodlama, Canlı Kanallar ve daha fazlası etkinlikleri için abone olun.

### <a name="cmaf-support"></a>CMAF desteği

CMAF'ı destekleyen Apple HLS (iOS 11+) ve MPEG-DASH oynatıcılar için CMAF ve 'cbcs' şifreleme desteği.

### <a name="video-indexer"></a>Video Indexer

Video Indexer GA sürümü Ağustos ayında açıklandı. Şu anda desteklenen özellikler hakkında yeni bilgiler için [Video Dizinleyici nedir'e](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json)bakın. 

### <a name="plans-for-changes"></a>Değişiklik planları

#### <a name="azure-cli-20"></a>Azure CLI 2.0
 
Tüm özellikler (Live, Content Key İlkeleri, Hesap/Varlık Filtreleri, Akış İlkeleri dahil) işlemlerini içeren Azure CLI 2.0 modülü yakında geliyor. 

### <a name="known-issues"></a>Bilinen sorunlar

Yalnızca Varlık veya Hesap Filtreleri için önizleme API'sini kullanan müşteriler aşağıdaki sorundan etkilenir.

Medya Hizmetleri v3 CLI veya API'lerle 09/28 ve 10/12 tarihleri arasında Varlıklar veya Hesap Filtreleri oluşturduysanız, tüm Varlık ve Hesap Filtreleri'ni kaldırmanız ve bir sürüm çakışması nedeniyle bunları yeniden oluşturmanız gerekir. 

## <a name="may-2018---preview"></a>Mayıs 2018 - Önizleme

### <a name="net-sdk"></a>.NET SDK

.NET SDK'da aşağıdaki özellikler mevcuttur:

* Medya içeriğini kodlamak veya analiz etmek için **Dönüşümler** ve **İşler.** Örneğin, [Bkz. Akış dosyaları](stream-files-tutorial-with-api.md) ve [Çözümle.](analyze-videos-tutorial-with-api.md)
* Son kullanıcı aygıtlarına içerik yayınlamak ve aktarmak için **Konum belirlemeleri** akışı
* İçerik teslim ederken anahtar teslimini ve içerik korumasını (DRM) yapılandırmak için **Akış İlkeleri** ve **İçerik Anahtarı İlkeleri.**
* **Canlı** akış içeriğinin yutulmasını ve arşivletilmesi için Canlı Etkinlikler ve **Canlı Çıktılar.**
* Medya içeriğini Azure Depolama'da depolamak ve yayımlamak için **varlıklar.** 
* Hem canlı hem de isteğe bağlı medya içeriği için dinamik paketleme, şifreleme ve akış yapılandırmak ve ölçeklendirmek için **Uç Noktaları** akışı.

### <a name="known-issues"></a>Bilinen sorunlar

* Bir iş gönderirken, HTTPS URL'lerini, SAS URL'lerini veya Azure Blob depolama alanında bulunan dosyalara giden yolları kullanarak kaynak videonuzu yutmayı belirtebilirsiniz. AMS v3 şu anda HTTPS URL'leri üzerinden yığın halinde aktarım kodlamasını desteklememektedir.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

- [Genel bakış](media-services-overview.md)
- [Medya Hizmetleri v3 Dokümantasyon güncellemeleri](docs-release-notes.md)
- [Medya Hizmetleri v2 sürüm notları](../previous/media-services-release-notes.md)
