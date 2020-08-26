---
title: Sayfa düzeni sürümleri
titleSuffix: Azure AD B2C
description: Özel ilkelerde UI özelleştirmesi için sayfa düzeni sürüm geçmişi.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 08/24/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 868d99a82009dc8545fc24ad1cfa1da3959da131
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88852065"
---
# <a name="page-layout-versions"></a>Sayfa düzeni sürümleri

Sayfa düzeni paketleri, sayfa öğelerinde düzeltmeler ve iyileştirmeler içerecek şekilde düzenli olarak güncelleştirilir. Aşağıdaki değişiklik günlüğü her sürümde tanıtılan değişiklikleri belirtir.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="self-asserted-page-selfasserted"></a>Otomatik olarak onaylanan sayfa (selfasted)

**2.1.0**

- Yerelleştirme ve erişilebilirlik düzeltmeleri.

**2.0.0**

- Özel ilkelerde [görüntüleme denetimleri](display-controls.md) için destek eklendi.

**1.2.0**

- Username/email ve Password alanları artık `form` Edge ve Internet Explorer 'ın (IE) bu bilgileri düzgün bir şekilde kaydetmesine izin vermek için HTML öğesini kullanır.
- Geliştirilmiş kullanıcı deneyimi için yapılandırılabilir bir kullanıcı girişi doğrulama gecikmesi eklendi.
- Erişilebilirlik düzeltmeleri
- Artık `data-preload="true"` CSS ve JavaScript için yükleme sırasını denetlemek üzere [HTML etiketinizdeki](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerileri sağlayacak
- Chrome çevirisi desteği

**1.1.0**

- İptal uyarısı kaldırıldı
- Hata öğeleri için CSS sınıfı
- Hata mantığını gelişmiş göster/gizle
- Varsayılan CSS kaldırıldı

**1.0.0**

- İlk yayın

## <a name="unified-sign-in-sign-up-page-with-password-reset-link-unifiedssp"></a>Parola sıfırlama bağlantısı ile Birleşik oturum açma sayfası (unifiedssp)

**2.1.0**

- Birden çok kaydolma bağlantısı için destek eklendi.
- İlkede tanımlanan koşul kurallarına göre Kullanıcı girişi doğrulaması için destek eklendi.

**1.2.0**

- Username/email ve Password alanları artık `form` Edge ve Internet Explorer 'ın (IE) bu bilgileri düzgün bir şekilde kaydetmesine izin vermek için HTML öğesini kullanır.
- Erişilebilirlik düzeltmeleri
- Artık `data-preload="true"` CSS ve JavaScript için yükleme sırasını denetlemek üzere [HTML etiketinizdeki](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerileri sağlayacak
- Chrome çevirisi desteği

**1.1.0**

- Oturumumu Açık tut (KMSı) denetimi eklendi

**1.0.0**

- İlk yayın

## <a name="mfa-page-multifactor"></a>MFA sayfası (çok faktörlü)

**1.2.1**

- Varsayılan şablonlarda erişilebilirlik düzeltmeleri

**1.2.0**

- Erişilebilirlik düzeltmeleri
- Artık `data-preload="true"` CSS ve JavaScript için yükleme sırasını denetlemek üzere [HTML etiketinizdeki](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerileri sağlayacak
- Chrome çevirisi desteği

**1.1.0**

- ' Kodu onayla ' düğmesi kaldırıldı
- Kod için giriş alanı artık altı (6) karaktere kadar olan girişleri alır
- Sayfa, 6 basamaklı bir kod girildiğinde, tıklanmasına gerek kalmadan, otomatik olarak girilen kodu doğrulamaya çalışır
- Kod yanlışsa, giriş alanı otomatik olarak temizlenir
- Üç (3) yanlış kodla çalıştıktan sonra, B2C bağlı olan tarafa geri bir hata gönderir
- Erişilebilirlik düzeltmeleri
- Varsayılan CSS kaldırıldı

**1.0.0**

- İlk yayın

## <a name="exception-page-globalexception"></a>Özel durum sayfası (globalexception)

**1.2.0**

- Erişilebilirlik düzeltmeleri
- Artık `data-preload="true"` CSS ve JavaScript için yükleme sırasını denetlemek üzere [HTML etiketinizdeki](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerileri sağlayacak
- Chrome çevirisi desteği

**1.1.0**

- Erişilebilirlik onarımı
- İlkeden hiçbir kişi olmadığında varsayılan ileti kaldırılmıştır
- Varsayılan CSS kaldırıldı

**1.0.0**

- İlk yayın

## <a name="other-pages-providerselection-claimsconsent-unifiedssd"></a>Diğer sayfalar (ProviderSelection, Claimsonay, UnifiedSSD)

**1.2.0**

- Erişilebilirlik düzeltmeleri
- Artık `data-preload="true"` CSS ve JavaScript için yükleme sırasını denetlemek üzere [HTML etiketinizdeki](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
  - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
  - `script`Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
- E-posta alanı artık `type=email` ve mobil klavyeler doğru önerileri sağlayacak
- Chrome çevirisi desteği

**1.0.0**

- İlk yayın

## <a name="next-steps"></a>Sonraki adımlar

Özel ilkelerde uygulamalarınızın Kullanıcı arabirimini özelleştirmeye ilişkin ayrıntılar için bkz. [özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme](custom-policy-ui-customization.md).
