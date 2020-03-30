---
title: BuildingBlocks
titleSuffix: Azure AD B2C
description: Azure Active Directory B2C'de özel bir ilkenin BuildingBlocks öğesini belirtin.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 12/10/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: cf9c333af48e1c148dcd20ccf5ce91b284bd728b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78189880"
---
# <a name="buildingblocks"></a>BuildingBlocks

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

**BuildingBlocks** öğesi [TrustFrameworkPolicy](trustframeworkpolicy.md) öğesinin içine eklenir.

```XML
<TrustFrameworkPolicy
  xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
  xmlns:xsd="https://www.w3.org/2001/XMLSchema"
  xmlns="http://schemas.microsoft.com/online/cpim/schemas/2013/06"
  PolicySchemaVersion="0.3.0.0"
  TenantId="mytenant.onmicrosoft.com"
  PolicyId="B2C_1A_TrustFrameworkBase"
  PublicPolicyUri="http://mytenant.onmicrosoft.com/B2C_1A_TrustFrameworkBase">

  <BuildingBlocks>
    <ClaimsSchema>
      ...
    </ClaimsSchema>
    <Predicates>
    ...
    </Predicates>
    <PredicateValidations>
    ...
    </PredicateValidations>
    <ClaimsTransformations>
      ...
    </ClaimsTransformations>
    <ContentDefinitions>
      ...
    </ContentDefinitions>
    <Localization>
      ...
    </Localization>
    <DisplayControls>
      ...
    </DisplayControls>
 </BuildingBlocks>
```

**BuildingBlocks** öğesi tanımlanan sırada belirtilmesi gereken aşağıdaki öğeleri içerir:

- [ClaimsSchema](claimsschema.md) - İlkenin bir parçası olarak başvurulabilecek talep türlerini tanımlar. İddia şeması, talep türlerinizi beyan ettiğiniz yerdir. Talep türü, birçok programlı dilde bir değişkene benzer. Talep türünü, uygulamanızın kullanıcısından veri toplamak, sosyal kimlik sağlayıcılarından talep almak, özel bir REST API'sinden veri gönderip almak veya özel politikanız tarafından kullanılan dahili verileri depolamak için kullanabilirsiniz.

- [Yüklemler ve YüklemValidationsInput](predicates.md) - Yalnızca doğru biçimlendirilmiş verilerin bir hak talebine girilmesini sağlamak için bir doğrulama işlemi gerçekleştirmenizi sağlar.

- [İddiaDönüşümleri](claimstransformations.md) - İlkinizdeki talep dönüşümlerinin listesini içerir.  Talep dönüştürme, bir talebi diğerine dönüştürür. Talep dönüştürmesinde, aşağıdakiler gibi bir dönüştürme yöntemi belirtirsiniz:
  - Dize talebinin durumunu belirtilene değiştirme. Örneğin, bir dizeyi küçük harften büyük harfe değiştirme.
  - İki iddiayı karşılaştırmak ve bir iddiayı doğru olarak iade etmek, iddiaların eşleşip, aksi takdirde yanlış olduğunu gösterir.
  - İlkede sağlanan parametreden bir dize talebi oluşturma.
  - Rasgele sayı üreteci kullanarak rasgele bir dize oluşturma.
  - Bir talebi sağlanan biçim dizesine göre biçimlendirme. Bu dönüştürme C# `String.Format` yöntemini kullanır.

- InputValidation - Bu öğe, benzer *ve* *veya*boolean toplama gerçekleştirmenizi sağlar.

- [ContentDefinitions](contentdefinitions.md) - Kullanıcı yolculuğunuzda kullanmak üzere HTML5 şablonları için URL'ler içerir. Özel bir ilkede, içerik tanımı, kullanıcı yolculuğunda belirli bir adım için kullanılan HTML5 sayfası URI'yi tanımlar. Örneğin, oturum açma veya kaydolma, parola sıfırlama veya hata sayfaları. HTML5 dosyası için LoadUri'yi geçersiz kılarak görünüm ve his değiştirebilirsiniz. Veya ihtiyaçlarınıza göre yeni içerik tanımları oluşturabilirsiniz. Bu öğe, yerelleştirme kimliği kullanarak yerelleştirilmiş kaynaklar başvurusu içerebilir.

- [Yerelleştirme](localization.md) - Birden çok dili desteklemenizi sağlar. İlkelerdeki yerelleştirme desteği, bir ilkedeki desteklenen dillerin listesini ayarlamanıza ve varsayılan bir dil seçmenize olanak tanır. Dile özel dizeleri ve koleksiyonları da desteklenir.

- [DisplayControls](display-controls.md) - Bir sayfada görüntülenecek denetimleri tanımlar. Ekran denetimleri özel işlevsellik vardır ve arka uç doğrulama teknik profilleri ile etkileşim. Görüntü denetimleri şu anda **önizlemede.**
