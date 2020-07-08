---
title: Azure Media Services v3 sürüm notları | Microsoft Docs
description: En son gelişmelerden haberdar olmak için bu makalede, Azure Media Services v3 hakkında en son güncelleştirmeler sunulmaktadır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: na
ms.topic: article
ms.date: 06/03/2020
ms.author: juliako
ms.openlocfilehash: d09fba2130dc302378a59200349a569cc248234b
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86075442"
---
# <a name="azure-media-services-v3-release-notes"></a>Azure Media Services v3 sürüm notları

>Bu URL 'YI kopyalayarak ve yapıştırarak bu sayfanın ne zaman yeniden ziyaret ettikleridir hakkında bildirim alın: `https://docs.microsoft.com/api/search/rss?search=%22Azure+Media+Services+v3+release+notes%22&locale=en-us` RSS akışı okuyucusuna.

En son gelişmelerden haberdar olmak için, bu makalede hakkında bilgi verilmektedir:

* En son yayınlar
* Bilinen sorunlar
* Hata düzeltmeleri
* Kullanım dışı işlevsellik

## <a name="known-issues"></a>Bilinen sorunlar

> [!NOTE]
> V3 [canlı olaylarını](live-events-outputs-concept.md)yönetmek, v3 [varlıklarını](assets-concept.md) ve Işleri görüntülemek, API 'lere erişme hakkında bilgi almak, içeriği şifrelemek için [Azure Portal](https://portal.azure.com/) kullanabilirsiniz. Diğer tüm yönetim görevleri için (örneğin, dönüşümleri ve işleri yönetme) [REST API](https://aka.ms/ams-v3-rest-ref), [CLI](https://aka.ms/ams-v3-cli-ref)veya desteklenen [SDK 'lardan](media-services-apis-overview.md#sdks)birini kullanın.
>
> Ayrıntılar için bkz.: [Media Services v3 için Azure Portal sınırlamaları](frequently-asked-questions.md#what-are-the-azure-portal-limitations-for-media-services-v3).

## <a name="june-2020"></a>Haziran 2020

### <a name="live-video-analytics-on-iot-edge-preview-release"></a>IoT Edge Preview sürümünde canlı video analizi

IoT Edge canlı video analizinin önizlemesi herkese açık oldu. Daha fazla bilgi için bkz. [sürüm notları](../live-video-analytics-edge/release-notes.md).

IoT Edge üzerindeki canlı video analizi, Media Service ailesine yönelik bir genişletmektir. Bu, kendi Edge cihazlarınızda seçtiğiniz AI modelleriyle canlı videoyu çözümlemenize ve isteğe bağlı olarak bu videoyu yakalayıp kaydetmenize olanak sağlar. Artık canlı bir video işlem hattı oluşturma ve çalıştırma karmaşıklığı hakkında endişelenmenize gerek kalmadan gerçek zamanlı video analiziyle birlikte uygulamalar oluşturabilirsiniz.

### <a name="search-by-topics"></a>Konulara göre ara

Artık belirli konularda (yalnızca API) videoları aramak için arama API 'sini kullanabilirsiniz.

Konular, `textScope` (isteğe bağlı parametre) bir parçası olarak eklenir. Ayrıntılar için bkz. [API](https://api-portal.videoindexer.ai/docs/services/Operations/operations/Search-Videos) .  

### <a name="labels-enhancement"></a>Etiket geliştirmesi

Etiket etiketi yükseltildi ve artık tanımlanabilecek daha fazla görsel etiket içeriyor.

## <a name="may-2020"></a>Mayıs 2020

Azure Media Services artık şu bölgelerde genel kullanıma sunulmuştur: "Almanya Kuzey", "Almanya Orta Batı", "İsviçre Kuzey" ve "İsviçre Batı". Müşteriler, Azure portal kullanarak bu bölgelere Media Services dağıtabilir.

## <a name="april-2020"></a>Nisan 2020

### <a name="improvements-in-documentation"></a>Belgelerde iyileştirmeler

Azure Media Player belgeleri [Azure belgelerine](../azure-media-player/azure-media-player-overview.md)geçirildi.

## <a name="january-2020"></a>Ocak 2020

### <a name="improvements-in-media-processors"></a>Medya işlemcilerinde iyileştirmeler

- Video analizinde aralıklı kaynaklar için geliştirilmiş destek: Bu içerikler artık, çıkarım altyapılarına gönderilmeden önce doğru şekilde uyumlu değildir.
- "En Iyi" modu ile küçük resimler oluştururken, kodlayıcı artık tek parçalı olmayan bir çerçeveyi seçmek için 30 saniyeden daha fazla arama yapar.

### <a name="azure-government-cloud-updates"></a>Azure Kamu bulut güncelleştirmeleri

Media Services şu Azure Kamu bölgelerinde: *Usgov Arizona* ve *Usgov Texas*.

## <a name="december-2019"></a>Aralık 2019

Canlı ve video isteğe bağlı akış için *kaynak yardımı önceden getirme* ÜSTBILGILERI için CDN desteği eklendi; Akamai CDN ile doğrudan sözleşme sahibi olan müşteriler için kullanılabilir. Kaynak-yardım CDN-önceden getirme özelliği, Akamai CDN ve Azure Media Services Origin arasındaki aşağıdaki HTTP üst bilgi alışverişlerini içerir:

|HTTP üstbilgisi|Değerler|Gönderen|Alıcı|Amaç|
| ---- | ---- | ---- | ---- | ----- |
|CDN-Origin-yardım-önceden getirme-etkin | 1 (varsayılan) veya 0 |CDN|Kaynak|CDN 'in önceden getirme etkin olduğunu göstermek için|
|CDN-Origin-yardım-önceden getirme-yol| Örnek: <br/>Parçalar (video = 1400000000, Format = MPD-Time-cmaf)|Kaynak|CDN|CDN 'ye önceden getirme yolu sağlamak için|
|CDN-Origin-yardım-önceden getirme-Istek|1 (önceden getirme isteği) veya 0 (normal istek)|CDN|Kaynak|CDN 'den gelen isteğin bir önceden getirme olduğunu belirtmek için|

Üst bilgi değişimi 'nin bir parçasını eylemde görmek için aşağıdaki adımları deneyebilirsiniz:

1. Bir ses veya video segmenti veya parçası için Media Services kaynağa bir istek vermek üzere Postman veya kıvrımlı kullanın. İstekte CDN-Origin-yardım-önceden getirme-etkin: 1 üst bilgisini eklediğinizden emin olun.
2. Yanıtta, değeri olarak göreli bir yol ile CDN-Origin-yardım-önceden getirme-yol başlığını görmeniz gerekir.

## <a name="november-2019"></a>Kasım 2019

### <a name="live-transcription-preview"></a>Canlı döküm önizlemesi

Canlı döküm artık genel önizlemeye sunuldu ve Batı ABD 2 bölgesinde kullanıma sunulmuştur.

Canlı döküm, canlı olaylarla birlikte eklenti özelliği olarak çalışmak üzere tasarlanmıştır.  Doğrudan geçiş ve standart ya da Premium kodlama canlı olaylarında desteklenir.  Bu özellik etkinleştirildiğinde, hizmet bilişsel hizmetler 'in [konuşmadan metne](../../cognitive-services/speech-service/speech-to-text.md) özelliğini kullanarak gelen seslerdeki konuşulan kelimeleri metne dönüştürür. Bu metin daha sonra MPEG-DASH ve HLS protokollerinde video ve ses ile birlikte teslim için kullanılabilir hale getirilir. Faturalandırma, "çalışıyor" durumundayken canlı etkinliğin ek maliyeti olan yeni bir eklenti ölçmesini temel alır.  Canlı döküm ve faturalandırma hakkında daha fazla bilgi için bkz. [canlı](live-transcription.md) döküm

> [!NOTE]
> Şu anda, canlı döküm yalnızca Batı ABD 2 bölgesinde önizleme özelliği olarak kullanılabilir. Konuşulan sözcüklerin yalnızca şu anda Ingilizce (en-US) olarak dökümünü destekler.

### <a name="content-protection"></a>İçerik koruma

Eylül ayının sınırlı bölgelerinde yayınlanan *belirteç yeniden yürütme engellemesi* özelliği artık tüm bölgelerde kullanılabilir.
Media Services müşteriler artık, bir anahtar veya lisans istemek için aynı belirtecin kaç kez kullanılabileceği konusunda bir sınır ayarlayabilirler. Daha fazla bilgi için bkz. [belirteç yeniden yürütme engellemesi](content-protection-overview.md#token-replay-prevention).

### <a name="new-recommended-live-encoder-partners"></a>Yeni önerilen canlı kodlayıcı iş ortakları

RTMP canlı akışı için aşağıdaki yeni önerilen iş ortağı kodlayıcıları desteği eklendi:

- [Cambrıa canlı 4,3](https://www.capellasystems.net/products/cambria-live/)
- [GoPro Hero7/8 ve maks. eylem kameralar](https://gopro.com/help/articles/block/getting-started-with-live-streaming)
- [Restream.io](https://restream.io/)

### <a name="file-encoding-enhancements"></a>Dosya kodlama geliştirmeleri

- Yeni bir Içerik algılayan kodlama önceden ayarı artık kullanılabilir. İçeriğe duyarlı kodlama kullanarak bir GOP hizalı MP4 'leri kümesi üretir. Herhangi bir giriş içeriği verildiğinde, hizmet giriş içeriğinin ilk hafif analizini yapar. Bu sonuçları, en uygun katman sayısını, uygun bit hızını ve uyarlamalı akış tarafından teslim edilmek üzere çözüm ayarlarını belirlemede kullanır. Bu ön ayar özellikle, çıkış dosyalarının daha düşük bit hızlarındaki ancak izleyicilere uygun bir deneyim sunan bir kalitede olduğu düşük karmaşıklık ve orta ölçekli videolar için geçerlidir. Çıktı, video ve ses Aralanmış MP4 dosyaları içerir. Daha fazla bilgi için bkz. [Açık API özellikleri](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/Encoding.json).
- Standart Kodlayıcıdaki Re-sizer için iyileştirilmiş performans ve çoklu iş parçacığı. Belirli koşullar altında, müşteri% 5-40 VOD kodlaması arasında bir performans artışı görmelidir. Birden çok bit hızında kodlanan düşük karmaşıklık içeriği, en yüksek performans artışına neden olur. 
- Standart kodlama, zaman tabanlı GOP ayarı kullanılırken VOD kodlaması sırasında değişken çerçeve oranı (VFR) için normal bir GOP temposunda sağlar.  Bu, müşterilerin 15-30 fps arasında değişen karma kare hızı içeriğini gönderen bir müşterinin, yalnızca Uyarlamalı bit hızında akış MP4 dosyalarına göre hesaplanan normal GOP uzaklıkları görebilmesini sağlar. Bu, HLS veya DASH üzerinden gönderim yaparken parçalar arasında sorunsuzca geçiş yapma yeteneğini geliştirir. 
-  Değişken çerçeve oranı (VFR) kaynak içeriği için iyileştirilmiş AV Sync

### <a name="video-indexer-video-analytics"></a>Video Indexer, video analizi

- VideoAnalyzer önayarı kullanılarak ayıklanan ana kareler artık videonun yeniden boyutlandırılması yerine özgün çözümlenmektedir. Yüksek çözünürlüklü ana kare ayıklama, orijinal kalite görüntüleri sağlar ve Microsoft Görüntü İşleme ve Özel Görüntü İşleme Hizmetleri tarafından sunulan görüntü tabanlı yapay zeka modellerini kullanarak videonuzdan daha da ayrıntılı bilgiler elde etmenizi sağlar.

## <a name="september-2019"></a>Eylül 2019

###  <a name="media-services-v3"></a>Media Services v3  

#### <a name="live-linear-encoding-of-live-events"></a>Canlı olayların canlı doğrusal kodlaması

Media Services v3, canlı etkinliklerin canlı doğrusal kodlamasının 24 saat x 365 gün önizlemesini duyuruyor.

###  <a name="media-services-v2"></a>Media Services v2  

#### <a name="deprecation-of-media-processors"></a>Medya işlemcilerinin kullanımdan kaldırılması

*Azure Media Indexer* ve *Azure Media Indexer 2 Preview*kullanım dışı olarak duyuruluyoruz. Kullanımdan kaldırma tarihleri için, [eski bileşenler](../previous/legacy-components.md) konusuna bakın. [Azure Media Services video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) bu eski medya işlemcilerinin yerini alır.

Daha fazla bilgi için [Azure Media Indexer ve Azure Media Indexer 2 ' den Azure Media Services video Indexer geçiş](../previous/migrate-indexer-v1-v2.md)konusuna bakın.

## <a name="august-2019"></a>Ağustos 2019

###  <a name="media-services-v3"></a>Media Services v3  

#### <a name="south-africa-regional-pair-is-open-for-media-services"></a>Güney Afrika bölgesel çifti Media Services için açık 

Media Services artık Güney Afrika Kuzey ve Güney Afrika Batı bölgelerinde kullanılabilir.

Daha fazla bilgi için bkz. [Media Services v3 'nin bulunduğu bulutlar ve bölgeler](azure-clouds-regions.md).

###  <a name="media-services-v2"></a>Media Services v2  

#### <a name="deprecation-of-media-processors"></a>Medya işlemcilerinin kullanımdan kaldırılması

Kullanımdan kalkmakta olan *Windows Azure Medya Kodlayıcısı* (WAME) ve *Azure Medya Kodlayıcısı* (AME) medya işlemcilerinin kullanımdan kaldırılması duyuruluyor. Kullanımdan kaldırma tarihleri için, bu [eski bileşenler](../previous/legacy-components.md) konusuna bakın.

Ayrıntılar için bkz. [WAME 'i Media Encoder Standard geçirin](https://go.microsoft.com/fwlink/?LinkId=2101334) ve [adı Media Encoder Standard geçirin](https://go.microsoft.com/fwlink/?LinkId=2101335).
 
## <a name="july-2019"></a>Temmuz 2019

### <a name="content-protection"></a>İçerik koruma

Belirteç kısıtlamasıyla korunan içerik akışı yaparken, son kullanıcılar anahtar teslim isteğinin bir parçası olarak gönderilen bir belirteç elde etmeniz gerekir. *Belirteç yeniden yürütme engellemesi* özelliği, Media Services müşterilerin aynı belirtecin bir anahtar veya lisans istemek için kaç kez kullanılabileceği konusunda bir sınır ayarlamasına olanak tanır. Daha fazla bilgi için bkz. [belirteç yeniden yürütme engellemesi](content-protection-overview.md#token-replay-prevention).

Haziran itibariyle önizleme özelliği yalnızca ABD Orta ve ABD Orta Batı kullanılabilir.

## <a name="june-2019"></a>Haziran 2019

### <a name="video-subclipping"></a>Video alt kırpması

Artık bir [işi](https://docs.microsoft.com/rest/api/media/jobs)kullanarak kodlarken videoyu kırpabilir veya alt kırpabilirsiniz. 

Bu işlev, [Builtınstandardencoderönayar](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) ön ayarları veya [Standardencoderönayar](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) önayarları kullanılarak oluşturulan [dönüşümlerle](https://docs.microsoft.com/rest/api/media/transforms) birlikte kullanılır. 

Örneklere bakın:

* [.NET ile videoyu alt kırpın](subclip-video-dotnet-howto.md)
* [REST ile videoyu alt kırpın](subclip-video-rest-howto.md)

## <a name="may-2019"></a>Mayıs 2019

### <a name="azure-monitor-support-for-media-services-diagnostic-logs-and-metrics"></a>Azure Izleyici Media Services tanılama günlükleri ve ölçümleri desteği

Artık Media Services tarafından yayılan telemetri verilerini görüntülemek için Azure Izleyici 'yi kullanabilirsiniz.

* Media Services anahtar teslim uç noktası tarafından gönderilen istekleri izlemek için Azure Izleyici tanılama günlüklerini kullanın. 
* Media Services [akış uç noktaları](streaming-endpoint-concept.md)tarafından yayılan ölçümleri izleyin.   

Ayrıntılar için bkz. [Media Services ölçümleri ve tanılama günlüklerini izleme](media-services-metrics-diagnostic-logs.md).

### <a name="multi-audio-tracks-support-in-dynamic-packaging"></a>Dinamik paketlemeyle çoklu ses izleme desteği 

Birden çok codec ve dilde birden çok ses izi bulunan varlıklar akışı yaparken, [dinamik paketleme](dynamic-packaging-overview.md) artık HLS çıkışı (sürüm 4 veya üzeri) için birden çok ses parçasını desteklemektedir.

### <a name="korea-regional-pair-is-open-for-media-services"></a>Kore bölgesel çifti Media Services için açık 

Media Services artık Kore Orta ve Kore Güney bölgelerinde kullanılabilir. 

Daha fazla bilgi için bkz. [Media Services v3 'nin bulunduğu bulutlar ve bölgeler](azure-clouds-regions.md).

### <a name="performance-improvements"></a>Performans iyileştirmeleri

Media Services performans iyileştirmeleri içeren güncelleştirmeler eklendi.

* İşleme için desteklenen en büyük dosya boyutu güncelleştirildi. Bkz., [Kotalar ve sınırlar](limits-quotas-constraints.md).
* [Kodlama hızı iyileştirmeleri](media-reserved-units-cli-how-to.md#choosing-between-different-reserved-unit-types).

## <a name="april-2019"></a>Nisan 2019

### <a name="new-presets"></a>Yeni Önayarlar

* [Facedetectorönayar](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#facedetectorpreset) yerleşik çözümleyici önayarlarına eklendi.
* [ContentAwareEncodingExperimental](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset) , yerleşik kodlayıcı önayarlarına eklendi. Daha fazla bilgi için bkz. [içeriğe duyarlı kodlama](content-aware-encoding.md). 

## <a name="march-2019"></a>Mart 2019

Dinamik paketleme artık Dolby Atmos 'ı destekliyor. Daha fazla bilgi için bkz. [dinamik paketleme tarafından desteklenen ses codec bileşenleri](dynamic-packaging-overview.md#audio-codecs-supported-by-dynamic-packaging).

Artık akış bulucusu için uygulanacak varlık veya hesap filtrelerinin bir listesini belirtebilirsiniz. Daha fazla bilgi için bkz. [akış bulucu ile filtreleri ilişkilendirme](filters-concept.md#associating-filters-with-streaming-locator).

## <a name="february-2019"></a>Şubat 2019

Media Services v3 artık Azure Ulusal bulutlarında desteklenmektedir. Tüm özellikler henüz tüm bulutlarda kullanılamaz. Ayrıntılar için bkz. [Azure Media Services v3 'nin bulunduğu bulutlar ve bölgeler](azure-clouds-regions.md).

Media Services için Azure Event Grid şemalarına [Microsoft. Media. Joi Putprogress](media-services-event-schemas.md#monitoring-job-output-progress) olayı eklendi.

## <a name="january-2019"></a>Ocak 2019

### <a name="media-encoder-standard-and-mpi-files"></a>Media Encoder Standard ve MPı dosyaları 

MP4 dosyaları üretmek için Media Encoder Standard ile kodlarken, yeni bir. MPI dosyası oluşturulur ve çıkış varlığına eklenir. Bu MPı dosyası, [dinamik paketleme](dynamic-packaging-overview.md) ve akış senaryoları için performansı iyileştirmeye yöneliktir.

MPı dosyasını değiştirmemelisiniz veya kaldırmamalıdır ya da bu tür bir dosyanın var olan (veya olmayan) hizmetinize herhangi bir bağımlılığı almalısınız.

## <a name="december-2018"></a>Aralık 2018

V3 API 'sinin GA sürümündeki güncelleştirmeler şunlardır:
       
* **Varlık filtreleri** ve **hesap filtreleri**için **presentationtimerange** özellikleri artık ' gerekli ' değildir. 
* **İşler** ve **dönüşümler** için $top ve $Skip sorgu seçenekleri kaldırılmıştır ve $OrderBy eklenmiştir. Yeni sıralama işlevinin eklenmesinin bir parçası olarak, $top ve $skip seçeneklerinin, uygulanmamasına rağmen daha önce daha önce açığa çıkmıştı.
* Numaralandırma genişletilebilirliği yeniden etkinleştirildi. Bu özellik SDK 'nın önizleme sürümlerinde etkinleştirilmiştir ve GA sürümünde yanlışlıkla devre dışı bırakıldı.
* Önceden tanımlanmış iki akış ilkesi yeniden adlandırıldı. **Securestreaming** artık **Multidrmcencstreaming**. **SecureStreamingWithFairPlay** artık **Predefined_MultiDrmStreaming**.

## <a name="november-2018"></a>Kasım 2018

CLı 2,0 modülü artık [Azure Media Services v3 GA](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest) – v 2.0.50 için kullanılabilir.

### <a name="new-commands"></a>Yeni komutlar

- [az AMS hesabı](https://docs.microsoft.com/cli/azure/ams/account?view=azure-cli-latest)
- [az AMS Account-Filter](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest)
- [az AMS varlık](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest)
- [az AMS varlık-filtre](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest)
- [az AMS Content-Key-Policy](https://docs.microsoft.com/cli/azure/ams/content-key-policy?view=azure-cli-latest)
- [az AMS Job](https://docs.microsoft.com/cli/azure/ams/job?view=azure-cli-latest)
- [az AMS canlı-olay](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest)
- [az AMS Live-Output](https://docs.microsoft.com/cli/azure/ams/live-output?view=azure-cli-latest)
- [az AMS streaming-Endpoint](https://docs.microsoft.com/cli/azure/ams/streaming-endpoint?view=azure-cli-latest)
- [az AMS streaming-Locator](https://docs.microsoft.com/cli/azure/ams/streaming-locator?view=azure-cli-latest)
- [az AMS Account MRU](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) -medya ayrılmış birimlerini yönetmenizi sağlar. Daha fazla bilgi için bkz. [Ölçek medya ayrılmış birimleri](media-reserved-units-cli-how-to.md).

### <a name="new-features-and-breaking-changes"></a>Yeni özellikler ve son değişiklikler

#### <a name="asset-commands"></a>Varlık komutları

- ```--storage-account```ve ```--container``` bağımsız değişkenler eklendi.
- Sona erme saati (şimdi + 23h) için varsayılan değerler ve komutta izinler (okuma) ```az ams asset get-sas-url``` eklendi.

#### <a name="job-commands"></a>İş komutları

- ```--correlation-data```ve ```--label``` eklenen bağımsız değişkenler
- ```--output-asset-names```olarak yeniden adlandırıldı ```--output-assets``` . Şimdi, ' assetName = Label ' biçiminde varlıkların boşlukla ayrılmış bir listesini kabul eder. Etiketi olmayan bir varlık şu şekilde gönderilebilir: ' assetName = '.

#### <a name="streaming-locator-commands"></a>Akış Bulucu komutları

- ```az ams streaming locator```temel komut ile değiştirilmiştir ```az ams streaming-locator``` .
- ```--streaming-locator-id```ve ```--alternative-media-id support``` bağımsız değişkenler eklendi.
- ```--content-keys argument```bağımsız değişken güncelleştirildi.
- ```--content-policy-name```olarak yeniden adlandırıldı ```--content-key-policy-name``` .

#### <a name="streaming-policy-commands"></a>Akış Ilkesi komutları

- ```az ams streaming policy```temel komut ile değiştirilmiştir ```az ams streaming-policy``` .
- Şifreleme parametreleri desteği ```az ams streaming-policy create``` eklendi.

#### <a name="transform-commands"></a>Dönüşüm komutları

- ```--preset-names```bağımsız değişken ile değiştirilmiştir ```--preset``` . Artık tek seferde yalnızca 1 çıkış/önayar ayarlayabilirsiniz (çalıştırmanız gerekir daha fazla bilgi için ```az ams transform output add``` ). Ayrıca, yolu özel JSON 'unuza geçirerek özel Standardencoderönayar ayarlayabilirsiniz.
- ```az ams transform output remove```, kaldırılacak çıkış dizini geçirerek gerçekleştirilebilir.
- ```--relative-priority, --on-error, --audio-language and --insights-to-extract```ve komutlarına eklenen bağımsız değişkenler ```az ams transform create``` ```az ams transform output add``` .

## <a name="october-2018---ga"></a>Ekim 2018-GA

Bu bölümde Azure Media Services (AMS) Ekim güncelleştirmeleri açıklanmaktadır.

### <a name="rest-v3-ga-release"></a>REST v3 GA sürümü

[Rest v3 GA sürümü](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01) , canlı, hesap/varlık düzeyi bildirim FILTRELERI ve DRM desteği için daha fazla API içerir.

#### <a name="azure-resource-management"></a>Azure Kaynak Yönetimi 

Azure Kaynak yönetimi desteği, birleştirilmiş yönetim ve işlemler API 'SI (artık tek bir yerde) sunar.

Bu sürümden itibaren, canlı olaylar oluşturmak için Kaynak Yöneticisi şablonları kullanabilirsiniz.

#### <a name="improvement-of-asset-operations"></a>Varlık işlemlerinin geliştirilmesi 

Aşağıdaki geliştirmeler sunulmuştur:

- HTTP (s) URL 'Leri veya Azure Blob Storage SAS URL 'Lerinden alma.
- Varlıklar için kendi kapsayıcı adlarından istediğinizi belirtin. 
- Azure Işlevleri ile özel iş akışları oluşturmak için daha kolay çıkış desteği.

#### <a name="new-transform-object"></a>Yeni dönüştürme nesnesi

Yeni **dönüşüm** nesnesi kodlama modelini basitleştirir. Yeni nesne, kodlama Kaynak Yöneticisi şablonları ve hazır ayarları oluşturmayı ve paylaşmayı kolaylaştırır. 

#### <a name="azure-active-directory-authentication-and-rbac"></a>Azure Active Directory kimlik doğrulaması ve RBAC

Azure AD kimlik doğrulaması ve rol tabanlı Access Control (RBAC), Azure AD 'de role veya kullanıcılara göre güvenli dönüşümler, canlı olaylar, Içerik anahtar Ilkeleri veya varlıklar sağlar.

#### <a name="client-sdks"></a>İstemci SDK'ları  

Media Services v3 sürümünde desteklenen diller: .NET Core, Java, Node.js, Ruby, TypeScript, Python, go.

#### <a name="live-encoding-updates"></a>Canlı kodlama güncelleştirmeleri

Aşağıdaki canlı kodlama güncelleştirmeleri sunulmuştur:

- Canlı için yeni düşük gecikme modu (10 saniyelik uçtan uca).
- İyileştirilmiş RTMP desteği (artan kararlılık ve daha fazla kaynak Kodlayıcısı desteği).
- RTMPS güvenli alma.

    Canlı bir olay oluşturduğunuzda, artık 4 alma URL 'Si alırsınız. 4 içe alınan URL 'Ler neredeyse aynıdır, aynı akış belirtecine (AppID) sahiptir, yalnızca bağlantı noktası numarası bölümü farklıdır. URL 'Lerden ikisi, RTMPS için birincil ve yedeğdir. 
- 24 saat kodlamayı kodlama desteği. 
- SCTE35 aracılığıyla RTMP 'de geliştirilmiş ad sinyali desteği.

#### <a name="improved-event-grid-support"></a>İyileştirilmiş Event Grid desteği

Aşağıdaki Event Grid destek geliştirmelerini görebilirsiniz:

- Logic Apps ve Azure Işlevleriyle daha kolay geliştirme için Azure Event Grid tümleştirme. 
- Kodlama, Canlı Kanallar ve daha fazlası için olaylar için abone olun.

### <a name="cmaf-support"></a>CMAF desteği

Apple HLS (iOS 11 +) için CMAF ve ' CBCS ' şifreleme desteği ve CMAF 'yi destekleyen MPEG-DASH çalarlar.

### <a name="video-indexer"></a>Video Indexer

Video Indexer GA yayını Ağustos ayında duyuruldu. Şu anda desteklenen özellikler hakkında daha fazla bilgi için bkz. [video Indexer nedir](../../cognitive-services/video-indexer/video-indexer-overview.md?toc=/azure/media-services/video-indexer/toc.json&bc=/azure/media-services/video-indexer/breadcrumb/toc.json). 

### <a name="plans-for-changes"></a>Değişiklik planları

#### <a name="azure-cli-20"></a>Azure CLI 2.0
 
Tüm özelliklerle (canlı, Içerik anahtarı Ilkeleri, hesap/varlık filtreleri, akış Ilkeleri dahil) işlemler içeren Azure CLı 2,0 modülü yakında kullanıma sunulacak. 

### <a name="known-issues"></a>Bilinen sorunlar

Yalnızca varlık veya AccountFilters için Önizleme API 'sini kullanan müşteriler aşağıdaki sorundan etkilenir.

Media Services v3 CLı veya API 'Leri ile 09/28 ve 10/12 arasında varlıklar veya hesap filtreleri oluşturduysanız, tüm varlık ve AccountFilters değerlerini kaldırmalı ve sürüm çakışması nedeniyle bunları yeniden oluşturmanız gerekir. 

## <a name="may-2018---preview"></a>Mayıs 2018-Önizleme

### <a name="net-sdk"></a>.NET SDK

.NET SDK 'da aşağıdaki özellikler mevcuttur:

* Medya içeriğini kodlamak veya çözümlemek için **dönüşümler** ve **işler** . Örnekler için bkz. [akış dosyaları](stream-files-tutorial-with-api.md) ve [Çözümleme](analyze-videos-tutorial-with-api.md).
* Son Kullanıcı cihazlarındaki yayımlama ve akış içeriği için **akış Konumlandırıcı**
* İçerik teslim edilirken anahtar teslimi ve içerik korumayı (DRM) yapılandırmak için **akış ilkeleri** ve **içerik anahtarı ilkeleri** .
* Canlı akış içeriğini alma ve arşivlemeyi yapılandırmak için **canlı olaylar** ve **canlı çıktılar** .
* Azure depolama 'da medya içeriğini depolamak ve yayımlamak için **varlıklar** . 
* Canlı ve isteğe bağlı medya içeriği için dinamik paketleme, şifreleme ve akışı yapılandırmak ve ölçeklendirmek için **akış uç noktaları** .

### <a name="known-issues"></a>Bilinen sorunlar

* Bir iş gönderilirken, HTTPS URL 'Leri, SAS URL 'Leri veya Azure Blob depolama alanında bulunan dosyalara yollar kullanarak kaynak videonuzu almak istediğinizi belirtebilirsiniz. Şu anda Media Services v3, HTTPS URL 'Leri üzerinden öbekli aktarım kodlamasını desteklemez.

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="see-also"></a>Ayrıca bkz.

[Media Services V2 'den v3 'e geçmek Için geçiş kılavuzu](migrate-from-v2-to-v3.md#known-issues).

## <a name="next-steps"></a>Sonraki adımlar

- [Genel Bakış](media-services-overview.md)
- [Media Services v3 belge güncelleştirmeleri](docs-release-notes.md)
- [Media Services V2 sürüm notları](../previous/media-services-release-notes.md)
