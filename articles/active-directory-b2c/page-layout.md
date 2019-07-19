---
title: Sayfa düzeni seçin-Azure Active Directory B2C
description: Azure Active Directory B2C bir sayfa düzeni seçme hakkında bilgi edinin.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 011fb262ff91c56269c5b7dc9adf4aaeab9acbd5
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68229056"
---
# <a name="select-a-page-layout-in-azure-active-directory-b2c-using-custom-policies"></a>Özel ilkeleri kullanarak Azure Active Directory B2C bir sayfa düzeni seçme

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Azure Active Directory (Azure AD) B2C ilkelerinizde JavaScript istemci tarafı kodunu, Kullanıcı akışları veya özel ilkeler kullanıp kullanmayacağınızı etkinleştirebilirsiniz. Uygulamalarınız için JavaScript 'i etkinleştirmek üzere [özel ilkenize](active-directory-b2c-overview-custom.md)bir öğe eklemeniz, bir sayfa düzeni seçmeniz ve isteklerinizin [b2clogin.com](b2clogin.md) kullanmanız gerekir.

Sayfa düzeni, Azure AD B2C sağladığı öğelerin ve sağladığınız içeriğin bir ilişkidir.

Bu makalede, bir özel ilkede yapılandırarak Azure AD B2C bir sayfa düzeninin nasıl görüntüleneceği açıklanır.

> [!NOTE]
> Kullanıcı akışları için JavaScript 'ı etkinleştirmek istiyorsanız, bkz. [Azure Active Directory B2C JavaScript ve sayfa düzeni sürümleri](user-flow-javascript-overview.md).

## <a name="replace-datauri-values"></a>DataUri değerleri Değiştir

Özel ilkelerinizi etkinleştirmiş olabilirsiniz [ContentDefinitions](contentdefinitions.md) kullanıcı yolculuğunda kullanılan HTML şablonları tanımlayın. **ContentDefinition** içeren bir **DataUri** Azure AD B2C tarafından sağlanan sayfa öğelerine başvurur. **LoadUri** sağladığınız HTML ve CSS içeriği göreli yolu.

```XML
<ContentDefinition Id="api.idpselections">
  <LoadUri>~/tenant/default/idpSelector.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Idp selection page</Item>
    <Item Key="language.intro">Sign in</Item>
  </Metadata>
</ContentDefinition>
```

Bir sayfa düzeni seçmek için ilkeleriniz içindeki [ContentDefinitions](contentdefinitions.md) Içindeki **datauri** değerlerini değiştirirsiniz. Eski geçiş tarafından **DataUri** değerleri için yeni değerleri, sabit bir paket seçeneğini belirliyoruz. Bu paket kullanmanın avantajı, değiştirme yüklemeyecekseniz ve beklenmeyen davranışlara neden sayfanızda anlarsınız ' dir.

Sayfa düzeni ayarlamak için, **Datauri** değerlerini bulmak için aşağıdaki tabloyu kullanın.

| Eski DataUri değer | Yeni DataUri değeri |
| ----------------- | ----------------- |
| `urn:com:microsoft:aad:b2c:elements:claimsconsent:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0` |

## <a name="version-change-log"></a>Sürüm değişiklik günlüğü

Sayfa düzeni paketleri, sayfa öğelerinde düzeltmeler ve iyileştirmeler içerecek şekilde düzenli olarak güncelleştirilir. Aşağıdaki değişiklik günlüğü her sürümde tanıtılan değişiklikleri belirtir.

### <a name="110"></a>1.1.0

- Özel durum sayfası (globalexception)
  - Erişilebilirlik onarımı
  - İlkeden hiçbir kişi olmadığında varsayılan ileti kaldırılmıştır
  - Varsayılan CSS kaldırıldı
- MFA sayfası (çok faktörlü)
  - ' Kodu onayla ' düğmesi kaldırıldı
  - Kod için giriş alanı artık altı (6) karaktere kadar olan girişleri alır
  - Sayfa, 6 basamaklı bir kod girildiğinde, tıklanmasına gerek kalmadan, otomatik olarak girilen kodu doğrulamaya çalışır
  - Kod yanlışsa, giriş alanı otomatik olarak temizlenir
  - Üç (3) yanlış kodla çalıştıktan sonra, B2C bağlı olan tarafa geri bir hata gönderir
  - Erişilebilirlik düzeltmeleri
  - Varsayılan CSS kaldırıldı
- Otomatik olarak onaylanan sayfa (selfasted)
  - İptal uyarısı kaldırıldı
  - Hata öğeleri için CSS sınıfı
  - Hata mantığını gelişmiş göster/gizle
  - Varsayılan CSS kaldırıldı
- Birleşik SSP (unifiedssp)
  - Oturumumu Açık tut (KMSı) denetimi eklendi

### <a name="100"></a>1.0.0

- İlk yayın

## <a name="next-steps"></a>Sonraki adımlar

Kullanıcı arabirimi, uygulamalarınızın nasıl özelleştirebileceğiniz hakkında daha fazla bilgi [özel bir ilke kullanarak Azure Active Directory B2C'de, uygulamanızın kullanıcı arabirimini özelleştirme](active-directory-b2c-ui-customization-custom.md).
