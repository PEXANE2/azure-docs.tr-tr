---
title: Media Services-Azure 'da içerik anahtarı Ilkeleri | Microsoft Docs
description: Bu makalede, Içerik anahtarı Ilkelerinin ne olduğu ve Azure Media Services tarafından nasıl kullanıldıkları hakkında bir açıklama sunulmaktadır.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: conceptual
ms.date: 08/31/2020
ms.author: inhenkel
ms.custom: seodec18
ms.openlocfilehash: dd19d0730b25685b008af16d1c1eac6537750612
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89297278"
---
# <a name="content-key-policies"></a>İçerik Anahtar İlkeleri

[!INCLUDE [media services api v3 logo](./includes/v3-hr.md)]

Media Services ile canlı ve isteğe bağlı içeriğinizi Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sisteminden (Microsoft PlayReady, Google Widevine ve Apple FairPlay ile) dinamik olarak dağıtabilirsiniz. Media Services, yetkili istemcilere AES anahtarları ve DRM (PlayReady, Widevine ve FairPlay) lisanslarını sunmaya yönelik bir hizmet sağlar. 

Akışınızla ilgili şifreleme seçeneklerini belirtmek için bir [akış ilkesi](streaming-policy-concept.md) oluşturmanız ve bunu [akış konumunuzla](streaming-locators-concept.md)ilişkilendirmeniz gerekir. İçerik anahtarının ( [varlıklarınıza](assets-concept.md)güvenli erişim sağlayan) son istemcilere teslim edildiğini yapılandırmak Için [içerik anahtarı ilkesini](/rest/api/media/contentkeypolicies) oluşturursunuz. Belirtilen yapılandırmaya sahip anahtarların istemcilere teslim edilebilmesi için karşılanması gereken Içerik anahtarı Ilkesinde gereksinimleri (kısıtlamalar) ayarlamanız gerekir. İçerik anahtarı ilkesi, açık akış veya indirme için gerekli değildir. 

Genellikle, içerik anahtarı ilkenizi [akış konumunuzla](streaming-locators-concept.md)ilişkilendirirsiniz. Alternatif olarak, içerik anahtarı ilkesini bir [akış ilkesi](streaming-policy-concept.md) içinde belirtebilirsiniz (Gelişmiş senaryolar için özel bir akış ilkesi oluştururken). 

## <a name="best-practices-and-considerations"></a>En iyi uygulamalar ve noktalar

> [!IMPORTANT]
> Lütfen aşağıdaki önerileri gözden geçirin.

* Media Service hesabınız için sınırlı bir ilke kümesi tasarlamanızı ve aynı seçenek gerektiğinde bunları akış bulucular için yeniden kullanmanız gerekir. Daha fazla bilgi için bkz. [Kotalar ve sınırlar](limits-quotas-constraints.md).
* İçerik anahtarı ilkeleri güncelleştirilebilir. Anahtar teslimi önbellekler güncelleştirilmiş ilkeyi güncelleştirmek ve seçmek 15 dakika sürebilir. 

   İlkeyi güncelleştirerek, var olan CDN önbelleğinizin üzerine yazılır, bu da önbelleğe alınmış içerik kullanan müşteriler için kayıttan yürütme sorununa neden olabilir.  
* Her varlık için yeni bir içerik anahtar ilkesi oluşturmanız önerilir. Aynı ilke seçeneklerine ihtiyacı olan varlıklar arasında aynı içerik anahtarı ilkesini paylaşmanın başlıca avantajları şunlardır:
   
   * Az sayıda ilkeyi yönetmek daha kolaydır.
   * İçerik anahtarı ilkesinde güncelleştirmeler yapmanız gerekiyorsa, değişiklikler neredeyse hemen tüm yeni lisans istekleri üzerinde devreye girer.
* Yeni bir ilke oluşturmanız gerekiyorsa, varlık için yeni bir akış Bulucu oluşturmanız gerekir.
* İçerik anahtarını Media Services yeniden AutoGenerate sağlamak önerilir. 

   Genellikle, uzun süreli bir anahtar kullanır ve [Get](/rest/api/media/contentkeypolicies/get)ile içerik anahtarı ilkesinin varolup olmadığını kontrol edersiniz. Anahtarı almak için, parolaları veya kimlik bilgilerini almak üzere ayrı bir eylem yöntemi çağırmanız gerekir, aşağıdaki örneğe bakın.

## <a name="example"></a>Örnek

Anahtarı almak için, `GetPolicyPropertiesWithSecretsAsync` [mevcut ilke örneğinde bir Imzalama anahtarı al](get-content-key-policy-dotnet-howto.md#get-contentkeypolicy-with-secrets) bölümünde gösterildiği gibi kullanın.

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, sayfalama

Bkz. [Media Services varlıkların filtrelenmesi, sıralanması, sayfalama](entities-overview.md).

## <a name="additional-notes"></a>Ek notlar

* Türündeki Içerik anahtarı Ilkelerinin özellikleri `Datetime` her zaman UTC biçimindedir.
* Widevine, Google Inc. tarafından sunulan bir hizmettir ve Google, Inc 'nin hizmet koşullarına ve gizlilik Ilkesine tabidir.

## <a name="next-steps"></a>Sonraki adımlar

* [AES-128 dinamik şifreleme ve anahtar dağıtım hizmetini kullanma](protect-with-aes128.md)
* [DRM dinamik şifreleme ve lisans teslim hizmetini kullanma](protect-with-drm.md)
* [Encodehttpandpublishaesşifrelendi](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
