---
title: Azure portalını kullanarak içerik anahtarı yetkilendirme ilkesini yapılandırma | Microsoft Dokümanlar
description: Bu makalede, bir içerik anahtarı için bir yetkilendirme ilkesi nasıl yapılandırılabildiğini gösterin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: ee82a3fa-c34b-48f2-a108-8ba321f1691e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/19/2019
ms.author: juliako
ms.openlocfilehash: 8580bafd4d68ef6567b09fefcaa01c682ae2cafe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74968808"
---
# <a name="configure-a-content-key-authorization-policy"></a>İçerik anahtarı yetkilendirme ilkesini yapılandırma
[!INCLUDE [media-services-selector-content-key-auth-policy](../../../includes/media-services-selector-content-key-auth-policy.md)]

## <a name="overview"></a>Genel Bakış
 128 bit şifreleme anahtarları veya [PlayReady dijital haklar yönetimi (DRM)](https://www.microsoft.com/playready/overview/)kullanarak Gelişmiş Şifreleme Standardı (AES) ile korunan MPEG-DASH, Sorunsuz Akış ve HTTP Canlı Akış (HLS) akışlarını sunmak için Azure Medya Hizmetlerini kullanabilirsiniz. Medya Hizmetleri ile Widevine DRM ile şifrelenmiş DASH akışlarını da teslim edebilirsiniz. PlayReady ve Widevine, ortak şifreleme (ISO/IEC 23001-7 CENC) belirtimi uyarınca şifrelenir.

Medya Hizmetleri ayrıca, müşterilerin şifrelenmiş içeriği oynatmak için AES anahtarları veya PlayReady/Widevine lisansları alabilecekleri bir anahtar/lisans teslim hizmeti de sağlar.

Bu makalede, içerik anahtarı yetkilendirme ilkesini yapılandırmak için Azure portalının nasıl kullanılacağı gösterilmektedir. Anahtar daha sonra içeriğinizi dinamik olarak şifrelemek için kullanılabilir. Şu anda HLS, MPEG-DASH ve Düzgün Akış biçimlerini şifreleyebilirsiniz. Aşamalı indirmeleri şifreleyemezsiniz.

Bir oynatıcı dinamik olarak şifrelenecek şekilde ayarlanmış bir akış istediğinde, Medya Hizmetleri Yapılandırılmış anahtarı kullanarak AES veya DRM şifrelemesi kullanarak içeriğinizi dinamik olarak şifreler. Oynatıcı, akışın şifresini çözmek için anahtar teslim hizmetinden anahtarı ister. Kullanıcının anahtarı almaya yetkili olup olmadığını belirlemek için, hizmet anahtar için belirlediğiniz yetkilendirme ilkelerini değerlendirir.

Birden çok içerik anahtarına sahip olmayı planlıyorsanız veya Medya Hizmetleri anahtar teslim hizmeti dışında bir anahtar/lisans teslim hizmeti URL'si belirtmek istiyorsanız, Medya Hizmetleri .NET SDK veya REST API'lerini kullanın. Daha fazla bilgi için bkz.

* [Medya Hizmetleri .NET SDK'yı kullanarak içerik anahtarı yetkilendirme ilkesini yapılandırma](media-services-dotnet-configure-content-key-auth-policy.md)
* [Medya Hizmetleri REST API'sini kullanarak içerik anahtarı yetkilendirme ilkesini yapılandırma](media-services-rest-configure-content-key-auth-policy.md)

### <a name="some-considerations-apply"></a>Bazı hususlar geçerlidir
* Media Services hesabınız oluşturulduğunda hesabınıza “Durdurulmuş” durumda bir varsayılan akış uç noktası eklenir. İçeriğinizi akışa başlamak ve dinamik paketleme ve dinamik şifrelemeden yararlanmak için akış bitiş noktanızın "Çalışan" durumunda olması gerekir. 
* Kıymetiniz bir dizi uyarlanabilir bitrate MP4 veya uyarlanabilir bithızı Düzgün Akış dosyaları içermelidir. Daha fazla bilgi için [bkz.](media-services-encode-asset.md)
* Anahtar teslim hizmeti ContentKeyAuthorizationPolicy ve ilgili nesneleri (ilke seçenekleri ve kısıtlamaları) 15 dakika için önbelleğe alınr. Bir ContentKeyAuthorizationPolicy oluşturabilir ve belirterek belirterek belirterek belirterek belirtebilir, bunu sınayabilir ve sonra açık kısıtlamayla ilkeyi güncelleştirebilirsiniz. Bu işlem, ilkeğin açık sürüme geçmeden önce yaklaşık 15 dakika sürer.
* Bir Medya Hizmetleri akışı bitiş noktası, ÖN KONTROL yanıtındaki CORS Access-Control-Allow-Origin\*üstbilgisinin değerini joker karakter olarak ayarlar. Bu değer, Azure Media Player, Roku ve JWPlayer ve diğerleri dahil olmak üzere çoğu oyuncuda iyi çalışır. Ancak, dash.js kullanan bazı oyuncular çalışmaz, çünkü kimlik bilgileri modu "dahil etmek" için ayarlanmış, XMLHttpRequest\*kendi dash.js joker " " " Erişim-Denetim-İzin-Origin değeri olarak izin vermez. Dash.js'deki bu sınırlamaya geçici çözüm olarak, istemcinizi tek bir etki alanından barındırıyorsanız, Medya Hizmetleri bu etki alanını ön uçuş yanıtı üstbilgisinde belirtebilir. Yardım için Azure portalı üzerinden bir destek bileti açın.

## <a name="configure-the-key-authorization-policy"></a>Anahtar yetkilendirme ilkesini yapılandırma
Anahtar yetkilendirme ilkesini yapılandırmak için **İçERİk KORUMA** sayfasını seçin.

Medya Hizmetleri, anahtar isteklerini gerçekleştiren kullanıcıların kimliğini doğrulamanın birden çok yolunu destekler. İçerik anahtarı yetkilendirme ilkesi açık, belirteç veya IP yetkilendirme kısıtlamalarına sahip olabilir. (IP REST veya .NET SDK ile yapılandırılabilir.)

### <a name="open-restriction"></a>Açık kısıtlama
Açık kısıtlama, sistemin anahtarı önemli bir istekte bulunan herkese teslim etmesi anlamına gelir. Bu kısıtlama sınama amacıyla yararlı olabilir.

![OpenPolicy][open_policy]

### <a name="token-restriction"></a>Belirteç kısıtlaması
Belirteç kısıtlı ilkesini seçmek için **TOKEN** düğmesini seçin.

Belirteç kısıtlı ilkesi, bir güvenlik belirteci hizmeti (STS) tarafından verilen bir belirteç ile birlikte olmalıdır. Medya Hizmetleri, basit web belirteci[(SWT)](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2)ve JSON Web Belirteci (JWT) biçimlerindeki belirteçleri destekler. Daha fazla bilgi için [Bkz. JWT kimlik doğrulaması.](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/)

Medya Hizmetleri STS sağlamaz. Belirteçleri vermek için özel bir STS oluşturabilirsiniz. STS belirtilmiş anahtar ve belirteç kısıtlama yapılandırmasında belirttiğiniz sorun talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Belirteç geçerliyse ve belirteçteki talepler içerik anahtarı için yapılandırılanlarla eşleşirse, Medya Hizmetleri anahtar teslim hizmeti şifreleme anahtarını istemciye döndürür.

Belirteç kısıtlamalı ilkeyi yapılandırdığınızda, birincil doğrulama anahtarını, vereni ve hedef kitle parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı, belirteçle imzalandığı anahtarı içerir. İhraççı, belirteci veren STS'dir. Hedef kitle (bazen kapsam olarak adlandırılır), belirteci veya belirteç erişim eizin kaynağının amacını açıklar. Medya Hizmetleri anahtar teslim hizmeti, belirteçteki bu değerlerin şablondaki değerlerle eşleştirdiğini doğrular.

### <a name="playready"></a>PlayReady
İçeriğinizi PlayReady ile koruduğunuzda, yetkilendirme politikanızda belirtmeniz gereken şeylerden biri PlayReady lisans şablonunu tanımlayan bir XML dizesidir. Varsayılan olarak, aşağıdaki ilke ayarlanır:

    <PlayReadyLicenseResponseTemplate xmlns:i="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/Azure/MediaServices/KeyDelivery/PlayReadyTemplate/v1">
          <LicenseTemplates>
            <PlayReadyLicenseTemplate><AllowTestDevices>true</AllowTestDevices>
              <ContentKey i:type="ContentEncryptionKeyFromHeader" />
              <LicenseType>Nonpersistent</LicenseType>
              <PlayRight>
                <AllowPassingVideoContentToUnknownOutput>Allowed</AllowPassingVideoContentToUnknownOutput>
              </PlayRight>
            </PlayReadyLicenseTemplate>
          </LicenseTemplates>
        </PlayReadyLicenseResponseTemplate>

Alma ilkesi **xml** düğmesini seçebilir ve [Medya Hizmetleri PlayReady lisans şablonuna genel bakışta](media-services-playready-license-template-overview.md)tanımlanan XML şemasına uygun farklı bir XML sağlayabilirsiniz.

## <a name="additional-notes"></a>Ek notlar

* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

[open_policy]: ./media/media-services-portal-configure-content-key-auth-policy/media-services-protect-content-with-open-restriction.png
[token_policy]: ./media/media-services-key-authorization-policy/media-services-protect-content-with-token-restriction.png

