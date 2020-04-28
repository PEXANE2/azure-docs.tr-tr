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
ms.date: 02/26/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 3d0cb06f84fdd96d099e05f55ba62c37cb1192c7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "78183984"
---
# <a name="page-layout-versions"></a>Sayfa düzeni sürümleri

Sayfa düzeni paketleri, sayfa öğelerinde düzeltmeler ve iyileştirmeler içerecek şekilde düzenli olarak güncelleştirilir. Aşağıdaki değişiklik günlüğü her sürümde tanıtılan değişiklikleri belirtir.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="200"></a>2.0.0

- Otomatik olarak onaylanan sayfa (`selfasserted`)
  - Özel ilkelerde [görüntüleme denetimleri](display-controls.md) için destek eklendi.

## <a name="120"></a>1.2.0

- Tüm sayfalar
  - Erişilebilirlik düzeltmeleri
  - Artık CSS ve JavaScript için `data-preload="true"` yükleme sırasını denetlemek üzere [HTML etiketinizdeki](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) özniteliği ekleyebilirsiniz.
    - Bağlı CSS dosyalarını HTML şablonunuz ile aynı anda yükleyin; böylece dosyalar yüklenirken ' titreşimi ' yoktur.
    - `script` Etiketlerin alındığı ve sayfa yüklenmeden önce yürütüldüğü sırayı denetleyin.
  - E-posta alanı `type=email` artık ve mobil klavyeler doğru önerileri sağlayacak
  - Chrome çevirisi desteği
- Birleşik ve kendinden onaylanan sayfalar
  - Username/email ve Password alanları artık Edge ve Internet `form` Explorer 'ıN (IE) bu bilgileri düzgün bir şekilde kaydetmesine izin vermek için HTML öğesini kullanır.

## <a name="110"></a>1.1.0

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

## <a name="100"></a>1.0.0

- İlk yayın

## <a name="next-steps"></a>Sonraki adımlar

Özel ilkelerde uygulamalarınızın Kullanıcı arabirimini özelleştirmeye ilişkin ayrıntılar için bkz. [özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştirme](custom-policy-ui-customization.md).
