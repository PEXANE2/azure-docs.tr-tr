---
title: Azure Medya Hizmetleri sürüm notları | Microsoft Dokümanlar
description: Bu makalede, Microsoft Azure Media Services v2 sürüm notları hakkında konuşuyor.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2019
ms.author: juliako
ms.openlocfilehash: a55ef4f814c0d13a6efbeeb8cd9a5565d2869432
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183304"
---
# <a name="azure-media-services-release-notes"></a>Azure Medya Hizmetleri sürüm notları

Azure Medya Hizmetleri için bu sürüm notları, önceki sürümlerden ve bilinen sorunlardan gelen değişiklikleri özetler.

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik eklenmemektedir. <br/>En son sürümü göz atın, [Medya Hizmetleri v3](https://docs.microsoft.com/azure/media-services/latest/). Ayrıca, [v2'den v3'e geçiş kılavuzuna](../latest/migrate-from-v2-to-v3.md) bakın

Sizi etkileyen sorunları çözmeye odaklanabilmek için müşterilerimizden haber almak istiyoruz. Bir sorunu bildirmek veya soru sormak için [Azure Medya Hizmetleri MSDN Forumu'nda]bir gönderi gönderin. 

## <a name="known-issues"></a><a id="issues"/>Bilinen sorunlar
### <a name="media-services-general-issues"></a><a id="general_issues"/>Medya Hizmetleri genel sorunları

| Sorun | Açıklama |
| --- | --- |
| REST API'de birkaç yaygın HTTP üstbilgi sağlanmaz. |REST API'sini kullanarak Medya Hizmetleri uygulamalarını geliştirirseniz, bazı yaygın HTTP üstbilgi alanlarının (CLIENT-REQUEST-ID, REQUEST-ID ve RETURN-CLIENT-REQUEST-ID dahil) desteklenmediğini fark elersiniz. Üstbilgi gelecekteki bir güncelleştirmede eklenir. |
| Yüzde kodlamaya izin verilmez. |Medya Hizmetleri, akış içeriği için URL'ler inşa ederken IAssetFile.Name özelliğinin değerini kullanır (örneğin,). `http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters` Bu nedenle, yüzde kodlama izin verilmez. Ad özelliğinin değeri aşağıdaki [yüzde kodlama-ayrılmış karakterlerden](https://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters)hiçbirine sahip olamaz : !*'();:@&=+$,/?%#[]". Ayrıca, dosya adı uzantısı için yalnızca bir "." olabilir. |
| Azure Depolama SDK sürüm 3.x'in bir parçası olan ListBlobs yöntemi başarısız olur. |Medya [Hizmetleri, 2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) sürümüne dayalı SAS URL'leri oluşturur. Bir blob kapsayıcısındaki lekeleri listelemek için Depolama SDK'sını kullanmak istiyorsanız, Depolama SDK sürüm 2.x'in bir parçası olan [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.listblobs) yöntemini kullanın. |
| Medya Hizmetleri azaltma mekanizması, hizmete aşırı istekte bulunduran uygulamalar için kaynak kullanımını kısıtlar. Hizmet "Hizmet Kullanılamıyor" 503 HTTP durum kodunu döndürebilir. |Daha fazla bilgi için, [Medya Hizmetleri hata kodlarında](media-services-encoding-error-codes.md)503 HTTP durum kodunun açıklamasına bakın. |
| Varlıkları sorguladığınızı zedilen, ortak REST sürümü 2 sorgu sonuçlarını 1.000 sonuçla sınırladığı için aynı anda 1.000 varlık sınırı döndürülür. |[Bu .NET örneğinde](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) ve [bu REST API örneğinde](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities)açıklandığı gibi (.NET)/üst (REST) atla ve al'ı kullanın. |
| Bazı istemciler, Düzgün Akış bildiriminde yineetiketi sorunuyla karşılaşabilir. |Daha fazla bilgi için [bu bölüme](media-services-deliver-content-overview.md#known-issues)bakın. |
| Medya Hizmetleri .NET SDK nesneleri seri hale getirilemez ve sonuç olarak Redis için Azure Önbelleği ile çalışmaz. |Redis için Azure Önbelleğine eklemek için SDK AssetCollection nesnesini seri hale getirmeye çalışırsanız, bir özel durum atılır. |
|REST API, Varlık veya Hesap düzeyi filtresi almaya çalışırken "Filtreye REST Api'nin bu sürümüyle erişilemez" diyen bir hata iletisi ile yanıt verir.|Filtre, filtreyi almak için kullanılandan daha yeni bir API sürümüyle oluşturuldu veya değiştirildi. Bu, müşteri tarafından kullanılan kod veya araçlar tarafından iki API sürümü kullanılıyorsa gerçekleşebilir.  Burada en iyi çözüm, kodu veya araçları yeni veya iki API sürümlerini kullanmak için yükseltmektir.|

## <a name="rest-api-version-history"></a><a id="rest_version_history"/>REST API sürüm geçmişi
Media Services REST API sürüm geçmişi hakkında bilgi için [Azure Media Services REST API başvurusuna]bakın.

## <a name="february-2020"></a>Şubat 2020

Bazı analitik ortam işlemcileri kullanımdan kaldırılacaktır. Emeklilik tarihleri [için, eski bileşenler](legacy-components.md) konusuna bakın.

## <a name="september-2019"></a>Eylül 2019

### <a name="deprecation-of-media-processors"></a>Ortam işlemcilerinin amortismanı

*Azure Media Indexer* ve Azure Media *Indexer 2 Preview'un*amortismanını duyuruyoruz. [Azure Media Services Video Indexer](https://docs.microsoft.com/azure/media-services/video-indexer/) bu eski medya işlemcilerinin yerini alır.

Emeklilik tarihleri için bu [eski bileşenler](legacy-components.md) konusuna bakın.

Ayrıca bkz: [Azure Media Indexer ve Azure Media Indexer 2'den Azure Media Services Video Indexer'a geçirin.](migrate-indexer-v1-v2.md)

## <a name="august-2019"></a>Ağustos 2019

### <a name="deprecation-of-media-processors"></a>Ortam işlemcilerinin amortismanı

*Windows Azure Media Encoder* (WAME) ve Azure *Media Encoder* (AME) medya işlemcilerinin amortismanını duyururuz. Emeklilik tarihleri için bu [eski bileşenler](legacy-components.md) konusuna bakın.

Ayrıntılar için [WAME'yi Media Encoder Standardına Geçirve](https://go.microsoft.com/fwlink/?LinkId=2101334) [AME'yi Media Encoder Standardına Geçirin'](https://go.microsoft.com/fwlink/?LinkId=2101335)e bakın.

## <a name="march-2019"></a>Mart 2019

Azure Medya Hizmetleri'nin Media Hyperlapse Preview özelliği küçümsüldü.

## <a name="december-2018"></a>Aralık 2018

Azure Medya Hizmetleri'nin Media Hyperlapse Preview özelliği yakında kullanımdan kaldırılacak. Medya Hizmetleri, 19 Aralık 2018'den itibaren Media Hyperlapse'de artık değişiklik veya iyileştirme yapmayacaktır. 29 Mart 2019'da emekliye ayrıldı ve artık kullanılamaz.

## <a name="october-2018"></a>Ekim 2018

### <a name="cmaf-support"></a>CMAF desteği

CMAF'ı destekleyen Apple HLS (iOS 11+) ve MPEG-DASH oynatıcılar için CMAF ve 'cbcs' şifreleme desteği.

### <a name="web-vtt-thumbnail-sprites"></a>Web VTT küçük tırnak sprites

Artık Medya Hizmetlerini v2 API'lerimizi kullanarak Web VTT küçük resim spriteleri oluşturmak için kullanabilirsiniz. Daha fazla bilgi için [bkz.](generate-thumbnail-sprite.md)

## <a name="july-2018"></a>Temmuz 2018

En son hizmet sürümünde, bir İş başarısız olduğunda hizmet tarafından döndürülen hata iletilerinde, iki veya daha fazla satıra nasıl bölündüne ilişkin küçük biçimlendirme değişiklikleri vardır.

## <a name="may-2018"></a>Mayıs 2018 

12 Mayıs 2018'den itibaren canlı kanallar RTP/MPEG-2 aktarım akışı protokolünü desteklemeyedevam edecektir. Lütfen RTP/MPEG-2'den RTMP'ye veya parçalanmış MP4 (Düzgün Akış) yutma protokollerine geçiş yapmayın.

## <a name="october-2017-release"></a>Ekim 2017 sürümü
> [!IMPORTANT] 
> Medya Hizmetleri, Azure Erişim Denetim Hizmeti kimlik doğrulama anahtarları için desteği küçümsüyor. 22 Haziran 2018'de Access Control Service anahtarlarını kullanarak medya servisleri ile kod üzerinden kimlik doğrulayabilirsiniz. [Azure AD tabanlı kimlik doğrulamasına](media-services-use-aad-auth-to-access-ams-api.md)göre Azure Etkin Dizin (Azure AD) kullanmak için kodunuzu güncelleştirmeniz gerekir. Azure portalındaki bu değişiklikle ilgili uyarılara dikkat edin.

### <a name="updates-for-october-2017"></a>Ekim 2017 güncelleştirmeleri
#### <a name="sdks"></a>SDK’lar
* .NET SDK, Azure AD kimlik doğrulamasını destekleyecek şekilde güncelleştirildi. Access Control Service kimlik doğrulaması desteği, Azure AD'ye daha hızlı geçişi teşvik etmek amacıyla Nuget.org en son .NET SDK'dan kaldırıldı. 
* JAVA SDK, Azure AD kimlik doğrulamasını destekleyecek şekilde güncelleştirildi. Azure AD kimlik doğrulaması desteği Java SDK'ya eklendi. Medya Hizmetleri ile Java SDK'nın nasıl kullanılacağı hakkında bilgi için Azure [Medya Hizmetleri için Java istemcisi SDK ile başlama](media-services-java-how-to-use.md)

#### <a name="file-based-encoding"></a>Dosya tabanlı kodlama
* Artık içeriğinizi H.265 yüksek verimli video kodlama (HEVC) video codec'ine kodlamak için Premium Encoder'ı kullanabilirsiniz. H.265'i diğer codec'lere göre seçerseniz fiyatlandırma etkisi olmaz. HEVC patent lisansları hakkında daha fazla bilgi için [Çevrimiçi Hizmetler Koşulları'na](https://azure.microsoft.com/support/legal/)bakın.
* iOS11 veya GoPro Hero 6 kullanılarak çekilen video gibi H.265 (HEVC) video codec ile kodlanan kaynak video için artık bu videoları kodlamak için Premium Kodlayıcı'yı veya Standart Kodlayıcı'yı kullanabilirsiniz. Patent lisansları hakkında daha fazla bilgi için [Çevrimiçi Hizmet Koşulları'na](https://azure.microsoft.com/support/legal/)bakın.
* Birden çok dil ses parçası içeren içerik için, dil değerlerinin ilgili dosya biçimi belirtimine (örneğin, ISO MP4) göre doğru şekilde etiketlenmiş olması gerekir. Ardından, içeriği akış için kodlamak için Standart Kodlayıcı'yı kullanabilirsiniz. Elde edilen akış bulucu, kullanılabilir ses dillerini listeler.
* Standart Encoder şimdi iki yeni ses sadece sistem ön ayarları destekler, "AAC Audio" ve "AAC Kaliteli Ses." Her ikisi de sırasıyla 128 Kbps ve 192 Kbps bit hızlarında stereo gelişmiş ses kodlaması (AAC) çıkışı üretir.
* Premium Kodlayıcı artık quicktime/MOV dosya biçimlerini giriş olarak destekler. Video codec [bu GitHub makalede listelenen Apple ProRes türlerinden](https://docs.microsoft.com/azure/media-services/media-services-media-encoder-standard-formats)biri olmalıdır. Ses aac veya darbe kodu modülasyonu (PCM) olmalıdır. Premium Kodlayıcı, örneğin QuickTime/MOV dosyalarına sarılmış DVC/DVCPro videosunu desteklemez. Standart Kodlayıcı bu video codec'lerini destekler.
* Kodlayıcılarda aşağıdaki hata düzeltmeleri yapılmıştır:

    * Artık bir giriş varlığı kullanarak iş gönderebilirsiniz. Bu işler bittikten sonra, varlığı değiştirebilir (örneğin, varlık içindeki dosyaları ekleyebilir, silebilir veya yeniden adlandırabilirsiniz) ve ek işler gönderebilirsiniz.
    * Standart Encoder tarafından üretilen JPEG küçük resimlerin kalitesi artırıldı.
    * Standart Encoder, çok kısa süreli videolarda giriş meta verilerini ve küçük resim oluşturmayı daha iyi işler.
    * Standart Kodlayıcı'da kullanılan H.264 kod çözücüsündeki iyileştirmeler bazı nadir yapıları ortadan kaldırır. 

#### <a name="media-analytics"></a>Media Analytics
Azure Media Redactor'in genel kullanılabilirliği: Bu ortam işlemcisi, seçilen kişilerin yüzlerini bulanıkleştirerek anonymizasyon gerçekleştirir ve kamu güvenliği ve haber ortamı senaryolarında kullanım için idealdir. 

Bu yeni işlemci hakkında genel bir bakış [için, bu blog yazısı](https://azure.microsoft.com/blog/azure-media-redactor/)bakın. Dokümantasyon ve ayarlar hakkında daha fazla bilgi için [Azure Media Analytics ile Redact yüzlerini](media-services-face-redaction.md)görün.



## <a name="june-2017-release"></a>Haziran 2017 sürümü

Medya Hizmetleri artık [Azure AD tabanlı kimlik doğrulamasını](media-services-use-aad-auth-to-access-ams-api.md)destekliyor.

> [!IMPORTANT]
> Şu anda, Medya Hizmetleri Erişim Denetim Hizmeti kimlik doğrulama modelini destekler. Erişim Kontrol Hizmeti yetkilendirmesi 1 Haziran 2018 tarihinde iptal edilecektir. Azure AD kimlik doğrulaması modeline mümkün olan en kısa sürede geçiş yapmanız önerilir.

## <a name="march-2017-release"></a>Mart 2017 sürümü

Artık bir kodlama görevi oluştururken "Uyarlanabilir Akış" önceden ayarlanmış dizesini belirterek [bir bitrate merdivenini otomatik olarak oluşturmak](media-services-autogen-bitrate-ladder-with-mes.md) için Standart Kodlayıcı'yı kullanabilirsiniz. Medya Hizmetleri ile akış için bir video kodlamak için "Uyarlanabilir Akış" ön ayarını kullanın. Belirli bir senaryo için bir kodlama ön ayarını özelleştirmek [için, bu ön ayarlarla](media-services-mes-presets-overview.md)başlayabilirsiniz.

Artık [fMP4 parçaları oluşturan bir kodlama görevi oluşturmak](media-services-generate-fmp4-chunks.md)için Media Encoder Standard veya Media Encoder Premium İş Akışını kullanabilirsiniz. 

## <a name="february-2017-release"></a>Şubat 2017 sürümü

1 Nisan 2017'den itibaren, hesabınızdaki 90 günden büyük tüm iş kayıtları, ilişkili görev kayıtlarıyla birlikte otomatik olarak silinir. Toplam kayıt sayısı maksimum kotanın altında olsa bile silme işlemi gerçekleşir. İş/görev bilgilerini arşivlemek için, [Medya Hizmetleri .NET SDK ile varlıkları ve ilgili varlıkları yönet'te](media-services-dotnet-manage-entities.md)açıklanan kodu kullanabilirsiniz.

## <a name="january-2017-release"></a>Ocak 2017 sürümü

Medya Hizmetleri'nde, akış bitiş noktası, içeriği doğrudan bir istemci oynatıcı uygulamasına veya daha fazla dağıtım için bir içerik dağıtım ağına (CDN) sunabilen bir akış hizmetini temsil eder. Medya Hizmetleri ayrıca sorunsuz Azure İçerik Dağıtım Ağı tümleştirmesi de sağlar. Bir StreamingEndpoint hizmetinden giden akış, canlı akış, isteğe bağlı bir video veya medya hizmetleri hesabınıza varlığınızın aşamalı olarak indirilmesi olabilir. Her Medya Hizmetleri hesabı varsayılan bir akış bitiş noktası içerir. Hesap altında ek akış uç noktaları oluşturulabilir. 

Akış uç noktalarının iki sürümü vardır: 1.0 ve 2.0. 10 Ocak 2017'den itibaren, yeni oluşturulan Tüm Medya Hizmetleri hesapları sürüm 2.0 varsayılan akış bitiş noktasını içerir. Bu hesaba eklediğiniz ek akış uç noktaları da sürüm 2.0'dır. Bu değişiklik varolan hesapları etkilemez. Varolan akış uç noktaları sürüm 1.0'dır ve sürüm 2.0'a yükseltilebilir. Bu değişiklikle davranış, faturalandırma ve özellik değişiklikleri vardır. Daha fazla bilgi için bkz. [Akış uç noktalarına genel bakış](media-services-streaming-endpoints-overview.md).

2.15 sürümünden başlayarak, Medya Hizmetleri akış uç noktası varlığına aşağıdaki özellikleri ekledi:

* CdnSağlayıcı 
* CdnProfil
* ÜcretsizDeneme Bitiş Zamanı 
* StreamingEndpointVersion 

Bu özellikler hakkında daha fazla bilgi için [Bkz.](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint) 

## <a name="december-2016-release"></a>Aralık 2016 sürümü

 Artık medya hizmetlerini telemetri/metrik verilerine erişmek için medya hizmetlerini kullanabilirsiniz. Canlı kanal, akış bitiş noktası ve arşiv varlıkları için telemetri verileri toplamak için Medya Hizmetleri'nin geçerli sürümünü kullanabilirsiniz. Daha fazla bilgi için Medya [Hizmetleri telemetrisi'ne](media-services-telemetry-overview.md)bakın.

## <a name="july-2016-release"></a><a id="july_changes16"/>Temmuz 2016 sürümü
### <a name="updates-to-the-manifest-file-ism-generated-by-encoding-tasks"></a>Bildirim dosyasında güncelleştirmeler (*. ISM) kodlama görevleri tarafından oluşturulan
Bir kodlama görevi Media Encoder Standard veya Media Encoder Premium'a gönderildiğinde, kodlama görevi çıktı varlığında bir [akış bildirimi dosyası](media-services-deliver-content-overview.md) (*.ism) oluşturur. En son hizmet sürümüyle, bu akış bildirimi dosyasının sözdizimi güncelleştirildi.

> [!NOTE]
> Akış bildirimi (.ism) dosyasının sözdizimi dahili kullanım için ayrılmıştır. Gelecek sürümlerde değişebilir. Bu dosyanın içeriğini değiştirmeyin veya değiştirmeyin.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-the-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Yeni bir istemci bildirimi (*. ISMC) dosyası, bir kodlama görevi bir veya daha fazla MP4 dosyası çıktığında çıktı varlığında oluşturulur
En son hizmet sürümünden başlayarak, bir veya daha fazla MP4 dosyası oluşturan bir kodlama görevi tamamlandıktan sonra, çıktı varlığı da bir akış istemci bildirimi (*.ismc) dosyası içerir. .ismc dosyası dinamik akış performansını artırmaya yardımcı olur. 

> [!NOTE]
> İstemci bildirimi (.ismc) dosyasının sözdizimi dahili kullanım için ayrılmıştır. Gelecek sürümlerde değişebilir. Bu dosyanın içeriğini değiştirmeyin veya değiştirmeyin.
> 
> 

Daha fazla bilgi için [bu bloga](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/)bakın.

### <a name="known-issues"></a>Bilinen sorunlar
Bazı istemciler, Düzgün Akış bildiriminde yineetiketi sorunuyla karşılaşabilir. Daha fazla bilgi için [bu bölüme](media-services-deliver-content-overview.md#known-issues)bakın.

## <a name="april-2016-release"></a><a id="apr_changes16"></a>Nisan 2016 sürümü
### <a name="media-analytics"></a>Media Analytics
 Medya Hizmetleri, güçlü video zekası için Media Analytics'i tanıttı. Daha fazla bilgi için Medya [Hizmetleri Analizi'ne genel bakış](media-services-analytics-overview.md)bölümüne bakın.

### <a name="apple-fairplay-preview"></a>Apple FairPlay (önizleme)
Artık Apple FairPlay ile HTTP Live Streaming (HLS) içeriğinizi dinamik olarak şifrelemek için Medya Hizmetlerini kullanabilirsiniz. Ayrıca, müşterilere FairPlay lisansları sunmak için Medya Hizmetleri lisans teslim hizmetini de kullanabilirsiniz. Daha fazla bilgi için "Apple FairPlay ile korunan HLS içeriğinizi akışaltına almak için Azure Medya Hizmetlerini kullanın" konusuna bakın.

## <a name="february-2016-release"></a><a id="feb_changes16"></a>Şubat 2016 sürümü
.NET (3.5.3) için Medya Hizmetleri SDK'nın en son sürümü, Google Widevine ile ilgili bir hata düzeltmesi içerir. Widevine ile şifrelenmiş birden fazla varlık için AssetDeliveryPolicy'yi yeniden kullanmak imkansızdı. Bu hata düzeltmesinin bir parçası olarak, Aşağıdaki özellik SDK'ya eklendi: WidevineBaseLicenseAcquisitionUrl.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a name="january-2016-release"></a><a id="jan_changes_16"></a>Ocak 2016 sürümü
Kodlayıcı adlarıyla karışıklığı azaltmak için ayrılmış birimlerin kodlanması yeniden adlandırıldı.

Temel, Standart ve Premium kodlama ayrılmış birimleri sırasıyla S1, S2 ve S3 ayrılmış birimleri olarak yeniden adlandırıldı. Bugün Temel kodlama ayrılmış birimleri kullanan müşteriler, Azure portalında (ve faturada) S1'i etiket olarak görür. Standart ve Premium kullanan müşteriler sırasıyla S2 ve S3 etiketlerini görürler. 

## <a name="december-2015-release"></a><a id="dec_changes_15"></a>Aralık 2015 sürümü

### <a name="media-encoder-deprecation-announcement"></a>Media Encoder amortisman duyurusu

 Media Encoder, Media Encoder Standard'ın yayınlanmasından itibaren yaklaşık 12 ay içinde amortismana sokulacaktır.

### <a name="azure-sdk-for-php"></a>PHP için Azure SDK
Azure SDK ekibi, Medya Hizmetleri için güncelleştirmeler ve yeni özellikler içeren PHP paketi [için Azure SDK'nın](https://github.com/Azure/azure-sdk-for-php) yeni bir sürümü yayınladı. Özellikle, PHP için Medya Hizmetleri SDK artık en son [içerik koruma](media-services-content-protection-overview.md) özelliklerini destekler. Bu özellikler, belirteç kısıtlamaları olan ve olmayan AES ve DRM (PlayReady ve Widevine) ile dinamik şifrelemedir. Ayrıca ölçekleme [kodlama birimlerini](media-services-dotnet-encoding-units.md)destekler.

Daha fazla bilgi için bkz.

* Aşağıdaki [kod örnekleri](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) hızlı bir şekilde başlamanıza yardımcı olur:
  * **vodworkflow_aes.php**: Bu PHP dosyası, AES-128 dinamik şifrelemeve anahtar teslim hizmetinin nasıl kullanılacağını gösterir. [AES-128 dinamik şifreleme](media-services-protect-with-aes128.md)ve anahtar teslim hizmetinde açıklanan .NET örneğine dayanmaktadır.
  * **vodworkflow_aes.php**: Bu PHP dosyası PlayReady dinamik şifrelemeve lisans teslim hizmetinin nasıl kullanılacağını gösterir. [PlayReady ve/veya Widevine dinamik ortak şifrelemesinde](media-services-protect-with-playready-widevine.md)açıklanan .NET örneğine dayanmaktadır.
  * **scale_encoding_units.php**: Bu PHP dosyası, ayrılmış birimlerin kodlanmasının nasıl ölçeklendirilebildiğini gösterir.

## <a name="november-2015-release"></a><a id="nov_changes_15"></a>Kasım 2015 sürümü
 Medya Hizmetleri artık bulutta Widevine lisans teslim hizmeti sunmaktadır. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/)bakın. Ayrıca, [bu öğretici](media-services-protect-with-playready-widevine.md) ve [GitHub deposubakın.](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm) 

Medya Hizmetleri tarafından sağlanan Widevine lisans teslim hizmetleri önizlemededir. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/)bakın.

## <a name="october-2015-release"></a><a id="oct_changes_15"></a>Ekim 2015 sürümü
Medya Hizmetleri şu anda aşağıdaki veri merkezlerinde yayınlanmaktadır: Brezilya Güney, Hindistan Batı, Hindistan Güney ve Hindistan Orta. Artık Medya Hizmeti hesapları [oluşturmak](media-services-portal-create-account.md) ve Medya Hizmetleri [dokümantasyon web sayfasında](https://azure.microsoft.com/documentation/services/media-services/)açıklanan çeşitli görevleri gerçekleştirmek için Azure portalını kullanabilirsiniz. Bu veri merkezlerinde Canlı Kodlama etkinleştirildi. Ayrıca, bu veri merkezlerinde her türlü kodlama ayrılmış birim bulunmaz.

* Brezilya Güney: Yalnızca Standart ve Temel kodlama ayrılmış birimler mevcuttur.
* Hindistan Batı, Hindistan Güney ve Hindistan Orta: Sadece Temel kodlama ayrılmış birimleri mevcuttur.

## <a name="september-2015-release"></a><a id="september_changes_15"></a>Eylül 2015 sürümü
Medya Hizmetleri artık Widevine modüler DRM teknolojisi ile hem isteğe bağlı video hem de canlı akışları koruma olanağı sunuyor. Widevine lisanslarını sunmanıza yardımcı olmak için aşağıdaki teslimat hizmetleri ortaklarını kullanabilirsiniz:
* [Axinom](https://www.axinom.com) 
* [EZDRM](https://ezdrm.com/) 
* [castLabs](https://castlabs.com/company/partners/azure/) 

Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)bakın.
  
AssetDeliveryConfiguration’ı Widevine kullanacak şekilde yapılandırmak için [Media Services .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (3.5.1 sürümünden başlayarak) veya REST API'yi kullanabilirsiniz. 
* Media Services, Apple ProRes videoları için destek ekledi. Artık Apple ProRes veya diğer codec'leri kullanan QuickTime kaynak video dosyalarınızı yükleyebilirsiniz. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/)bakın.
* Artık subkslipping ve canlı arşiv çıkarma yapmak için Media Encoder Standard kullanabilirsiniz. Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/)bakın.
* Aşağıdaki filtreleme güncelleştirmeleri yapıldı: 
  
  * Artık Yalnızca ses filtresiyle Apple HLS biçimini kullanabilirsiniz. Bu güncelleştirmeyi, URL'de (yalnızca ses=false) belirterek yalnızca ses kaydını kaldırmak için kullanabilirsiniz.
  * Varlıklarınız için filtreler tanımladığınızda, artık birden çok (en fazla üç) filtreyi tek bir URL'de birleştirebilirsiniz.
    
    Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)bakın.
* Medya Hizmetleri artık HLS sürüm 4'teki I-çerçeveleri desteklemektedir. I-frame desteği ileri sarma ve geri sarma işlemlerini en iyi duruma getirmektedir. Varsayılan olarak, tüm HLS sürüm 4 çıkışları I-frame çalma listesi (EXT-X-I-FRAME-STREAM-INF) içerir.
Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/)bakın.

## <a name="august-2015-release"></a><a id="august_changes_15"></a>Ağustos 2015 sürümü
* Java sürümü 0.8.0 sürümü için Medya Hizmetleri SDK ve yeni örnekler artık mevcuttur. Daha fazla bilgi için bkz.
    
* Azure Media Player çok sesli akış desteği ile güncellendi. Daha fazla bilgi için [bu blog gönderisi](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)bakın.

## <a name="july-2015-release"></a><a id="july_changes_15"></a>Temmuz 2015 sürümü
* Media Encoder Standard'ın genel durumu açıklandı. Daha fazla bilgi için [bu blog gönderisi](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)bakın.
  
    Media Encoder [Standard, bu bölümde](https://go.microsoft.com/fwlink/?LinkId=618336)açıklandığı gibi hazır ayarları kullanır. 4K kodlar için önceden ayarlanmış bir kullanım yaptığınızda, Premium ayrılmış birim türünü alın. Daha fazla bilgi için [bkz.](media-services-scale-media-processing-overview.md)
* Medya Hizmetleri ve Media Player ile gerçek zamanlı altyazılar kullanıldı. Daha fazla bilgi için [bu blog gönderisi](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)bakın.

### <a name="media-services-net-sdk-updates"></a>Medya Hizmetleri .NET SDK güncellemeleri
Medya Hizmetleri .NET SDK şimdi sürüm 3.4.0.0. Aşağıdaki güncelleştirmeler yapıldı: 

* Canlı arşiv için destek uygulandı. Canlı arşiv içeren bir varlığı indiremezsiniz.
* Dinamik filtreler için destek uygulandı.
* İşlevsellik, kullanıcıların bir varlığı silerken bir depolama kapsayıcısı tutabilmeleri için uygulandı.
* Hata düzeltmeleri kanallardaki yeniden deneme ilkeleriyle ilgili olarak yapıldı.
* Media Encoder Premium İş Akışı etkinleştirildi.

## <a name="june-2015-release"></a><a id="june_changes_15"></a>Haziran 2015 sürümü
### <a name="media-services-net-sdk-updates"></a>Medya Hizmetleri .NET SDK güncellemeleri
Medya Hizmetleri .NET SDK şimdi sürüm 3.3.0.0. Aşağıdaki güncelleştirmeler yapıldı: 

* OpenId Connect keşif spec için destek eklendi.
* Kimlik sağlayıcı tarafında anahtarları nakışişleme için destek eklendi.

Bir OpenID Connect bulma belgesini ortaya çıkaran bir kimlik sağlayıcısı kullanıyorsanız (Azure AD, Google ve Salesforce'un yaptığı gibi), Medya Hizmetleri'ne OpenID Connect keşif spec'inden JSON Web Belirteçleri'nin (JWTs) doğrulanması için imzalama anahtarları alması için talimat verebilirsiniz. 

Daha fazla bilgi için, [Medya Hizmetlerinde JWT kimlik doğrulaması ile çalışmak için OpenID Connect keşif spec'inden JSON web anahtarlarını kullanın'](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/)a bakın.

## <a name="may-2015-release"></a><a id="may_changes_15"></a>Mayıs 2015 sürümü
Aşağıdaki yeni özellikler açıklandı:

* [Medya Hizmetleri ile canlı kodlama önizleme](media-services-manage-live-encoder-enabled-channels.md)
* [Dinamik manifesto](media-services-dynamic-manifest-overview.md)

## <a name="april-2015-release"></a><a id="april_changes_15"></a>Nisan 2015 sürümü
### <a name="general-media-services-updates"></a>Genel Medya Hizmetleri güncellemeleri
* [Media Player](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/) açıklandı.
* Media Services REST 2.10 ile başlayarak, Gerçek Zamanlı Mesajlaşma Protokolü (RTMP) yutacak şekilde yapılandırılan kanallar birincil ve ikincil yutma URL'leri ile oluşturulur. Daha fazla bilgi için [Kanal yapılandırmaları'na](media-services-live-streaming-with-onprem-encoders.md#channel_input)bakın.
* Azure Media Indexer güncelleştirildi.
* İspanyolca dil desteği eklendi.
* XML biçimi için yeni bir yapılandırma eklendi.

Daha fazla bilgi için [bu bloga](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/)bakın.

### <a name="media-services-net-sdk-updates"></a>Medya Hizmetleri .NET SDK güncellemeleri
Medya Hizmetleri .NET SDK şimdi sürüm 3.2.0.0. Aşağıdaki güncelleştirmeler yapıldı:

* Son dakika değişikliği: TokenRestrictionTemplate.Issuer ve TokenRestrictionTemplate.Audience bir dize türünde olarak değiştirildi.
* Güncelleştirmeler özel yeniden deneme ilkeleri oluşturma ile ilgili yapıldı.
* Dosya yükleme ve indirme ile ilgili hata düzeltmeleri yapıldı.
* MediaServicesCredentials sınıfı artık karşı kimlik doğrulaması yapmak için birincil ve ikincil erişim denetimi uç noktalarını kabul eder.

## <a name="march-2015-release"></a><a id="march_changes_15"></a>Mart 2015 sürümü
### <a name="general-media-services-updates"></a>Genel Medya Hizmetleri güncellemeleri
* Medya Hizmetleri artık İçerik Dağıtım Ağı tümleştirmesi sağlar. Tümleştirmeyi desteklemek için, CdnEnabled özelliği StreamingEndpoint'e eklendi. CdnEnabled sürüm 2.9 ile başlayan REST API'leri ile kullanılabilir. Daha fazla bilgi için [Bkz. StreamingEndpoint.](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint) CdnEnabled 3.1.0.2 sürümünden başlayarak .NET SDK ile kullanılabilir. Daha fazla bilgi için [Bkz. StreamingEndpoint.](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx)
* Media Encoder Premium İş Akışı açıklandı. Daha fazla bilgi için Azure [Medya Hizmetleri'nde Premium kodlama yı tanıtma 'ya](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)bakın.

## <a name="february-2015-release"></a><a id="february_changes_15"></a>Şubat 2015 sürümü
### <a name="general-media-services-updates"></a>Genel Medya Hizmetleri güncellemeleri
Medya Hizmetleri REST API şimdi sürüm 2.9 olduğunu. Bu sürümden başlayarak, akış uç noktalarıyla İçerik Dağıtım Ağı tümleştirmesini etkinleştirebilirsiniz. Daha fazla bilgi için [Bkz. StreamingEndpoint.](https://msdn.microsoft.com/library/dn783468.aspx)

## <a name="january-2015-release"></a><a id="january_changes_15"></a>Ocak 2015 sürümü
### <a name="general-media-services-updates"></a>Genel Medya Hizmetleri güncellemeleri
Dinamik şifreleme ile içerik korumasının genel kullanılabilirliği duyuruldu. Daha fazla bilgi için [bkz.](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/)

### <a name="media-services-net-sdk-updates"></a>Medya Hizmetleri .NET SDK güncellemeleri
Medya Hizmetleri .NET SDK şimdi sürüm 3.1.0.1.

Bu sürüm varsayılan Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate constructor eski olarak işaretlenmiş. Yeni oluşturucu, TokenType'ı bağımsız değişken olarak alır.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a name="december-2014-release"></a><a id="december_changes_14"></a>Aralık 2014 sürümü
### <a name="general-media-services-updates"></a>Genel Medya Hizmetleri güncellemeleri
* Media Indexer'a bazı güncelleştirmeler ve yeni özellikler eklendi. Daha fazla bilgi için Azure [Media Indexer sürüm 1.1.6.7 sürüm notlarına](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/)bakın.
* Ayrılmış birimleri kodlamayı güncelleştirmek için kullanabileceğiniz yeni bir REST API eklendi. Daha fazla bilgi için [bkz: ENcodingReservedUnitType with REST](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* Anahtar teslim hizmeti için CORS desteği eklendi.
* Yetkilendirme ilkesi seçeneklerini sorgulamada performans iyileştirmeleri yapıldı.
* Çin veri merkezinde, [anahtar teslim](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) URL'si artık müşteri başınadır (diğer veri merkezlerinde olduğu gibi).
* HLS otomatik hedef süresi eklendi. Canlı akış yaparken HLS her zaman dinamik olarak paketlenir. Varsayılan olarak, Medya Hizmetleri anahtar kare aralığına (KeyFrameInterval) göre HLS segment ambalaj oranını (FragmentsPerSegment) otomatik olarak hesaplar. Bu yöntem, canlı kodlayıcıdan alınan bir resim grubu (GOP) olarak da adlandırılır. Daha fazla bilgi için bkz: [Medya Hizmetleri yle Çalışma canlı akışı.](https://msdn.microsoft.com/library/azure/dn783466.aspx)

### <a name="media-services-net-sdk-updates"></a>Medya Hizmetleri .NET SDK güncellemeleri
[Medya Hizmetleri .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) şimdi sürüm 3.1.0.0. Aşağıdaki güncelleştirmeler yapıldı:

* .NET SDK bağımlılığı .NET 4.5 Çerçevesi'ne yükseltildi.
* Ayrılmış birimleri kodlamayı güncelleştirmek için kullanabileceğiniz yeni bir API eklendi. Daha fazla bilgi için bkz: [ayrılmış birim türünü güncelleştir in ve .NET'i kullanarak ayrılmış birimleri kodlamayı artırın.](media-services-dotnet-encoding-units.md)
* Belirteç kimlik doğrulaması için JWT desteği eklendi. Daha fazla bilgi için, [Medya Hizmetleri'nde JWT belirteç kimlik doğrulaması ve dinamik şifreleme](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)ye bakın.
* PlayReady lisans şablonuna BeginDate ve ExpirationDate için göreli uzaklıklar eklendi.

## <a name="november-2014-release"></a><a id="november_changes_14"></a>Kasım 2014 sürümü
* Artık Medya Hizmetlerini, Canlı Düzgün Akış (fMP4) içeriğini Bir SSL bağlantısı üzerinden yutmak için kullanabilirsiniz. SSL üzerinden yutmak için, https için yutulan URL'yi güncelleştirdiğinden emin olun. Şu anda, Medya Hizmetleri özel etki alanları ile SSL desteklemiyor. Canlı akış hakkında daha fazla bilgi için Azure [Medya Hizmetleri Yle Çalışma'ya](https://msdn.microsoft.com/library/azure/dn783466.aspx)bakın.
* Şu anda, bir RTMP canlı akışı bir SSL bağlantısı üzerinden yutamaz.
* Yalnızca içeriğinizi teslim ettiğiniz akış bitiş noktası 10 Eylül 2014'ten sonra oluşturulduysa SSL üzerinden akış yapabilirsiniz. Akış URL'leriniz 10 Eylül 2014'ten sonra oluşturulan akış uç noktalarını temel alıyorsa, URL "streaming.mediaservices.windows.net" (yeni biçim) içerir. "origin.mediaservices.windows.net" (eski biçim) içeren akış URL'leri SSL'yi desteklemez. URL'niz eski biçimdeyse ve SSL üzerinden akış yapmak istiyorsanız, [yeni bir akış bitiş noktası oluşturun.](media-services-portal-manage-streaming-endpoints.md) İçeriğinizi SSL üzerinden aktarmak için yeni akış bitiş noktasına dayalı URL'ler kullanın.

### <a name="media-services-net-sdk"></a><a id="oct_sdk"></a>Media Services .NET SDK
.NET uzantıları için Medya Hizmetleri SDK şimdi sürüm 2.0.0.3.

.NET için Medya Hizmetleri SDK şimdi sürüm 3.0.0.8 olduğunu. Aşağıdaki güncelleştirmeler yapıldı:

* Yeniden deneme ilkesi sınıflarında yeniden düzenleme uygulandı.
* HTTP istek üstbilgilerine bir kullanıcı aracısı dizesi eklendi.
* NuGet geri yükleme oluşturma adımı eklendi.
* Senaryo testleri depodan x509 sertifika kullanmak için sabit edildi.
* Kanal ve akış sonu güncelleştirildiğinde doğrulama ayarları eklendi.

### <a name="new-github-repository-to-host-media-services-samples"></a>Medya Hizmetleri örneklerini barındıracak yeni GitHub deposu
Örnekler Medya [Hizmetleri örnekleri GitHub deposubulunmaktadır.](https://github.com/Azure/Azure-Media-Services-Samples)

## <a name="september-2014-release"></a><a id="september_changes_14"></a>Eylül 2014 sürümü
Medya Hizmetleri REST meta verileri artık sürüm 2.7'dir. En son REST güncellemeleri hakkında daha fazla bilgi için [Medya Hizmetleri REST API referansına](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference)bakın.

.NET için Medya Hizmetleri SDK şimdi sürüm 3.0.0.7

### <a name="breaking-changes"></a><a id="sept_14_breaking_changes"></a>Yeni değişiklikler
* Origin, [StreamingEndpoint]olarak yeniden adlandırıldı.
* MP4 dosyalarını kodlamak ve yayımlamak için Azure portalını kullandığınızda varsayılan davranışta bir değişiklik yapıldı.

### <a name="new-featuresscenarios-that-are-part-of-the-general-availability-release"></a><a id="sept_14_GA_changes"></a>Genel kullanılabilirlik sürümünden bir parçası olan yeni özellikler/senaryolar
* Media Indexer ortam işlemcisi tanıtıldı. Daha fazla bilgi için [Media Index media dosyalarını Media Indexer ile](https://msdn.microsoft.com/library/azure/dn783455.aspx)birlikte görün.
* Özel etki alanı (ana bilgisayar) adları eklemek için [StreamingEndpoint] varlığını kullanabilirsiniz.
  
    Medya Hizmetleri akışı uç nokta adı olarak özel bir etki alanı adı kullanmak için, akış bitiş noktanıza özel ana bilgisayar adları ekleyin. Özel ana bilgisayar adları eklemek için Medya Hizmetleri REST API'lerini veya .NET SDK'yı kullanın.
  
    Aşağıdaki noktalara dikkat edilmelidir:
  
  * Özel etki alanı adının sahipliğine sahip olmalısınız.
  * Etki alanı adının sahipliği Media Services tarafından doğrulanmalıdır. Etki alanını doğrulamak için, DNS mediaservices-dns-bölgesini doğrulamak için MediaServicesAccountId üst etki alanını eşleyen bir CName oluşturun.
  * Özel ana bilgisayar adını (örneğin, sports.contoso.com) Medya Hizmetleri AkışıSon nokta ana adınız (örneğin, amstest.streaming.mediaservices.windows.net) eşleyen başka bir CName oluşturmanız gerekir.

    Daha fazla bilgi [için, StreamingEndpoint](https://msdn.microsoft.com/library/azure/dn783468.aspx) makalesinde CustomHostNames özelliğine bakın.

### <a name="new-featuresscenarios-that-are-part-of-the-public-preview-release"></a><a id="sept_14_preview_changes"></a>Genel önizleme sürümünde yer alan yeni özellikler/senaryolar
* Canlı akış önizlemesi. Daha fazla bilgi için bkz: [Medya Hizmetleri yle Çalışma canlı akışı.](https://msdn.microsoft.com/library/azure/dn783466.aspx)
* Anahtar teslim hizmeti. Daha fazla bilgi için [AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanın'](https://msdn.microsoft.com/library/azure/dn783457.aspx)a bakın.
* AES dinamik şifreleme. Daha fazla bilgi için [AES-128 dinamik şifreleme ve anahtar teslim hizmetini kullanın'](https://msdn.microsoft.com/library/azure/dn783457.aspx)a bakın.
* PlayReady lisans teslim hizmeti. 
* PlayReady dinamik şifreleme. 
* Medya Hizmetleri PlayReady lisans şablonu. Daha fazla bilgi için [Medya Hizmetleri PlayReady lisans şablonuna genel bakış]abakın.
* Depolama yla şifrelenmiş varlıkları akış. Daha fazla bilgi için [Akış depolama alanı yla şifrelenmiş içeriğe](https://msdn.microsoft.com/library/azure/dn783451.aspx)bakın.

## <a name="august-2014-release"></a><a id="august_changes_14"></a>Ağustos 2014 sürümü
Bir varlığı kodladığınızda, kodlama işi tamamlandığında bir çıktı kıymeti üretilir. Bu sürüme kadar, Media Services Encoder çıktı varlıkları hakkında meta veriler üretti. Bu sürümden başlayarak, kodlayıcı giriş varlıkları hakkında meta veriler de üretir. Daha fazla bilgi için [Giriş meta verileri] ve Çıktı meta [verilerine]bakın.

## <a name="july-2014-release"></a><a id="july_changes_14"></a>Temmuz 2014 sürümü
Azure Medya Hizmetleri Paketleyicisi ve Şifreleyicisi için aşağıdaki hata düzeltmeleri yapılmıştır:

* Canlı arşiv varlığı HLS'ye aktarıldığında, yalnızca ses oynatılır: Bu sorun giderildi ve artık hem ses hem de video oynatılabilir.
* Bir varlık HLS ve AES 128 bit zarf şifrelemesine paketlendiğinde, paketlenmiş akışlar Android cihazlarda oynatılamaz: Bu hata düzeltildi ve paketlenmiş akış HLS'yi destekleyen Android cihazlarda oynatılır.

## <a name="may-2014-release"></a><a id="may_changes_14"></a>Mayıs 2014 sürümü
### <a name="general-media-services-updates"></a><a id="may_14_changes"></a>Genel Medya Hizmetleri güncellemeleri
HlS sürüm 3'ün akışını sağlamak için [artık dinamik ambalajı] kullanabilirsiniz. HLS sürüm 3'ü aktarmak için, kaynak bulucu yoluna aşağıdaki biçimi ekleyin: * .ism/manifest(format=m3u8-aapl-v3). Daha fazla bilgi için [bu foruma](https://social.msdn.microsoft.com/Forums/en-US/13b8a776-9519-4145-b9ed-d2b632861fde/dynamic-packaging-to-hls-v3)bakın.

Dinamik paketleme artık PlayReady ile statik olarak şifrelenmiş Düzgün Akış'a dayalı olarak PlayReady ile şifrelenmiş HLS (sürüm 3 ve sürüm 4) teslimini de destekler. PlayReady ile Düzgün Akış'ı nasıl şifreleyecekleri hakkında bilgi için [PlayReady ile Düzgün Akışı Koru](https://msdn.microsoft.com/library/azure/dn189154.aspx)konusuna bakın.

### <a name="media-services-net-sdk-updates"></a><a name="may_14_donnet_changes"></a>Medya Hizmetleri .NET SDK güncellemeleri
Medya Hizmetleri .NET SDK şimdi sürüm 3.0.0.5 olduğunu. Aşağıdaki güncelleştirmeler yapıldı:

* Medya varlıklarını yüklerve indirirken hız ve esneklik daha iyidir.
* Yeniden deneme mantığı ve geçici özel durum işlemede iyileştirmeler yapıldı: 
  
  * Sorgularken, değişiklikleri kaydettiğinizde ve dosyaları yüklediğinizde veya indirdiğinizde oluşan özel durumlar için geçici hata algılama ve yeniden deneme mantığı iyileştirilmiştir. 
  * Web özel durumları aldığınızda (örneğin, Bir Erişim Denetim Hizmeti belirteç isteği sırasında), önemli hatalar şimdi daha hızlı başarısız olur.

Daha fazla bilgi için [,.NET için Medya Hizmetleri SDK'daki Yeniden Deneme mantığına]bakın.

## <a name="januaryfebruary-2014-releases"></a><a id="jan_feb_changes_14"></a>Ocak/Şubat 2014 bültenleri
### <a name="media-services-net-sdk-3001-3002-and-3003"></a><a name="jan_fab_14_donnet_changes"></a>Medya Hizmetleri .NET SDK 3.0.0.1, 3.0.0.2 ve 3.0.0.3
3.0.0.1 ve 3.0.0.2'deki değişiklikler şunlardır:

* OrderBy deyimleri ile LINQ sorgularının kullanımına ilişkin sorunlar giderildi.
* [GitHub'daki] test çözümleri birim tabanlı testlere ve senaryo tabanlı testlere ayrılmıştır.

Değişiklikler hakkında daha fazla bilgi için [Medya Hizmetleri .NET SDK 3.0.0.1 ve 3.0.0.2 sürümlerine](http://gtrifonov.com/2014/02/07/windows-azure-media-services-net-sdk-3-0-0-2-release/index.html)bakın.

3.0.0.3 sürümünde aşağıdaki değişiklikler yapılmıştır:

* Azure depolama bağımlılıkları sürüm 3.0.3.0'ı kullanacak şekilde yükseltildi.
* Geriye dönük uyumluluk sorunu 3.0 için düzeltildi. *.* Serbest bırakır.

## <a name="december-2013-release"></a><a id="december_changes_13"></a>Aralık 2013 sürümü
### <a name="media-services-net-sdk-3000"></a><a name="dec_13_donnet_changes"></a>Medya Hizmetleri .NET SDK 3.0.0.0
> [!NOTE]
> 3.0.x.x salınımları geriye doğru 2.4.x.x serbest bırakmaları ile uyumlu değildir.
> 
> 

Medya Hizmetleri SDK en son sürümü şimdi 3.0.0.0. En son paketi NuGet'den indirebilir veya [GitHub'dan]bitleri alabilirsiniz.

Medya Hizmetleri SDK sürümü 3.0.0.0'dan başlayarak [Azure AD Erişim Denetim Hizmeti](https://msdn.microsoft.com/library/hh147631.aspx) belirteçlerini yeniden kullanabilirsiniz. Daha fazla bilgi için [,.NET için Medya Hizmetleri SDK ile Medya Hizmetlerine Bağlan'daki](https://msdn.microsoft.com/library/azure/jj129571.aspx)"Access Control Service belirteçlerini yeniden kullan" bölümüne bakın.

### <a name="media-services-net-sdk-extensions-2000"></a><a name="dec_13_donnet_ext_changes"></a>Medya Hizmetleri .NET SDK uzantıları 2.0.0.0
 Medya Hizmetleri .NET SDK uzantıları, kodunuzu basitleştiren ve Medya Hizmetleri ile geliştirmeyi kolaylaştıran bir dizi uzantı yöntemi ve yardımcı işlevidir. [Medya Hizmetleri .NET SDK uzantılarından](https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev)en son bitleri alabilirsiniz.

## <a name="november-2013-release"></a><a id="november_changes_13"></a>Kasım 2013 sürümü
### <a name="media-services-net-sdk-changes"></a><a name="nov_13_donnet_changes"></a>Medya Hizmetleri .NET SDK değişiklikleri
Bu sürümden başlayarak,.NET için Medya Hizmetleri SDK, Media Services REST API katmanına çağrı yapıldığında oluşabilecek geçici hata hatalarını işler.

## <a name="august-2013-release"></a><a id="august_changes_13"></a>Ağustos 2013 sürümü
### <a name="media-services-powershell-cmdlets-included-in-azure-sdk-tools"></a><a name="aug_13_powershell_changes"></a>Azure SDK araçlarında yer alan Medya Hizmetleri PowerShell cmdletleri
Aşağıdaki Medya Hizmetleri PowerShell cmdlets artık [Azure SDK araçlarına](https://github.com/Azure/azure-sdk-tools)dahildir:

* AzureMediaHizmetleri Al 

    Örneğin, `Get-AzureMediaServicesAccount`
* New-AzureMediaServicesAccount 
  
    Örneğin, `New-AzureMediaServicesAccount -Name "MediaAccountName" -Location "Region" -StorageAccountName "StorageAccountName"`
* New-AzureMediaServicesKey 
  
    Örneğin, `New-AzureMediaServicesKey -Name "MediaAccountName" -KeyType Secondary -Force`
* Remove-AzureMediaServicesAccount 
  
    Örneğin, `Remove-AzureMediaServicesAccount -Name "MediaAccountName" -Force`

## <a name="june-2013-release"></a><a id="june_changes_13"></a>Haziran 2013 sürümü
### <a name="media-services-changes"></a><a name="june_13_general_changes"></a>Medya Hizmetleri değişiklikleri
Bu bölümde belirtilen aşağıdaki değişiklikler, Haziran 2013 Medya Hizmetleri sürümlerinde yer alan güncelleştirmelerdir:

* Birden çok depolama hesabını bir Medya Hizmeti hesabına bağlama özelliği. 
    * StorageAccount
    * Varlık.DepolamaHesap Adı ve Varlık.DepolamaHesabı
* Job.Priority'i güncelleme yeteneği. 
* Bildirimle ilgili varlıklar ve özellikler: 
    * İş BildirimleriAbonelik
    * BildirimEndPoint
    * İş
* Varlık.Uri 
* Locator.Name 

### <a name="media-services-net-sdk-changes"></a><a name="june_13_dotnet_changes"></a>Medya Hizmetleri .NET SDK değişiklikleri
Aşağıdaki değişiklikler Haziran 2013 Medya Hizmetleri SDK sürümlerinde yer aldı. En son Medya Hizmetleri SDK GitHub mevcuttur.

* Medya Hizmetleri SDK, sürüm 2.3.0.0 ile başlayarak birden fazla depolama hesabını Bir Medya Hizmetleri hesabına bağlamayı destekler. Aşağıdaki API'ler bu özelliği destekler:
  
    * IStorageAccount türü
    * Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts özelliği
    * StorageAccount özelliği
    * StorageAccountName özelliği
  
      Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/azure/dn271889.aspx)
* Bildirimle ilgili API'ler. Sürüm 2.2.0.0'dan başlayarak Azure Kuyruk depolama bildirimlerini dinleyebilirsiniz. Daha fazla bilgi için [bkz.](https://msdn.microsoft.com/library/azure/dn261241.aspx)
  
    * Microsoft.WindowsAzure.MediaServices.Client.IJob.JobBildirimSubscriptions özelliği
    * Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint türü
    * Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription türü
    * Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection türü
    * Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType türü
* Depolama istemcisi SDK 2.0 bağımlılık (Microsoft.WindowsAzure.StorageClient.dll)
* OData 5.5'e bağımlılık (Microsoft.Data.OData.dll)

## <a name="december-2012-release"></a><a id="december_changes_12"></a>Aralık 2012 sürümü
### <a name="media-services-net-sdk-changes"></a><a name="dec_12_dotnet_changes"></a>Medya Hizmetleri .NET SDK değişiklikleri
* IntelliSense: Eksik IntelliSense belgeleri birçok tür için eklendi.
* Microsoft.Practices.TransientFaultHandling.Core: SDK'nın bu derlemenin eski bir sürümüne hala bağımlı olduğu bir sorun giderildi. SDK şimdi bu derlemenin 5.1.1209.1 sürümüne başvuruyor.

Kasım 2012 SDK'da bulunan sorunlar için düzeltmeler:

* IAsset.Locators.Count: Bu sayı, tüm bulucular silindikten sonra yeni IAsset arabirimlerinde doğru bir şekilde bildirilir.
* iAssetFile.ContentFileSize: Bu değer artık IAssetFile.Upload (filepath) tarafından yüklendikten sonra düzgün bir şekilde ayarlanır.
* iAssetFile.ContentFileSize: Bir varlık dosyası oluşturduğunuzda bu özellik artık ayarlanabilir. Daha önce sadece okundu.
* IAssetFile.Upload(filepath): Bu senkron yükleme yönteminin varlığa birden çok dosya yüklendiğinde aşağıdaki hatayı attığı bir sorun düzeltildi. Hata "Sunucu isteği doğrulamak için başarısız oldu. İmza da dahil olmak üzere Yetkilendirme üstbilgisinin değerinin doğru oluştuğundan emin olun."
* IAssetFile.UploadAsync: Dosyaların eşzamanlı olarak beş dosyayla yüklenmesini sınırlayan bir sorun düzeltildi.
* IAssetFile.UploadProgressChanged: Bu olay artık SDK tarafından sağlanmaktadır.
* iAssetFile.DownloadAsync(string, BlobTransferClient, ILocator, CancellationToken): Bu yöntem aşırı yükleme şimdi sağlanır.
* IAssetFile.DownloadAsync: Dosyaların eşzamanlı olarak karşıdan yüklemeyi beş dosyayla sınırlandıran bir sorun düzeltildi.
* iAssetFile.Delete(): IAssetFile için dosya yüklenmediyse, arama silme nin özel durum atabileceği bir sorun düzeltildi.
* İşler: Bir iş şablonu kullanarak "PlayReady Protection Task" ile "MP4 to Smooth Streams görevi" zincirleme hiçbir görev oluşturmadığı bir sorun düzeltildi.
* EncryptionUtils.GetCertificateFromStore(): Bu yöntem, sertifika yapılandırma sorunlarına dayalı sertifikayı bulmadaki bir hata nedeniyle artık geçersiz bir başvuru istisnası düzenlemez.

## <a name="november-2012-release"></a><a id="november_changes_12"></a>Kasım 2012 sürümü
Bu bölümde belirtilen değişiklikler, Kasım 2012 (sürüm 2.0.0.0) SDK'da yer alan güncelleştirmelerdi. Bu değişiklikler, Haziran 2012 önizleme SDK sürümü için yazılmış herhangi bir kodun değiştirilmesini veya yeniden yazılmasını gerektirebilir.

* Varlıklar
  
    * IAsset.Create(assetName) *tek* varlık oluşturma işlevidir. iAsset.Create artık yöntem aramasının bir parçası olarak dosyaları yüklemez. Yüklemek için IAssetFile'ı kullanın.
    * IAsset.Publish yöntemi ve AssetState.Publish numaralandırma değeri Hizmetler SDK kaldırıldı. Bu değere dayanan tüm kodlar yeniden yazılmalıdır.
* Fileınfo
  
    * Bu sınıf kaldırıldı ve yerine IAssetFile dosyalandı.
  
* IAssetFiles
  
    * IAssetFile FileInfo'nun yerini alır ve farklı bir davranışa sahiptir. Bunu kullanmak için, IAssetFiles nesnesini anlık olarak, ardından Medya Hizmetleri SDK'sını veya Depolama SDK'sını kullanarak bir dosya yüklemesi ekleyin. Aşağıdaki IAssetFile.Upload aşırı yükleri kullanılabilir:
  
        * IAssetFile.Upload(filePath): Bu senkron yöntem iş parçacığı engeller ve biz sadece tek bir dosya yüklediğinizde öneririz.
        * IAssetFile.UploadAsync(filePath, blobTransferClient, locator, cancellationToken): Bu asynchronous yöntemi tercih edilen yükleme mekanizmasıdır. 
    
            Bilinen hata: İptal belirteci kullanırsanız, yükleme iptal edilir. Görevlerin birçok iptal durumları olabilir. Özel durumları düzgün bir şekilde yakalamalı ve işlemelisiniz.
* Locators
  
    * Kökene özgü sürümler kaldırıldı. SAS'a özel bağlam. Locators.CreateSasLocator (varlık, accessPolicy) genel kullanılabilirlik tarafından amortismana veya kaldırıldı işaretlenir. Güncelleştirilmiş davranış için "Yeni işlevsellik" altındaki "Konum belirleyiciler" bölümüne bakın.

## <a name="june-2012-preview-release"></a><a id="june_changes_12"></a>Haziran 2012 önizleme sürümü
Aşağıdaki işlevsellik SDK Kasım sürümünde yeni oldu:

* Varlıkları silme
  
    * IAsset, IAssetFile, ILocator, IAccessPolicy ve IContentKey nesneleri artık nesne düzeyinde silinir, yani, IObject.Delete(), Toplama bir silme gerektiren yerine, yani cloudMediaContext.ObjCollection.Delete(objInstance).
* Locators
  
    * Locator artık CreateLocator yöntemi kullanılarak oluşturulmalıdır. Onlar kurmak istediğiniz bulucu belirli türü için bir argüman olarak LocatorType.SAS veya LocatorType.OnDemandOrigin enum değerlerini kullanmanız gerekir.
    * İçeriğiniz için kullanılabilir URI'leri edinmeyi kolaylaştırmak için yer buluculara yeni özellikler eklendi. Bu yer belirleyicilerin yeniden tasarımı, gelecekteki üçüncü taraf genişletilebilirliği için daha fazla esneklik sağlar ve ortam istemcisi uygulamaları için kullanım kolaylığını artırır.
* Asynchronous yöntem desteği
  
    * Tüm yöntemlere eşzamanlı destek eklendi.

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Medya Hizmetleri MSDN Forumu]: https://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Medya Hizmetleri REST API başvurusu]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[Media Services pricing details]: https://azure.microsoft.com/pricing/details/media-services/
[Giriş meta verileri]: https://msdn.microsoft.com/library/azure/dn783120.aspx
[Çıkış meta verileri]: https://msdn.microsoft.com/library/azure/dn783217.aspx
[Deliver content]: https://msdn.microsoft.com/library/azure/hh973618.aspx
[Index media files with the Azure Media Indexer]: https://msdn.microsoft.com/library/azure/dn783455.aspx
[AkışBitiş Noktası]: https://msdn.microsoft.com/library/azure/dn783468.aspx
[Work with Media Services live streaming]: https://msdn.microsoft.com/library/azure/dn783466.aspx
[Use AES-128 dynamic encryption and the key delivery service]: https://msdn.microsoft.com/library/azure/dn783457.aspx
[Use PlayReady dynamic encryption and the license delivery service]: https://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: https://azure.microsoft.com/services/preview/
[Media Services PlayReady lisans şablonuna genel bakış]: https://msdn.microsoft.com/library/azure/dn783459.aspx
[Stream storage-encrypted content]: https://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://portal.azure.com
[Dinamik paketleme]: https://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin's blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[Protect Smooth Streaming with PlayReady]: https://msdn.microsoft.com/library/azure/dn189154.aspx
[.NET için Medya Hizmetleri SDK'da mantığı yeniden deneyin]: https://msdn.microsoft.com/library/azure/dn745650.aspx
[Grass Valley announces EDIUS 7 streaming through the cloud]: https://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Control Media Services Encoder output file names]: https://msdn.microsoft.com/library/azure/dn303341.aspx
[Create overlays]: https://msdn.microsoft.com/library/azure/dn640496.aspx
[Stitch video segments]: https://msdn.microsoft.com/library/azure/dn640504.aspx
[Azure Media Services .NET SDK 3.0.0.1 and 3.0.0.2 releases]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure AD Access Control Service]: https://msdn.microsoft.com/library/hh147631.aspx
[Connect to Media Services with the Media Services SDK for .NET]: https://msdn.microsoft.com/library/azure/jj129571.aspx
[Media Services .NET SDK extensions]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[Azure SDK tools]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Manage Media Services assets across multiple Storage accounts]: https://msdn.microsoft.com/library/azure/dn271889.aspx
[Handle Media Services job notifications]: https://msdn.microsoft.com/library/azure/dn261241.aspx

