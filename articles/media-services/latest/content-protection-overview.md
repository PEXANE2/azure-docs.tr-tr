---
title: Medya Hizmetleri v3 dinamik şifreleme ile içeriğinizi koruyun
titleSuffix: Azure Media Services
description: Azure Medya Hizmetleri'nde dinamik şifreleme, akış protokolleri ve şifreleme türleri ile içerik koruması hakkında bilgi edinin.
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
ms.date: 03/17/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: c1c9440f7ec70cea98f270f04c3030c800dd0fde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79461121"
---
# <a name="protect-your-content-with-media-services-dynamic-encryption"></a>Medya Hizmetleri dinamik şifreleme ile içeriğinizi koruyun

Bilgisayarınızı niçin depolama, işleme ve teslim yoluyla tüm yol boyunca terk eden andan itibaren medyagüvenliğini güvence altına almak için Azure Medya Hizmetleri'ni kullanın. Media Services ile canlı ve isteğe bağlı içeriğinizi Gelişmiş Şifreleme Standardı (AES-128) veya üç büyük dijital haklar yönetimi (DRM) sisteminden herhangi biriyle dinamik olarak şifrelenmiş olarak teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Medya Hizmetleri ayrıca yetkili müşterilere AES anahtarları ve DRM (PlayReady, Widevine ve FairPlay) lisansları sunmak için bir hizmet sağlar. İçerik AES açık anahtarıyla şifrelenir ve HTTPS üzerinden gönderilirse, istemciye ulaşana kadar net değildir. 

Medya Hizmetleri v3'te, bir içerik anahtarı Akış lı Konumbelirleyici ile ilişkilidir [(bu örneğe](protect-with-aes128.md)bakın). Medya Hizmetleri anahtar teslim hizmetini kullanıyorsanız, Azure Medya Hizmetleri'nin sizin için içerik anahtarını oluşturmasına izin verebilirsiniz. Kendi anahtar teslim hizmetinizi kullanıyorsanız veya iki veri merkezinde aynı içerik anahtarına sahip olmanız gereken yüksek kullanılabilirlik senaryosunu işlemeniz gerekiyorsa, içerik anahtarı kendiniz oluşturulmalıdır.

Bir akış bir oynatıcı tarafından istendiğinde, Medya Hizmetleri AES clear tuşu veya DRM şifrelemesi kullanarak içeriğinizi dinamik olarak şifrelemek için belirtilen anahtarı kullanır. Akışın şifresini çözmek için oynatıcı anahtarı Media Services anahtar teslim hizmetinden veya belirttiğiniz anahtar teslim hizmetinden ister. Kullanıcının anahtarı almaya yetkili olup olmadığına karar vermek için, hizmet anahtar için belirlediğiniz içerik anahtarı ilkesini değerlendirir.

Lisanslarınız ve anahtarlarınız için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için REST API'sını veya Medya Hizmetleri istemci kitaplığını kullanabilirsiniz.

Aşağıdaki resim, Medya Hizmetleri içerik koruması için iş akışını göstermektedir:

![Medya Hizmetleri içerik koruması için iş akışı](./media/content-protection/content-protection.svg)
  
&#42; *Dinamik şifreleme, AES-128 net tuşu, CBCS ve CENC'i destekler. Ayrıntılar için [destek matrisine](#streaming-protocols-and-encryption-types)bakın.*

Bu makalede, Medya Hizmetleri ile içerik koruma anlamanıza yardımcı olan kavramlar ve terminoloji açıklanmaktadır.

## <a name="main-components-of-a-content-protection-system"></a>İçerik koruma sisteminin ana bileşenleri

İçerik koruma sisteminizi başarıyla tamamlamak için çabanın kapsamını tam olarak anlamanız gerekir. Aşağıdaki bölümler, uygulamanız gereken üç bölüme genel bir bakış sağlar.

> [!NOTE]
> Bir sonraki bölüme geçmeden önce aşağıdaki bölümlerdeki her parçaya odaklanmanızı ve tam olarak test etmenizi şiddetle tavsiye ediyoruz. İçerik koruma sisteminizi test etmek için bölümlerde belirtilen araçları kullanın.

### <a name="media-services-code"></a>Medya Hizmetleri kodu
  
[DRM örneği,](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) .NET kullanarak Media Services v3 ile çoklu DRM sistemini nasıl uygulayacağınızı gösterir. Ayrıca, Medya Hizmetleri lisans/anahtar teslim hizmetinin nasıl kullanılacağını da gösterir.
  
Her bir varlığı birden fazla şifreleme türü (AES-128, PlayReady, Widevine, FairPlay) ile şifreleyebilirsiniz. Birleştirmenin ne anlama olduğunu görmek için [Akış protokolleri ve şifreleme türlerine](#streaming-protocols-and-encryption-types)bakın.

Örnek, nasıl yapılacağını gösterir:

1. [İçerik anahtarı ilkesi](content-key-policy-concept.md)oluşturun ve yapılandırır.

   Son istemcilere içerik anahtarının (varlıklarınıza güvenli erişim sağlayan) nasıl teslim edildiğine yapılandırmak için bir içerik anahtarı ilkesi oluşturursunuz:  

   * Lisans teslim yetkisini tanımlayın. JSON Web Belirteci'ndeki (JWT) talepleri temel alan yetkilendirme denetiminin mantığını belirtin.
   * [PlayReady,](playready-license-template-overview.md) [Widevine](widevine-license-template-overview.md)ve/veya [FairPlay](fairplay-license-overview.md) lisanslarını yapılandırın. Şablonlar, DRM'lerin her biri için hakları ve izinleri yapılandırmanızı sağlar.

     ```
     ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
     ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
     ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
     ```

2. Şifrelenmiş varlığı akışa aktarmak için yapılandırılan bir [akış bulucu](streaming-locators-concept.md) oluşturun.
  
   Akış bulucu bir [akış ilkesi](streaming-policy-concept.md)ile ilişkili olmalıdır. Örnekte, "Predefined_MultiDrmCencStreaming" ilkesini belirledik. `StreamingLocator.StreamingPolicyName`

   PlayReady ve Widevine şifrelemeleri uygulanır ve anahtar yapılandırılmış DRM lisanslarına göre oynatma istemcisine teslim edilir. Akışınızı CBCS (FairPlay) ile de şifrelemek istiyorsanız, "Predefined_MultiDrmStreaming" ilkesini kullanın.

   Akış bulucu, tanımladığınız içerik anahtarı ilkesiyle de ilişkilidir.

3. Bir test belirteci oluşturun.

   Yöntem, `GetTokenAsync` test jetonu oluşturmanın nasıl yapılacağını gösterir.
4. Akış URL'sini oluşturun.

   Yöntem, `GetDASHStreamingUrlAsync` akış URL'sinin nasıl oluşturulabildiğini gösterir. Bu durumda, URL DASH içeriğini akışı.

### <a name="player-with-an-aes-or-drm-client"></a>AES veya DRM istemcisi olan oyuncu

Bir oyuncu SDK dayalı bir video oynatıcı uygulaması (yerli veya tarayıcı tabanlı) aşağıdaki gereksinimleri karşılamak gerekir:

* Oyuncu SDK gerekli DRM istemcilerini destekler.
* Oyuncu SDK gerekli akış protokollerini destekler: Smooth, DASH ve/veya HTTP Live Streaming (HLS).
* Oyuncu SDK, lisans edinme isteğinde jwt belirteci ile başa çıkabilir.

Azure Media Player API'sını kullanarak bir [oynatıcı](https://amp.azure.net/libs/amp/latest/docs/)oluşturabilirsiniz. Farklı DRM platformlarında hangi DRM teknolojisini kullanacağımı belirtmek için [Azure Media Player ProtectionInfo API'yi](https://amp.azure.net/libs/amp/latest/docs/) kullanın.

AES veya CENC (Widevine ve/veya PlayReady) şifreli içeriği test etmek için [Azure Media Player'ı](https://aka.ms/azuremediaplayer)kullanabilirsiniz. **Gelişmiş seçenekleri** seçtiğinizden ve şifreleme seçeneklerinizi kontrol ettiğinizden emin olun.

FairPlay şifreli içeriği test etmek istiyorsanız, [bu test oynatıcısını](https://aka.ms/amtest)kullanın. Oyuncu Widevine, PlayReady ve FairPlay DRM'lerini ve AES-128 net anahtar şifrelemesini destekler.

Farklı DRM'leri test etmek için doğru tarayıcıyı seçin:

* Widevine için Chrome, Opera veya Firefox.
* PlayReady için Microsoft Edge veya Internet Explorer 11.
* FairPlay için macOS'ta Safari.

### <a name="security-token-service"></a>Güvenlik belirteç hizmeti

Bir güvenlik belirteci hizmeti (STS), JWT'yi arka uç kaynak erişimi için erişim belirteci olarak verir. Azure Medya Hizmetleri lisans/anahtar teslim hizmetini arka uç kaynağı olarak kullanabilirsiniz. Bir STS aşağıdaki şeyleri tanımlamak zorundadır:

* İhraççı ve hedef kitle (veya kapsam).
* İçerik korumadaki iş gereksinimlerine bağlı olan talepler.
* İmza doğrulaması için simetrik veya asimetrik doğrulama.
* Anahtar rollover desteği (gerekirse).

[Bu STS aracını](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) STS'yi test etmek için kullanabilirsiniz. Her üç doğrulama türünü de destekler: anahtar devrine sahip simetrik, asimetrik veya Azure Active Directory (Azure AD).

## <a name="streaming-protocols-and-encryption-types"></a>Akış protokolleri ve şifreleme türleri

PlayReady, Widevine veya FairPlay kullanarak AES net tuşu veya DRM şifrelemesi ile dinamik olarak şifrelenmiş içeriğinizi sunmak için Medya Hizmetlerini kullanabilirsiniz. Şu anda HLS, MPEG DASH ve Düzgün Akış biçimlerini şifreleyebilirsiniz. Her protokol aşağıdaki şifreleme yöntemlerini destekler.

### <a name="hls"></a>HLS

HLS protokolü aşağıdaki kapsayıcı biçimlerini ve şifreleme düzenlerini destekler:

|Kapsayıcı biçimi|Şifreleme düzeni|URL örneği|
|---|---|---|
|Tümü|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbc)`|
|MPG2-TS |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbcs-aapl)`|
|CMAF(fmp4) |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`|
|MPG2-TS |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cenc)`|
|CMAF(fmp4) |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cenc)`|

HLS/CMAF + FairPlay (HEVC/H.265 dahil) aşağıdaki cihazlarda desteklenir:

* iOS 11 veya sonrası.
* iPhone 8 veya sonrası.
* Intel 7.Nesil Işlemci ile MacOS High Sierra.

### <a name="mpeg-dash"></a>MPEG-DASH

MPEG-DASH protokolü aşağıdaki kapsayıcı biçimlerini ve şifreleme düzenlerini destekler:

|Kapsayıcı biçimi|Şifreleme düzeni|URL Örnekleri
|---|---|---|
|Tümü|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cbc)`|
|BOS(fmp4) |CENC (Widevine + PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cenc)`|
|CMAF(fmp4)|CENC (Widevine + PlayReady)|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-cmaf,encryption=cenc)`|

### <a name="smooth-streaming"></a>Kesintisiz Akış

Düzgün Akış protokolü aşağıdaki kapsayıcı biçimlerini ve şifreleme düzenlerini destekler.

|Protokol|Kapsayıcı biçimi|Şifreleme düzeni|
|---|---|---|
|fMP4|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cbc)`|
|fMP4 | CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cenc)`|

### <a name="browsers"></a>Browsers (Tarayıcılar)

Ortak tarayıcılar aşağıdaki DRM istemcilerini destekler:

|Tarayıcı|Şifreleme|
|---|---|
|Chrome|Widevine|
|Microsoft Edge, Internet Explorer 11|PlayReady|
|Firefox|Widevine|
|Opera|Widevine|
|Safari|FairPlay|

## <a name="controlling-content-access"></a>İçerik erişimini denetleme

İçerik anahtarı ilkesini yapılandırarak içeriğinize kimlerin erişebileceğini kontrol edebilirsiniz. Media Services, anahtar isteğinde bulunan kullanıcıları yetkilendirmenin birden çok yöntemini destekler. Anahtar istemciye teslim edilmeden önce istemcinin (oyuncu) ilkeyi karşılaması gerekir. İçerik anahtarı ilkesi *açık* veya *belirteç* kısıtlaması olabilir.

İzinsiz herkese lisans vermek istediğinizde açık sınırlı içerik anahtarı ilkesi kullanılabilir. Örneğin, geliriniz reklam tabanlıysa ve abonelik tabanlı değilse.  

Belirteç le sınırlı içerik anahtarı ilkesiyle, içerik anahtarı yalnızca lisans/anahtar isteğinde geçerli bir JWT belirteci veya basit bir web belirteci (SWT) sunan bir istemciye gönderilir. Bu belirteç bir STS tarafından verilmelidir.

Azure AD'yi STS olarak kullanabilir veya özel bir [STS](#using-a-custom-sts)dağıtabilirsiniz. STS belirtilmiş anahtar ve belirteç kısıtlama yapılandırmasında belirttiğiniz sorun talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Medya Hizmetleri lisansı/anahtar teslim hizmeti, bu koşullardan her ikisi de varsa, istenen lisansı veya anahtarı istemciye döndürür:

* Belirteç geçerlidir.
* Belirteçteki talepler, lisans veya anahtar için yapılandırılanlarla eşleşir.

Belirteç kısıtlamalı ilkeyi yapılandırdığınızda, birincil doğrulama anahtarını, vereni ve hedef kitle parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı, belirteçle imzalandığı anahtarı içerir. İhraççı, belirteci veren STS'dir. Bazen kapsam olarak adlandırılan hedef kitle, belirtecinin amacını veya belirteci tarafından erişime izin verdiği kaynağı açıklar. Medya Hizmetleri lisans/anahtar teslim hizmeti, belirteçteki bu değerlerin şablondaki değerlerle eşleştirdiğini doğrular.

### <a name="token-replay-prevention"></a>Belirteç tekrar önleme

*Token Yeniden Oynatma Önleme* özelliği, Medya Hizmetleri müşterilerinin bir anahtar veya lisans istemek için aynı belirteçlerin kaç kez kullanılabileceğini belirlemesine olanak tanır. Müşteri, belirteç tespinin lisans veya anahtar almak için kaç kez kullanılabileceğinibelirtebilen bir tür `urn:microsoft:azure:mediaservices:maxuses` talebi ekleyebilir. Anahtar Teslimi'ne aynı belirteç içeren sonraki tüm istekler yetkisiz bir yanıt döndürecektir. DrM [örneğine](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L601)talebin nasıl ekleyeceğinize bakın.
 
#### <a name="considerations"></a>Dikkat edilmesi gerekenler

* Müşteriler belirteç üretimi üzerinde kontrole sahip olmalıdır. İddianın belirteç tesine yerleştirilmesi gerekir.
* Bu özelliği kullanırken, son kullanma süresi, istek alındığı andan itibaren bir saatten fazla olan belirteçlere sahip istekler yetkisiz bir yanıtla reddedilir.
* Belirteçler, imzalarıyla benzersiz bir şekilde tanımlanır. Taşıma yükündeki herhangi bir değişiklik (örneğin, son kullanma süresine veya talepte güncelleştirme) belirteç imzasını değiştirir ve Anahtar Tesliminin daha önce rastlamadığını yeni bir belirteç olarak sayılır.
* Belirteç müşteri tarafından ayarlanan `maxuses` değeri aşmışsa oynatma başarısız olur.
* Bu özellik, varolan tüm korumalı içerik için kullanılabilir (yalnızca verilen belirteç değiştirilmelidir).
* Bu özellik hem JWT hem de SWT ile çalışır.

## <a name="using-a-custom-sts"></a>Özel bir STS kullanma

Bir müşteri belirteçleri sağlamak için özel bir STS kullanmayı tercih edebilir. Nedenleri şunlardır:

* Müşteri tarafından kullanılan kimlik sağlayıcısı (IDP) STS'yi desteklemez. Bu durumda, özel bir STS bir seçenek olabilir.
* Müşterinin STS'yi müşterinin abone faturalandırma sistemiyle tümleştirmek için daha esnek veya daha sıkı kontrole ihtiyaç duyabilir.

   Örneğin, bir [OTT](https://en.wikipedia.org/wiki/Over-the-top_media_services) servis operatörü premium, temel ve spor gibi birden çok abone paketi sunabilir. Operatör, yalnızca belirli bir paketteki içeriğin kullanıma sunulması için bir belirteçteki talepleri abone paketiyle eşleştirmek isteyebilir. Bu durumda, özel bir STS gerekli esneklik ve denetimi sağlar.

* Farklı DRM lisans parametrelerine (kiralama lisansına karşı abonelik lisansı) sahip farklı ContentKeyPolicyOptions arasında seçim yapmak için belirteciözel talepler eklemek için.
* Belirteç erişim sağlayan anahtarın içerik anahtarı tanımlayıcısını temsil eden bir talep eklemek için.

Özel bir STS kullandığınızda, iki değişiklik yapılmalıdır:

* Bir varlık için lisans teslim hizmetini yapılandırdığınızda, Azure AD'nin geçerli anahtarı yerine özel STS tarafından doğrulama için kullanılan güvenlik anahtarını belirtmeniz gerekir.
* Bir JTW belirteci oluşturulduğunda, Azure AD'deki geçerli X509 sertifikasının özel anahtarı yerine bir güvenlik anahtarı belirtilir.

İki tür güvenlik anahtarı vardır:

* Simetrik anahtar: Aynı anahtar bir JWT oluşturmak ve doğrulamak için kullanılır.
* Asimetrik anahtar: X509 sertifikasındaki genel-özel anahtar çifti, bir JWT'yi şifrelemek/oluşturmak için özel bir anahtarla ve belirteci doğrulamak için ortak anahtarla birlikte kullanılır.

Geliştirme platformunuz olarak .NET Framework/C# kullanıyorsanız, asimetrik güvenlik anahtarı için kullanılan X509 sertifikasının en az 2048'lik anahtar uzunluğu olmalıdır. Bu anahtar uzunluğu class System.IdentityModel.Tokens.X509AsymmetricSecurityKey in .NET Framework bir gereksinimidir. Aksi takdirde, aşağıdaki özel durum atılır: IDX10630: 'System.IdentityModel.Tokens.X509AssymmetricSecurityKey' imzalama için '2048' bit daha küçük olamaz.

## <a name="custom-key-and-license-acquisition-url"></a>Özel anahtar ve lisans edinme URL'si

Farklı bir lisans/anahtar teslim hizmeti (Medya Hizmetleri değil) belirtmek istiyorsanız aşağıdaki şablonları kullanın. Şablonlardaki değiştirilebilir iki alan, varlık başına akış ilkesi oluşturmak yerine akış ilkenizi birçok varlık arasında paylaşabilmeniz için vardır. 

* `EnvelopeEncryption.CustomKeyAcquisitionUrlTemplate`: Son kullanıcı oyuncularına anahtar teslim eden özel hizmetin URL'si için şablon. Anahtarları vermek için Azure Medya Hizmetleri'ni kullanırken gerekli değildir. 

   Şablon, hizmetin isteke özgü değerle çalışma zamanında güncelleştireceği değiştirilebilir belirteçleri destekler.  Şu anda desteklenen belirteç değerleri şunlardır:
   * `{AlternativeMediaId}`, Hangi StreamingLocatorId.AlternativeMediaId değeri ile değiştirilir.
   * `{ContentKeyId}`, istenen anahtarın tanımlayıcısının değeriyle değiştirilir.
* `StreamingPolicyPlayReadyConfiguration.CustomLicenseAcquisitionUrlTemplate`: Son kullanıcı oyuncularına lisans teslim eden özel hizmetin URL'si için şablon. Lisans vermek için Azure Medya Hizmetlerini kullanırken gerekli değildir.

   Şablon, hizmetin isteke özgü değerle çalışma zamanında güncelleştireceği değiştirilebilir belirteçleri destekler. Şu anda desteklenen belirteç değerleri şunlardır:  
   * `{AlternativeMediaId}`, Hangi StreamingLocatorId.AlternativeMediaId değeri ile değiştirilir.
   * `{ContentKeyId}`, istenen anahtarın tanımlayıcısının değeriyle değiştirilir. 
* `StreamingPolicyWidevineConfiguration.CustomLicenseAcquisitionUrlTemplate`: Önceki şablonla aynıdır, yalnızca Widevine için. 
* `StreamingPolicyFairPlayConfiguration.CustomLicenseAcquisitionUrlTemplate`: Önceki şablonla aynıdır, yalnızca FairPlay için.  

Örnek:

```csharp
streamingPolicy.EnvelopEncryption.customKeyAcquisitionUrlTemplate = "https://mykeyserver.hostname.com/envelopekey/{AlternativeMediaId}/{ContentKeyId}";
```

`ContentKeyId`istenen anahtarın bir değeri vardır. İsteğinizin `AlternativeMediaId` sizin tarafınızdaki bir varlıkla eşlenebilmesini istediğinizde kullanabilirsiniz. Örneğin, `AlternativeMediaId` izinleri aramanıza yardımcı olmak için kullanılabilir.

Özel lisans/anahtar edinme URL'leri kullanan REST örnekleri için [Akış İlkeleri - Oluştur](https://docs.microsoft.com/rest/api/media/streamingpolicies/create)' a bakın.

> [!NOTE]
> Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="troubleshoot"></a>Sorun giderme

Hata alırsanız, `MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY` uygun akış ilkesini belirttiğinden emin olun.

Bununla `_NOT_SPECIFIED_IN_URL`biten hatalar alırsanız, URL'deki şifreleme biçimini belirttiğinden emin olun. `…/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)` bunun bir örneğidir. [Bkz. Akış protokolleri ve şifreleme türleri.](#streaming-protocols-and-encryption-types)

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

* [AES şifrelemeile koruyun](protect-with-aes128.md)
* [DRM ile koruyun](protect-with-drm.md)
* [Erişim kontrolü ile çoklu DRM içerik koruma sistemi tasarlayın](design-multi-drm-system-with-access-control.md)
* [Depolama tarafı şifreleme](storage-account-concept.md#storage-side-encryption)
* [Sık sorulan sorular](frequently-asked-questions.md)
* [JSON Web Belirteci İşleyicisi](https://docs.microsoft.com/dotnet/framework/security/json-web-token-handler)
