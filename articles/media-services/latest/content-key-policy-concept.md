---
title: Medya Hizmetlerinde İçerik Anahtar İlkeleri - Azure | Microsoft Dokümanlar
description: Bu makalede, İçerik Anahtarı İlkeleri'nin ne olduğu ve Azure Medya Hizmetleri tarafından nasıl kullanıldığı hakkında bir açıklama yer alıyor.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/26/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 7ddef1e78b4f8f62145e10b4cabc4537e28aba2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74969913"
---
# <a name="content-key-policies"></a>İçerik Anahtar İlkeleri

Media Services ile canlı ve isteğe bağlı içeriğinizi Gelişmiş Şifreleme Standardı (AES-128) veya üç büyük dijital haklar yönetimi (DRM) sisteminden herhangi biriyle dinamik olarak şifrelenmiş olarak teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve Apple FairPlay. Medya Hizmetleri ayrıca yetkili müşterilere AES anahtarları ve DRM (PlayReady, Widevine ve FairPlay) lisansları sunmak için bir hizmet sağlar. 

Akışınızda şifreleme seçenekleri belirtmek için bir [Akış İlkesi](streaming-policy-concept.md) oluşturmanız ve [akış bulucunuzla](streaming-locators-concept.md)ilişkilendirmeniz gerekir. İçerik anahtarının [(Varlıklarınıza](assets-concept.md)güvenli erişim sağlayan) son istemcilere nasıl teslim edilir şekilde yapılandırılmak için [İçerik Anahtarı İlkesi'ni](https://docs.microsoft.com/rest/api/media/contentkeypolicies) oluşturursunuz. Belirtilen yapılandırmaya sahip anahtarların istemcilere teslim edilebilmesi için, İçerik Anahtarı İlkesi'ndeki gereksinimleri (kısıtlamaları) ayarlamanız gerekir. İçerik anahtarı ilkesi, net akış veya indirme için gerekli değildir. 

Genellikle, içerik anahtarı ilkenizi [Akış lı Bulucunuzla](streaming-locators-concept.md)ilişkilendirirsiniz. Alternatif olarak, [Akış İlkesi](streaming-policy-concept.md) içindeki içerik anahtarı ilkesini belirtebilirsiniz (gelişmiş senaryolar için özel bir akış ilkesi oluştururken). 

## <a name="best-practices-and-considerations"></a>En iyi uygulamalar ve dikkat edilmesi gerekenler

> [!IMPORTANT]
> Lütfen aşağıdaki önerileri gözden geçirin.

* Medya Hizmeti hesabınız için sınırlı bir ilke kümesi tasarlamalı ve aynı seçenekler gerektiğinde akış bulucularınız için yeniden kullanmalısınız. Daha fazla bilgi için [Kotalar ve sınırlamalar](limits-quotas-constraints.md)bölümüne bakın.
* İçerik anahtar ilkeleri güncellenir. Anahtar teslim önbelleklerin güncelleştirilmiş ilkeyi güncelleştirmesi ve alması 15 dakika kadar sürebilir. 

   İlkeyi güncelleştirerek, önbelleğe alınmış içeriği kullanan müşteriler için oynatma sorununa neden olabilecek varolan CDN önbelleğinin üzerine binmiş olursunuz.  
* Her varlık için yeni bir içerik anahtarı ilkesi oluşturmamanızı öneririz. Aynı ilke seçeneklerine ihtiyaç duyan varlıklar arasında aynı içerik anahtar ilkesini paylaşmanın başlıca yararları şunlardır:
   
   * Az sayıda ilkeyi yönetmek daha kolaydır.
   * İçerik anahtarı ilkesinde güncelleştirmeler yapmanız gerekiyorsa, değişiklikler hemen tüm yeni lisans isteklerinde yürürlüğe girer.
* Yeni bir ilke oluşturmanız gerekiyorsa, varlık için yeni bir akış bulucu oluşturmanız gerekir.
* Medya Hizmetlerinin içerik anahtarını otomatik olarak oluşturmasına izin kullanılması önerilir. 

   Genellikle, uzun ömürlü bir anahtar kullanır ve [Get](https://docs.microsoft.com/rest/api/media/contentkeypolicies/get)ile içerik anahtarı ilkesinin varlığını kontrol edersiniz. Anahtarı almak için, sırları veya kimlik bilgilerini almak için ayrı bir eylem yöntemi ni aramanız gerekir, aşağıdaki örneğe bakın.

## <a name="example"></a>Örnek

Anahtara ulaşmak için, `GetPolicyPropertiesWithSecretsAsync` [varolan ilke örneğinden imza anahtarı al'da](get-content-key-policy-dotnet-howto.md#get-contentkeypolicy-with-secrets) gösterildiği gibi kullanın.

## <a name="filtering-ordering-paging"></a>Filtreleme, sipariş etme, sayfalama

Bkz. [Medya Hizmetleri varlıklarınıfiltreleme, sipariş etme, sayfalama.](entities-overview.md)

## <a name="additional-notes"></a>Ek notlar

* İçerik Anahtar İlkelerinin `Datetime` özellikleri her zaman UTC biçimindedir.
* Widevine, Google Inc. tarafından sağlanan ve Google, Inc.'in hizmet koşullarına ve Gizlilik Politikasına tabi olan bir hizmettir.

## <a name="next-steps"></a>Sonraki adımlar

* [AES-128 dinamik şifreleme ve anahtar dağıtım hizmetini kullanma](protect-with-aes128.md)
* [DRM dinamik şifreleme ve lisans teslim hizmetini kullanma](protect-with-drm.md)
* [KodlamaHTTPAndPublishAESŞifreli](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
