---
title: Sayfa düzeni sürümleri
titleSuffix: Azure AD B2C
description: Özel ilkelerde Kullanıcı Arabirimi özelleştirmesi için sayfa düzeni sürüm geçmişi.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78183984"
---
# <a name="page-layout-versions"></a>Sayfa düzeni sürümleri

Sayfa düzeni paketleri, sayfa öğelerine düzeltmeleri ve iyileştirmeleri içerecek şekilde düzenli olarak güncelleştirilir. Aşağıdaki değişiklik günlüğü, her sürümde tanıtılan değişiklikleri belirtir.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="200"></a>2.0.0

- Kendi kendine ileri`selfasserted`sayfa ( )
  - Özel ilkelerde [görüntü denetimleri](display-controls.md) için destek eklendi.

## <a name="120"></a>1.2.0

- Tüm sayfalar
  - Erişilebilirlik düzeltmeleri
  - Artık `data-preload="true"` CSS ve JavaScript için yük sırasını denetlemek için [HTML etiketleriöz](custom-policy-ui-customization.md#guidelines-for-using-custom-page-content) ekleyebilirsiniz.
    - Bağlantılı CSS dosyalarını HTML şablonunuzun yüklemesi arasında 'titrememesi' için HTML şablonunuzun aynı anda yükleyin.
    - Etiketlerinizin `script` sayfa yüklemesinden önce getirilip yürütülme sırasını denetleyin.
  - E-posta `type=email` alanı şimdi ve mobil klavye doğru öneriler sağlayacaktır
  - Chrome çevirisi desteği
- Birleşik ve kendini öne süren sayfalar
  - Kullanıcı adı/e-posta ve parola `form` alanları artık Edge ve Internet Explorer'ın (IE) bu bilgileri düzgün bir şekilde kaydetmesine izin vermek için HTML öğesini kullanmaktadır.

## <a name="110"></a>1.1.0

- Özel durum sayfası (genel özel durum)
  - Erişilebilirlik düzeltmesi
  - İlkeden bir kişi olmadığında varsayılan iletiyi kaldırma
  - Varsayılan CSS kaldırıldı
- MFA sayfası (çok faktörlü)
  - 'Kodu Onayla' düğmesi kaldırıldı
  - Kodun giriş alanı artık yalnızca altı (6) karaktere kadar giriş alır
  - Sayfa, herhangi bir düğmeye tıklamak zorunda kalmadan, 6 haneli bir kod girildiğinde girilen kodu otomatik olarak doğrulamaya çalışır
  - Kod yanlışsa, giriş alanı otomatik olarak temizlenir
  - Yanlış kodla yapılan üç (3) denemeden sonra, B2C bir hatayı güvenen tarafa geri gönderir
  - Erişilebilirlik düzeltmeleri
  - Varsayılan CSS kaldırıldı
- Kendi kendini öne süren sayfa (selfasserted)
  - İptal uyarısı kaldırıldı
  - Hata elemanları için CSS sınıfı
  - Hata mantığı geliştirildi göster/gizle
  - Varsayılan CSS kaldırıldı
- Birleşik SSP (unifiedssp)
  - (KMSI) denetiminde oturum u tut beni tut eklendi

## <a name="100"></a>1.0.0

- İlk yayın

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınızın kullanıcı arabirimini özel ilkelerde özelleştirme hakkında ayrıntılı bilgi için, [özel bir ilke kullanarak uygulamanızın kullanıcı arabirimini özelleştir'e](custom-policy-ui-customization.md)bakın.
