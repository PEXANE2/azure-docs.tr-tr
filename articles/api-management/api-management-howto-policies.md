---
title: Azure API Yönetiminde İlkeler | Microsoft Dokümanlar
description: API Yönetimi'nde ilkeleri nasıl oluşturup, nasıl döşeyecek ve yapılandırılamayı öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/29/2017
ms.author: apimpm
ms.openlocfilehash: c10939b50a66cd608d27a71f02d959fbc2380f59
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70072316"
---
# <a name="policies-in-azure-api-management"></a>Azure API Management ilkeleri

Azure API Yönetimi'nde (APIM) ilkeler, yayımcının yapılandırma yoluyla API'nin davranışını değiştirmesine olanak tanıyan güçlü bir sistem yeteneğidir. İlkeler, bir API'nin isteği veya yanıtı üzerine sırayla yürütülen deyimler topluluğu. Popüler Deyimler, XML'den JSON'a biçim dönüştürme ve geliştiriciden gelen arama miktarını kısıtlamak için sınırlayıcı arama oranını içerir. Kutunun dışında çok daha fazla ilke kullanılabilir.

İlkeler, API tüketicisi ile yönetilen API arasında bulunan ağ geçidiiçinde uygulanır. Ağ geçidi tüm istekleri alır ve genellikle bunları temel API'ye değiştirilmeden iletir. Ancak bir ilke hem gelen istek hem de giden yanıt değişiklikleri uygulayabilir.

İlke ifadeleri herhangi bir API Management ilkesinde, ilke aksini belirtmedikçe, öznitelik değerleri ya da metin değerleri olarak kullanılabilir. [Akışı denetle][Control flow] ve [Değişken ayarla][Set variable] gibi bazı ilkeler ilke ifadelerini temel alır. Daha fazla bilgi için bkz. [Gelişmiş ilkeler][Advanced policies] ve [İlke ifadeleri][Policy expressions].

## <a name="understanding-policy-configuration"></a><a name="sections"> </a>İlke yapılandırması anlama

İlke tanımı, gelen ve giden deyimler dizisi açıklayan basit bir XML belgesidir. XML doğrudan tanım penceresinde düzenlenebilir. İfadelerin listesi sağa sağlanır ve geçerli kapsam için geçerli ifadeler etkinleştirilir ve vurgulanır.

Etkin bir ifadeyi tıklatmak, tanım görünümündeki imlecin yerine uygun XML'yi ekler. 

> [!NOTE]
> Eklemek istediğiniz ilke etkin değilse, bu ilke için doğru kapsamda olduğunuzu emin olun. Her ilke bildirimi belirli kapsamlarda ve ilke bölümlerinde kullanılmak üzere tasarlanmıştır. İlkenin ilke bölümlerini ve kapsamlarını gözden geçirmek için, [İlke Başvurusu'ndaki][Policy Reference]bu ilkenin **Kullanım** bölümünü denetleyin.
> 
> 

Yapılandırma `inbound`, , `backend` `outbound`, ve `on-error`. Belirtilen ilke deyimleri serisi, istek ve yanıt için yürütülür.

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies> 
```

Bir isteğin işlenmesi sırasında bir hata varsa, `inbound`, `backend`, `outbound` veya bölümlerde kalan adımlar atlanır ve `on-error` yürütme bölümündeki ifadeleratlar. Bölüme `on-error` ilke ekstreleri yerleştirerek `context.LastError` özelliği kullanarak hatayı gözden geçirebilir, `set-body` ilkeyi kullanarak hata yanıtını inceleyebilir ve özelleştirebilir ve bir hata oluşursa ne olacağını yapılandırabilirsiniz. Yerleşik adımlar ve ilke bildirimlerinin işlenmesi sırasında oluşabilecek hatalar için hata kodları vardır. Daha fazla bilgi [için, API Yönetimi ilkelerinde Hata işleme'ye](/azure/api-management/api-management-error-handling-policies)bakın.

## <a name="how-to-configure-policies"></a><a name="scopes"> </a>İlkeler nasıl yapılandırılır?

İlkeleri yapılandırma hakkında bilgi için ilkeleri [Ayarla veya edin.](set-edit-policies.md)

## <a name="policy-reference"></a>İlke Başvurusu

İlke bildirimlerinin ve ayarlarının tam listesi için [İlke başvurusuna](api-management-policy-reference.md) bakın.

## <a name="policy-samples"></a>İlke örnekleri

Daha fazla kod örneği için [İlke örneklerine](policy-samples.md) bakın.

## <a name="examples"></a>Örnekler

### <a name="apply-policies-specified-at-different-scopes"></a>Farklı kapsamlarda belirtilen ilkeleri uygulama

Genel düzeyde bir politikanız ve bir API için yapılandırılan bir ilkeniz varsa, belirli bir API kullanıldığında her iki ilke de uygulanır. API Yönetimi, temel öğe üzerinden birleştirilmiş ilke deyimlerinin deterministik sıralanmasına olanak tanır. 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

Yukarıdaki örnek ilke tanımında, `cross-domain` deyim, `find-and-replace` eiðen izlenecek daha yüksek ilkelerden önce yürütür. 

### <a name="restrict-incoming-requests"></a>Gelen istekleri kısıtlama

Gelen istekleri belirtilen IP adresleriyle sınırlamak için yeni bir deyim eklemek için imleci `inbound` XML öğesinin içeriğinin içine yerleştirin ve **arayan IP'leri kısıtla** ifadesini tıklatın.

![Kısıtlama ilkeleri][policies-restrict]

Bu, deyimin `inbound` nasıl yapılandırılacağına ilişkin kılavuz sağlayan öğeye bir XML parçacığı ekler.

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

Gelen istekleri sınırlamak ve yalnızca 1.2.3.4 IP adresinden gelenleri kabul etmek için XML'i aşağıdaki gibi değiştirin:

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

## <a name="next-steps"></a>Sonraki adımlar

İlkelerle çalışan daha fazla bilgi için bkz:

+ [API'leri Dönüştür](transform-api.md)
+ [İlke](api-management-policy-reference.md) deyimlerinin ve ayarlarının tam listesi için İlke Başvurusu
+ [İlke örnekleri](policy-samples.md)   

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
