---
title: Media Services dinamik şifreleme ile içeriğinizi koruyun
titleSuffix: Azure Media Services
description: Azure Media Services içindeki dinamik şifreleme, akış protokolleri ve şifreleme türleriyle içerik koruması hakkında bilgi edinin.
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
ms.date: 10/29/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 1ae8645f3e782305ed5e1b7847b019a978e48dde
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74977786"
---
# <a name="protect-your-content-with-media-services-dynamic-encryption"></a>Media Services dinamik şifreleme ile içeriğinizi koruyun

Ortamınızı, depolama, işleme ve teslim aracılığıyla bilgisayarınızdan tamamen ayrılmaları için güvenli hale getirmek üzere Azure Media Services kullanın. Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady ve Google Widevine Apple FairPlay. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere.  

Media Services v3 'de, içerik anahtarı akış Konumlandırıcı ile ilişkilendirilir ( [Bu örneğe](protect-with-aes128.md)bakın). Media Services anahtar teslim hizmetini kullanıyorsanız, sizin için içerik anahtarını Azure Media Services oluşturabilirsiniz. Size ait anahtar teslim hizmetini kullanıyorsanız veya iki veri merkezinde aynı içerik anahtarına sahip olmanız gereken yüksek bir kullanılabilirlik senaryosunu işlemeniz gerekiyorsa, içerik anahtarı kendiniz oluşturulmalıdır.

Bir akışa bir oynatıcı tarafından istendiğinde Media Services dinamik olarak içeriğinizi AES şifresiz anahtar veya DRM şifreleme kullanarak şifrelemek için belirtilen anahtar kullanır. Akış şifresini çözmek için Media Services anahtar dağıtımı hizmetiyle veya belirtilen anahtar dağıtımı hizmetiyle anahtar player ister. Kullanıcının anahtarı almak için yetkilendirilip yetkilendirilmeyeceğine karar vermek için, hizmet, anahtar için belirttiğiniz içerik anahtarı ilkesini değerlendirir.

Lisanslar ve anahtarları için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için REST API veya bir Media Services istemci Kitaplığı'nı kullanabilirsiniz.

Aşağıdaki görüntüde Media Services içerik koruması için iş akışı gösterilmektedir:

![Media Services içerik koruması için iş akışı](./media/content-protection/content-protection.svg)
  
&#42;*Dinamik şifreleme, AES-128 Clear Key, CBCS ve CENC 'yi destekler. Ayrıntılar için bkz. [destek matrisi](#streaming-protocols-and-encryption-types).*

Bu makalede, Media Services ile içerik korumayı anlamanıza yardımcı olacak kavramlar ve terminoloji açıklanmaktadır.

## <a name="main-components-of-a-content-protection-system"></a>Bir içerik koruma sisteminin ana bileşenleri

İçerik koruma sisteminizi başarıyla tamamlayabilmeniz için efor kapsamını tam olarak anlamanız gerekir. Aşağıdaki bölümler, uygulamanız gereken üç bölümden oluşan bir genel bakış sunar.

> [!NOTE]
> Sonraki bölüme geçmeden önce aşağıdaki bölümlerde yer alan her parçayı odaklamanız ve tam olarak sınamanız önerilir. İçerik koruma sisteminizi test etmek için bölümlerde belirtilen araçları kullanın.

### <a name="media-services-code"></a>Media Services kodu
  
[DRM örneği](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) , .net kullanarak Media Services v3 Ile çok DRM sisteminin nasıl uygulanacağını gösterir. Ayrıca, Media Services lisansı/anahtar teslim hizmetinin nasıl kullanılacağını gösterir.
  
Her bir varlığı birden fazla şifreleme türü (AES-128, PlayReady, Widevine, FairPlay) ile şifreleyebilirsiniz. Birleştirme açısından anlamlı olduğunu görmek için bkz. [akış protokolleri ve şifreleme türleri](#streaming-protocols-and-encryption-types).

Örnekte gösterildiği nasıl yapılır:

1. [İçerik anahtar ilkesi](content-key-policy-concept.md)oluşturun ve yapılandırın.

   İçerik anahtarının (varlıklarınıza güvenli erişim sağlayan) son istemcilere teslim edildiğini yapılandırmak için bir içerik anahtarı ilkesi oluşturursunuz:  

   * Lisans teslimi yetkilendirmesi tanımlayın. JSON Web Token (JWT) içindeki talepler temelinde yetkilendirme denetiminin mantığını belirtin.
   * [PlayReady](playready-license-template-overview.md), [Widevine](widevine-license-template-overview.md)ve/veya [Fairplay](fairplay-license-overview.md) lisanslarını yapılandırın. Şablonlar, her bir DRMs için hakları ve izinleri yapılandırmanızı sağlar.

     ```
     ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
     ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
     ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
     ```

2. Şifrelenmiş varlığı akışa almak için yapılandırılmış bir [akış Bulucu](streaming-locators-concept.md) oluşturun.
  
   Akış bulucunun bir [akış ilkesiyle](streaming-policy-concept.md)ilişkilendirilmesi vardır. Örnekte, `StreamingLocator.StreamingPolicyName` "Predefined_MultiDrmCencStreaming" ilkesine ayarlayacağız.

   PlayReady ve Widevine şifrelemeleri uygulanır ve anahtar, yapılandırılmış DRM lisanslarına göre kayıttan yürütme istemcisine gönderilir. Akışınızı CIBH (FairPlay) ile şifrelemek istiyorsanız "Predefined_MultiDrmStreaming" ilkesini kullanın.

   Akış bulucu, tanımladığınız içerik anahtarı ilkesiyle de ilişkilendirilir.

3. Bir test belirteci oluşturun.

   `GetTokenAsync` yöntemi bir test belirtecinin nasıl oluşturulacağını gösterir.
4. Akış URL'sini oluşturun.

   `GetDASHStreamingUrlAsync` yöntemi, akış URL 'sinin nasıl oluşturulacağını gösterir. Bu durumda, URL DASH içeriğini akışlar.

### <a name="player-with-an-aes-or-drm-client"></a>AES veya DRM istemcisiyle oynatıcı

Bir oynatıcı SDK (yerel veya tarayıcı tabanlı) tabanlı bir video oynatıcı uygulaması aşağıdaki gereksinimleri karşılaması gerekir:

* Player SDK 'Sı gereken DRM istemcilerini destekler.
* Player SDK 'Sı gereken akış protokollerini destekler: Düzgünleştir, TIRE ve/veya HTTP Canlı Akışı (HLS).
* Oynatıcı SDK, bir lisans alma isteğinde JWT belirteci geçirmeyi işleyebilir.

Bir oynatıcı kullanarak oluşturabileceğiniz [Azure Media Player API'sine](https://amp.azure.net/libs/amp/latest/docs/). Kullanma [Azure Media Player ProtectionInfo API'sine](https://amp.azure.net/libs/amp/latest/docs/) farklı DRM platformlarda kullanılacak DRM teknolojileri belirtmek için.

Test AES veya şifrelenmiş CENC (Widevine ve/veya PlayReady) için içerik, kullanabileceğiniz [Azure Media Player](https://aka.ms/azuremediaplayer). **Gelişmiş seçenekleri** seçtiğinizden ve şifreleme seçeneklerinizi kontrol ettiğinizden emin olun.

FairPlay şifreli içeriği test etmek istediğiniz kullanırsanız [bu test yürütücünün](https://aka.ms/amtest). Oynatıcı Widevine, PlayReady ve FairPlay DRMs 'yi, AES-128 şifresiz anahtar şifrelemesi ile birlikte destekler.

Farklı DRMs 'Leri test etmek için doğru tarayıcıyı seçin:

* Widevine için Chrome, Opera veya Firefox.
* PlayReady için Microsoft Edge veya Internet Explorer 11.
* FairPlay için macOS üzerinde Safari.

### <a name="security-token-service"></a>Güvenlik belirteci hizmeti

Bir güvenlik belirteci hizmeti (STS), arka uç kaynak erişimi için erişim belirteci olarak JWT yayınlar. Azure Media Services lisans/anahtar teslim hizmetini arka uç kaynağı olarak kullanabilirsiniz. STS aşağıdaki şeyleri tanımlamalıdır:

* Veren ve seyirci (ya da kapsam).
* Talepler, içerik korumasında iş gereksinimlerine bağımlıdır.
* İmza doğrulaması için simetrik veya asimetrik doğrulama.
* Anahtar aktarma desteği (gerekirse).

[Bu STS aracını](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) kullanarak STS 'yi test edebilirsiniz. Bu üç tür doğrulama anahtarını destekler: simetrik, asimetrik veya Azure Active Directory (Azure AD) anahtar geçişi.

## <a name="streaming-protocols-and-encryption-types"></a>Akış protokolleri ve şifreleme türleri

Media Services PlayReady, Widevine ve FairPlay kullanarak AES şifresiz anahtarını veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmek için kullanabilirsiniz. Şu anda, HLS, MPEG DASH ve Kesintisiz Akış biçimlerini şifreleyebilirsiniz. Her protokol aşağıdaki şifreleme yöntemlerini destekler.

### <a name="hls"></a>HLS

HLS protokolü aşağıdaki kapsayıcı biçimlerini ve şifreleme düzenlerini destekler:

|Kapsayıcı biçimi|Şifreleme şeması|URL örneği|
|---|---|---|
|Tümü|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbc)`|
|TS MPG2 |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbcs-aapl)`|
|CMAF(fmp4) |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`|
|TS MPG2 |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cenc)`|
|CMAF(fmp4) |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cenc)`|

HLS/CMAF + FairPlay (HEVC/H. 265 dahil) aşağıdaki cihazlarda desteklenir:

* iOS 11 veya üzeri.
* iPhone 8 veya üzeri.
* MacOS High Sierra, Intel 7 nesil CPU.

### <a name="mpeg-dash"></a>MPEG-DASH

MPEG-DASH protokolü aşağıdaki kapsayıcı biçimlerini ve şifreleme düzenlerini destekler:

|Kapsayıcı biçimi|Şifreleme şeması|URL örnekleri
|---|---|---|
|Tümü|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cbc)`|
|CSF(fmp4) |CENC (Widevine + PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cenc)`|
|CMAF(fmp4)|CENC (Widevine + PlayReady)|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-cmaf,encryption=cenc)`|

### <a name="smooth-streaming"></a>Kesintisiz Akış

Kesintisiz Akış Protokolü aşağıdaki kapsayıcı biçimlerini ve şifreleme düzenlerini destekler.

|Protokol|Kapsayıcı biçimi|Şifreleme şeması|
|---|---|---|
|fMP4|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cbc)`|
|fMP4 | CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cenc)`|

### <a name="browsers"></a>Tarayıcılar

Ortak tarayıcılar aşağıdaki DRM istemcilerini destekler:

|Tarayıcı|Şifreleme|
|---|---|
|Chrome|Widevine|
|Microsoft Edge, Internet Explorer 11|PlayReady|
|Firefox|Widevine|
|Opera|Widevine|
|Safari|FairPlay|

## <a name="controlling-content-access"></a>İçerik erişimini denetleme

İçeriğinizi içerik anahtarı ilkesi yapılandırarak kimlerin erişebileceğini kontrol edebilirsiniz. Media Services, anahtar isteğinde bulunan kullanıcıları yetkilendirmenin birden çok yöntemini destekler. Anahtarın istemciye teslim edilebilmesi için istemci (oynatıcı) ilkeyi karşılaması gerekir. İçerik anahtarı ilkeniz olabilir *açın* veya *belirteci* kısıtlama.

Bir açık kısıtlanmış içerik anahtarı ilkesi, yetkilendirmesiz herkese lisans vermek istediğinizde kullanılabilir. Örneğin, gelirleriniz ad tabanlıdır ve abonelik temelli değildir.  

Belirteç kısıtlı içerik anahtar ilkesiyle, içerik anahtarı yalnızca lisans/anahtar isteğinde geçerli bir JWT belirteci veya basit bir Web belirteci (SWT) sunan bir istemciye gönderilir. Bu belirtecin bir STS tarafından verilmesi gerekir.

Azure AD 'yi STS olarak kullanabilir veya [Özel BIR STS](#using-a-custom-sts)dağıtımı yapabilirsiniz. STS belirteci kısıtlama yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Media Services lisansı/anahtar teslim hizmeti, bu koşulların her ikisi de varsa, istenen lisansı veya anahtarı istemciye döndürür:

* Belirteç geçerli.
* Belirteçteki talepler, lisans veya anahtar için yapılandırılananlarla eşleşiyor.

Belirteç kısıtlı ilkesini yapılandırırken, birincil doğrulama anahtarını, verenin ve hedef kitle parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç birlikte imzalandığı anahtarını içerir. Veren, belirteci veren STS 'dir. Bazen kapsam olarak adlandırılan hedef kitle, belirtecin veya belirtecin erişim yetkisi veren kaynağın amacını açıklar. Media Services lisansı/anahtar teslim hizmeti, belirteçteki bu değerlerin şablondaki değerlerle eşleştiğini doğrular.

### <a name="token-replay-prevention"></a>Belirteç yeniden yürütme engellemesi

*Belirteç yeniden yürütme engellemesi* özelliği, Media Services müşterilerin aynı belirtecin bir anahtar veya lisans istemek için kaç kez kullanılabileceği konusunda bir sınır ayarlamasına olanak tanır. Müşteri, belirteçte `urn:microsoft:azure:mediaservices:maxuses` bir talep ekleyebilir; burada değer, belirtecin bir lisans ya da anahtar almak için kullanılabileceği sayı olan sayıdır. Anahtar teslimine aynı belirtece sahip sonraki tüm istekler, yetkisiz bir yanıt verecektir. Bkz. isteği [DRM örneğine](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L601)ekleme.
 
#### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Müşterilerin belirteç oluşturma üzerinde denetimi olmalıdır. Talebin kendisini belirtece yerleştirilmesi gerekir.
* Bu özellik kullanılırken, zaman aşımı süresi, isteğin alındığı zamandan bir saatten fazla olan istekleri yetkisiz bir Yanıtla reddedilir.
* Belirteçler, imzaları tarafından benzersiz şekilde tanımlanır. Yükte yapılan herhangi bir değişiklik (örneğin, sona erme saati veya talebin güncelleştirilmesi) belirtecin imzasını değiştirir ve anahtar tesliminin daha önce geldiği yeni bir belirteç olarak sayılır.
* Belirteç, müşteri tarafından ayarlanan `maxuses` değeri aşarsa kayıttan yürütme başarısız olur.
* Bu özellik, var olan tüm korumalı içerik (yalnızca verilen belirtecin değiştirilmesi gerekir) için kullanılabilir.
* Bu özellik hem JWT hem de SWT ile birlikte kullanılabilir.

## <a name="using-a-custom-sts"></a>Özel STS kullanma

Bir müşteri, belirteç sağlamak için özel bir STS kullanmayı tercih edebilir. Nedenler şunlardır:

* Müşteri tarafından kullanılan kimlik sağlayıcısı (ıDP) STS 'yi desteklemez. Bu durumda, özel STS bir seçenek olabilir.
* Müşteri faturalandırma sistemine müşterinin aboneyle STS tümleştirmek için daha esnek veya sıkı denetim gerekebilir.

   Örneğin, bir [Ott](https://en.wikipedia.org/wiki/Over-the-top_media_services) hizmeti operatörü Premium, temel ve spor gibi birden çok abone paketi sunabilir. İşleci, yalnızca belirli bir paket içeriğini kullanılabilir hale getirilir, böylece bir abonenin paket belirteciyle Taleplerde eşleştirilecek isteyebilirsiniz. Bu durumda, özel STS gereken esneklik ve denetim sağlar.

* Farklı bir DRM lisans parametreleriyle farklı ContentKeyPolicyOptions arasında seçim yapmak üzere belirtece özel talepler eklemek için (bir abonelik lisansı, bir kiralama lisansına karşı).
* Belirtecin erişim izni verdiği anahtarın içerik anahtar tanımlayıcısını temsil eden bir talep eklemek için.

Özel STS kullandığınızda, iki değişiklik yapılması gerekir:

* Bir varlık için lisans teslimat hizmeti yapılandırırken, Azure AD'den geçerli anahtar yerine özel STS tarafından doğrulama için kullanılan güvenlik anahtarı belirtmeniz gerekir.
* Güvenlik anahtarı geçerli X509 özel anahtarı yerine JTW belirteç oluşturulduğunda, belirtilen Azure ad'deki sertifika.

Güvenlik anahtarları iki tür vardır:

* Simetrik anahtar: oluşturulacak ve JWT'nin doğrulamak için aynı anahtar kullanılır.
* Asimetrik anahtar: genel-özel anahtar çifti x X509 sertifika JWT'nin şifrelemek/oluşturmak için bir özel anahtarla ve ortak anahtar belirteci doğrulamak için kullanılır.

.NET Framework kullanırsanız / C# geliştirme platformu olarak X509 bir asimetrik güvenlik anahtarı için kullanılan sertifika bir anahtar uzunluğu en az 2048 olması gerekir. Bu anahtar uzunluğu .NET Framework içindeki System. IdentityModel. Tokens. X509AsymmetricSecurityKey sınıfının bir gereksinimidir. Aksi takdirde, şu özel durum atılır: IDX10630: imzalama için ' System. IdentityModel. Tokens. X509AsymmetricSecurityKey ', ' 2048 ' bitten küçük olamaz.

## <a name="custom-key-and-license-acquisition-url"></a>Özel anahtar ve lisans alımı URL 'SI

Farklı bir lisans/anahtar teslim hizmeti (Media Services değil) belirtmek istiyorsanız aşağıdaki şablonları kullanın. Şablonlarda iki değiştirilebilir alan mevcuttur. böylece, her varlık için akış ilkesi oluşturmak yerine akış ilkenizi birçok varlık arasında paylaşabilirsiniz. 

* `EnvelopeEncryption.CustomKeyAcquisitionUrlTemplate`: Son Kullanıcı oynatıcılara anahtar sunan özel hizmetin URL 'SI. Anahtar vermek için Azure Media Services kullandığınızda bu gerekli değildir. 

   Şablon, hizmetin çalışma zamanında, isteğe özgü değerle güncelleşeceğini değiştirilebilen belirteçleri destekler.  Şu anda desteklenen belirteç değerleri şunlardır:
   * `{AlternativeMediaId}`, Streaminglocatorıd. Alternativemediaıd değeriyle değiştirilmiştir.
   * `{ContentKeyId}`, istenen anahtarın tanımlayıcısının değeriyle değiştirilmiştir.
* `StreamingPolicyPlayReadyConfiguration.CustomLicenseAcquisitionUrlTemplate`: Son Kullanıcı oynatıcılarına lisans sunan özel hizmetin URL 'SI için şablon. Lisans vermek için Azure Media Services kullandığınızda bu gerekli değildir.

   Şablon, hizmetin çalışma zamanında, isteğe özgü değerle güncelleşeceğini değiştirilebilen belirteçleri destekler. Şu anda desteklenen belirteç değerleri şunlardır:  
   * `{AlternativeMediaId}`, Streaminglocatorıd. Alternativemediaıd değeriyle değiştirilmiştir.
   * `{ContentKeyId}`, istenen anahtarın tanımlayıcısının değeriyle değiştirilmiştir. 
* `StreamingPolicyWidevineConfiguration.CustomLicenseAcquisitionUrlTemplate`: yalnızca Widevine için olan önceki şablonla aynı. 
* `StreamingPolicyFairPlayConfiguration.CustomLicenseAcquisitionUrlTemplate`: önceki şablonla aynı ancak yalnızca FairPlay için.  

Örnek:

```csharp
streamingPolicy.EnvelopEncryption.customKeyAcquisitionUrlTemplate = "https://mykeyserver.hostname.com/envelopekey/{AlternativeMediaId}/{ContentKeyId}";
```

`ContentKeyId` istenen anahtarın bir değeri vardır. İsteği kendi tarafınızda bir varlıkla eşlemek istiyorsanız `AlternativeMediaId` kullanabilirsiniz. Örneğin, `AlternativeMediaId` izinleri bulmanıza yardımcı olması için kullanılabilir.

Özel lisans/anahtar alma URL 'Leri kullanan REST örnekleri için bkz. [akış ilkeleri-oluştur](https://docs.microsoft.com/rest/api/media/streamingpolicies/create).

> [!NOTE]
> Widevine, Google Inc. tarafından sunulan bir hizmettir ve Google, Inc 'nin hizmet koşullarına ve gizlilik Ilkesine tabidir.

## <a name="troubleshoot"></a>Sorun giderme

`MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY` hata alırsanız, uygun akış ilkesini belirttiğinizden emin olun.

`_NOT_SPECIFIED_IN_URL`ile biten hatalar alırsanız, URL 'de şifreleme biçimini belirttiğinizden emin olun. `…/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)` bunun bir örneğidir. Bkz. [akış protokolleri ve şifreleme türleri](#streaming-protocols-and-encryption-types).

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde bulunun, güncelleştirmeleri al

Soru sormak, geri bildirimde bulunmak ve Media Services hakkında güncelleştirmeler almak için [Azure Media Services Community](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* [AES şifrelemesi ile koruma](protect-with-aes128.md)
* [DRM ile koruma](protect-with-drm.md)
* [Access Control ile çoklu DRM içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)
* [Depolama tarafı şifrelemesi](storage-account-concept.md#storage-side-encryption)
* [Sık sorulan sorular](frequently-asked-questions.md)
* [JSON Web Belirteci İşleyicisi](https://docs.microsoft.com/dotnet/framework/security/json-web-token-handler)
