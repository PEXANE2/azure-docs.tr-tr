---
title: Media Services dinamik şifrelemeyi kullanarak içeriğinizi koruyun-Azure | Microsoft Docs
description: Bu makale, dinamik şifreleme ile içerik korumasına genel bakış sunar. Ayrıca akış protokolleri ve şifreleme türleri hakkında konuşur.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 5d31e4a523fdedf9907e33c70638f07a08461ed1
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310313"
---
# <a name="content-protection-with-dynamic-encryption"></a>Dinamik şifreleme ile içerik koruma

Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlamak için kullanabilirsiniz. Media Services ile, canlı ve isteğe bağlı içeriğinizi Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sisteminden dinamik olarak şifreli olarak dağıtabilirsiniz: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Media Services v3 'de, içerik anahtarı akış Konumlandırıcı ile ilişkilendirilir ( [Bu örneğe](protect-with-aes128.md)bakın). Media Services anahtar teslim hizmetini kullanıyorsanız, sizin için içerik anahtarını Azure Media Services oluşturabilirsiniz. Kendi anahtar teslim hizmetinizi kullanıyorsanız veya iki veri merkezinde aynı içerik anahtarına sahip olmanız gereken yüksek bir kullanılabilirlik senaryosunu işlemeniz gerekiyorsa, içerik anahtarını kendiniz oluşturmalısınız.

Bir akışa bir oynatıcı tarafından istendiğinde Media Services dinamik olarak içeriğinizi AES şifresiz anahtar veya DRM şifreleme kullanarak şifrelemek için belirtilen anahtar kullanır. Akış şifresini çözmek için Media Services anahtar dağıtımı hizmetiyle veya belirtilen anahtar dağıtımı hizmetiyle anahtar player ister. Kullanıcı anahtarı almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen içerik anahtarı ilkesi hizmet tarafından değerlendirilir.

Lisanslar ve anahtarları için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için REST API veya bir Media Services istemci Kitaplığı'nı kullanabilirsiniz.

Aşağıdaki resimde Media Services content protection iş akışı gösterilmektedir: 

![İçerik koruma](./media/content-protection/content-protection.svg)

&#42;*dinamik şifreleme, AES-128 "şifresiz anahtar" ve CBCS CENC destekler. Ayrıntılar için destek matrisi bkz [burada](#streaming-protocols-and-encryption-types).*

Bu makalede, kavramlar ve terminoloji content protection ile Media Services anlamak için ilgili açıklanmaktadır.

## <a name="main-components-of-a-content-protection-system"></a>Bir içerik koruma sisteminin ana bileşenleri

"Content protection" sistem/uygulama tasarımınızı başarıyla tamamlamak için kapsamı çalışmasının tam olarak anlamak için gerekir. Aşağıdaki liste, üç bölümden uygulamanız gereken genel bir bakış sağlar. 

1. Azure Media Services kod
  
   [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) örneği, .NET kullanarak çok DRM sisteminin Media Services v3 ile nasıl uygulanacağını gösterir. Ayrıca, Media Services lisansı/anahtar teslim hizmeti 'nin nasıl kullanılacağını gösterir. Her bir varlığı birden fazla şifreleme türü (AES-128, PlayReady, Widevine, FairPlay) ile şifreleyebilirsiniz. Birlikte kullanılabilecek türler hakkında bilgi almak için bkz. [Akış protokolleri ve şifreleme türleri](#streaming-protocols-and-encryption-types).
  
   Örnekte gösterildiği nasıl yapılır:

   1. [Içerik anahtar ilkeleri](content-key-policy-concept.md)oluşturun ve yapılandırın. İçerik anahtarının (varlıklarınıza güvenli erişim sağlayan) son istemcilere teslim edildiğini yapılandırmak için bir **Içerik anahtarı ilkesi** oluşturursunuz.    

      * JWT içindeki talepler temelinde yetkilendirme denetimi mantığını belirterek lisans teslimi yetkilendirmesi tanımlayın.
      * [PlayReady](playready-license-template-overview.md), [Widevine](widevine-license-template-overview.md)ve/veya [Fairplay](fairplay-license-overview.md) lisanslarını yapılandırın. Şablonlar, kullanılan her bir DRMs için hakları ve izinleri yapılandırmanızı sağlar.

        ```
        ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
        ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
        ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
        ```
   2. Şifrelenmiş varlığı akışa almak için yapılandırılmış bir [akış Bulucu](streaming-locators-concept.md) oluşturun. 
  
      **Akış bulucunun** bir [akış ilkesiyle](streaming-policy-concept.md)ilişkilendirilmesi vardır. Örnekte, StreamingLocator. StreamingPolicyName öğesini "Predefined_MultiDrmCencStreaming" ilkesine ayarlayacağız. PlayReady ve Widevine şifrelemeleri uygulanır, anahtar, yapılandırılmış DRM lisanslarına göre kayıttan yürütme istemcisine gönderilir. Akışınızı CBCS (FairPlay) ile de şifrelemek isterseniz "Predefined_MultiDrmStreaming" öğesini kullanın.
      
      Akış bulucu, tanımlanan **Içerik anahtarı ilkesiyle** de ilişkilendirilir.
    
   3. Bir test belirteci oluşturun.

      **GetTokenAsync** yöntemi bir test oluşturmak nasıl belirteci gösterir.
   4. Akış URL'sini oluşturun.

      **GetDASHStreamingUrlAsync** yöntemi akış URL'si oluşturmak nasıl gösterir. Bu durumda, URL akışları **DASH** içeriği.

2. AES veya DRM istemci oynatıcı. Bir oynatıcı SDK (yerel veya tarayıcı tabanlı) tabanlı bir video oynatıcı uygulaması aşağıdaki gereksinimleri karşılaması gerekir:
   * Oyuncu SDK gerekli DRM istemcileri destekler
   * Player SDK 'Sı gerekli akış protokollerini destekler: Düzgünleştir, ÇIZGI ve/veya HLS
   * Oyuncu SDK lisansı edinme istekte bir JWT belirteci geçirme işleyebilir olması gerekir
  
     Bir oynatıcı kullanarak oluşturabileceğiniz [Azure Media Player API'sine](https://amp.azure.net/libs/amp/latest/docs/). Kullanma [Azure Media Player ProtectionInfo API'sine](https://amp.azure.net/libs/amp/latest/docs/) farklı DRM platformlarda kullanılacak DRM teknolojileri belirtmek için.

     Test AES veya şifrelenmiş CENC (Widevine ve/veya PlayReady) için içerik, kullanabileceğiniz [Azure Media Player](https://aka.ms/azuremediaplayer). "Gelişmiş Seçenekler" seçeneğini tıklatın ve denetimi, şifreleme seçenekleri emin olun.

     FairPlay şifreli içeriği test etmek istediğiniz kullanırsanız [bu test yürütücünün](https://aka.ms/amtest). Player, Widevine, PlayReady, destekler ve benzeri FairPlay DRM ve bunun yanı sıra AES-128 şifresiz anahtar şifrelemesiyle koruyun. 
    
     Farklı DRMs 'Leri test etmek için doğru tarayıcıyı seçmeniz gerekir: Widevine için Chrome/opera/Firefox, PlayReady için Microsoft Edge/ıE11, FairPlay için macOS.

3. Belirteç Hizmeti (JSON Web Token (JWT) olarak arka uç kaynağına erişim için erişim belirteci verir STS), güvenli hale getirin. AMS lisans teslim hizmetleri arka uç kaynağı olarak kullanabilirsiniz. STS sahip aşağıdaki tanımlamak:

   * Veren ve İzleyici (veya kapsam)
   * İçerik koruma, iş gereksinimleri bağımlı olan talepleri
   * İmza doğrulaması için simetrik ya da asimetrik doğrulama
   * Anahtar geçişi desteği (gerekirse)

     [Bu STS aracını](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) , tüm 3 tür doğrulama anahtarını destekleyen STS 'yi test etmek için kullanabilirsiniz: simetrik, asimetrik veya anahtar geçişi Ile Azure AD. 

> [!NOTE]
> Odaklanmanıza ve her parça (yukarıda açıklanmıştır) tam olarak test sonraki adımına geçmeden önce önerilir. "Content protection" sisteminizi test etmek için yukarıdaki listede belirtilen araçları kullanın.  

## <a name="streaming-protocols-and-encryption-types"></a>Akış protokolleri ve şifreleme türleri

Media Services PlayReady, Widevine ve FairPlay kullanarak AES şifresiz anahtarını veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmek için kullanabilirsiniz. Şu anda, HTTP canlı akışı (HLS), MPEG DASH ve kesintisiz akış biçimlerinde şifreleyebilirsiniz. Her protokolü, aşağıdaki şifreleme yöntemlerini destekler:

### <a name="hls"></a>HLS

HLS protokolü aşağıdaki kapsayıcı biçimlerini ve şifreleme düzenlerini destekler.

|Kapsayıcı biçimi|Şifreleme şeması|URL örneği|
|---|---|---|
|Tümü|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbc)`|
|TS MPG2 |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbcs-aapl)`|
|CMAF(fmp4) |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`|
|TS MPG2 |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cenc)`|
|CMAF(fmp4) |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cenc)`|

HLS/CMAF + FairPlay (HEVC/H. 265 dahil) aşağıdaki cihazlarda desteklenir:

* iOS v11 veya üzeri 
* iPhone 8 veya üzeri
* MacOS High Sierra Intel 7 gen CPU

### <a name="mpeg-dash"></a>MPEG-DASH

MPEG-DASH protokolü aşağıdaki kapsayıcı biçimlerini ve şifreleme düzenlerini destekler.

|Kapsayıcı biçimi|Şifreleme şeması|URL örnekleri
|---|---|---|
|Tümü|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cbc)`|
|CSF(fmp4) |CENC (Widevine + PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cenc)`|
|CMAF(fmp4)|CENC (Widevine + PlayReady)|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-cmaf,encryption=cenc)`|

### <a name="smooth-streaming"></a>Kesintisiz Akış

Kesintisiz Akış Protokolü aşağıdaki kapsayıcı biçimlerini ve şifreleme düzenlerini destekler.

|Protocol|Kapsayıcı biçimi|Şifreleme şeması|
|---|---|---|
|fMP4|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cbc)`|
|fMP4 | CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cenc)`|

### <a name="browsers"></a>Tarayıcılar

Ortak tarayıcılar aşağıdaki DRM istemcilerini destekler:

|Browser|Şifreleme|
|---|---|
|Chrome|Widevine|
|Microsoft Edge, IE 11|PlayReady|
|'U|Widevine|
|Opera|Widevine|
|Safari|FairPlay|

## <a name="control-content-access"></a>İçerik erişimi denetleme

İçeriğinizi içerik anahtarı ilkesi yapılandırarak kimlerin erişebileceğini kontrol edebilirsiniz. Media Services, anahtar isteğinde bulunan kullanıcıları yetkilendirmenin birden çok yöntemini destekler. İçerik anahtarı İlkesi yapılandırmanız gerekir. Anahtarın istemciye teslim edilebilmesi için istemci (oynatıcı) ilkeyi karşılaması gerekir. İçerik anahtarı ilkeniz olabilir **açın** veya **belirteci** kısıtlama. 

Bir belirteç kısıtlamalı içerik anahtar ilkesiyle, içerik anahtarı, anahtar/lisans istekte geçerli JSON Web Token (JWT) veya basit web belirteci (SWT) sunan bir istemciye gönderilir. Bu belirteci bir güvenlik belirteci hizmeti (STS) tarafından verilmiş olması gerekir. Azure Active Directory STS kullanın veya özel STS dağıtın. STS belirteci kısıtlama yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar dağıtımı hizmetiyle belirteç geçerliyse ve belirteçteki talepler için bir anahtar/lisans yapılandırılanlar eşleşen istemci için istenen anahtar/lisans döndürür.

Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç birlikte imzalandığı anahtarını içerir. Verici belirteci veren güvenli belirteç hizmetidir. Belirtecin amacı kapsam olarak da adlandırılan, hedef kitle açıklayan veya kaynak belirteci erişimini yetkilendirir. Media Services anahtar dağıtımı hizmetiyle belirtecindeki bu değerleri şablon değerleri eşleştiğini doğrular.

Müşteriler, farklı DRM lisans parametrelerine sahip farklı ContentKeyPolicyOptions arasında seçim yapmak için özel bir STS kullanır (bir abonelik lisansı, bir kiralama lisansına karşı) veya içerik anahtarını temsil eden bir talep dahil eder belirtecin erişim izni verdiği anahtarın tanımlayıcısı.

### <a name="token-replay-prevention"></a>Belirteç yeniden yürütme engellemesi

*Belirteç yeniden yürütme engellemesi* özelliği, Media Services müşterilerin aynı belirtecin bir anahtar veya lisans istemek için kaç kez kullanılabileceği konusunda bir sınır ayarlamasına olanak tanır. Müşteri, belirtecin bir lisans ya da anahtar `urn:microsoft:azure:mediaservices:maxuses` almak için kaç kez kullanılabileceğini, belirteçte bir tür talep ekleyebilir. Anahtar teslimine aynı belirtece sahip sonraki tüm istekler, yetkisiz bir yanıt verecektir. Bkz. isteği [DRM örneğine](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L601)ekleme.
 
#### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Müşterilerin belirteç oluşturma üzerinde denetimi olmalıdır. Talebin kendisini belirtece yerleştirilmesi gerekir.
* Bu özellik kullanılırken, zaman aşımı süresi, isteğin alındığı zamandan bir saatten fazla olan istekleri yetkisiz bir Yanıtla reddedilir.
* Belirteçler, imzaları tarafından benzersiz şekilde tanımlanır. Yükte yapılan herhangi bir değişiklik (örneğin, sona erme saati veya talebin güncelleştirilmesi) belirtecin imzasını değiştirir ve anahtar tesliminin daha önce karşılaşmadığından yeni bir belirteç olarak sayılır.
* Belirteç müşteri tarafından ayarlanan `maxuses` değeri aşarsa kayıttan yürütme başarısız olur.
* Bu özellik, var olan tüm korumalı içerik (yalnızca verilen belirtecin değiştirilmesi gerekir) için kullanılabilir.
* Bu özellik hem JWT hem de SWT ile birlikte kullanılabilir.

## <a name="custom-key-and-license-acquisition-url"></a>Özel anahtar ve lisans alımı URL 'SI

Farklı bir anahtar ve lisans teslim hizmeti (Media Services değil) belirtmek istiyorsanız aşağıdaki şablonları kullanın. Şablonlarda iki değiştirilebilir alan mevcuttur. böylece, her varlık için akış Ilkesi oluşturmak yerine akış Ilkenizi birçok varlık arasında paylaşabilirsiniz. 

* EnvelopeEncryption. CustomKeyAcquisitionUrlTemplate-Son Kullanıcı oynatıcılara anahtar teslim eden özel hizmetin URL 'SI için şablon. Anahtar vermek için Azure Media Services kullanılırken gerekli değildir. Şablon, hizmetin çalışma zamanında, isteğe özgü değerle güncelleşeceğini değiştirilebilen belirteçleri destekler.  Şu anda desteklenen belirteç değerleri, istenen anahtarın tanımlayıcısı değeriyle değiştirilen Streaminglocatorıd. Alternativemediaıd ve {Contentkeyıd} değeriyle değiştirilen {Alternativemediaıd}.
* StreamingPolicyPlayReadyConfiguration. Customlicensesunisitionurltemplate-Son Kullanıcı oynatıcılarına lisans teslim eden özel hizmetin URL 'SI için şablon. Lisans vermek için Azure Media Services kullanılırken gerekli değildir. Şablon, hizmetin çalışma zamanında, isteğe özgü değerle güncelleşeceğini değiştirilebilen belirteçleri destekler. Şu anda desteklenen belirteç değerleri, istenen anahtarın tanımlayıcısı değeriyle değiştirilen Streaminglocatorıd. Alternativemediaıd ve {Contentkeyıd} değeriyle değiştirilen {Alternativemediaıd}. 
* StreamingPolicyWidevineConfiguration. Customlicensetanışisitionurltemplate-yalnızca Widevine için yukarıdaki gibi. 
* StreamingPolicyFairPlayConfiguration. Customlicensetanışmationurltemplate-yalnızca FairPlay için yukarıdaki gibi.  

Örneğin:

```csharp
streamingPolicy.EnvelopEncryption.customKeyAcquisitionUrlTemplate = "https://mykeyserver.hostname.com/envelopekey/{AlternativeMediaId}/{ContentKeyId}";
```

, `ContentKeyId` İstenen anahtarın bir değerine sahiptir `AlternativeMediaId` ve isteği kendi tarafınızda bir varlıkla eşlemek istiyorsanız kullanılabilir. Örneğin `AlternativeMediaId` , izinleri bulmanıza yardımcı olması için kullanılabilir.

Özel anahtar ve lisans alımı URL 'Leri kullanan REST örnekleri için bkz. [akış ilkeleri-oluşturma](https://docs.microsoft.com/rest/api/media/streamingpolicies/create)

## <a name="troubleshoot"></a>Sorun giderme

`MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY` Hatayı alırsanız uygun akış ilkesini belirttiğinizden emin olun.

İle `_NOT_SPECIFIED_IN_URL`biten hatalar alırsanız, URL 'de şifreleme biçimini belirttiğinizden emin olun. Örneğin: `…/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`. Bkz. [akış protokolleri ve şifreleme türleri](#streaming-protocols-and-encryption-types).

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* [AES şifrelemesi ile koruma](protect-with-aes128.md)
* [DRM ile koruma](protect-with-drm.md)
* [Access Control ile çoklu DRM içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)
* [Depolama tarafı şifrelemesi](storage-account-concept.md#storage-side-encryption)
* [Sık sorulan sorular](frequently-asked-questions.md)

