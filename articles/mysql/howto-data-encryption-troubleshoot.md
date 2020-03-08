---
title: MySQL için Azure veritabanı 'nda veri şifreleme sorunlarını giderme
description: MySQL için Azure veritabanı 'nda veri şifreleme sorunlarını giderme hakkında bilgi edinin
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 516f0b2080fc894ec00f222c712ffdea4ee74356
ms.sourcegitcommit: f5e4d0466b417fa511b942fd3bd206aeae0055bc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78851110"
---
# <a name="troubleshoot-data-encryption-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda veri şifreleme sorunlarını giderme

Bu makalede, müşteri tarafından yönetilen anahtar kullanılarak veri şifrelemesi ile yapılandırıldığında MySQL için Azure veritabanı 'nda oluşabilecek yaygın sorunların nasıl tanımlanacağına ve çözümleneceği açıklanır.

## <a name="introduction"></a>Giriş

Veri şifrelemesini Azure Key Vault bir müşteri tarafından yönetilen anahtar kullanacak şekilde yapılandırdığınızda, sunucular anahtara sürekli erişim gerektirir. Sunucu, Azure Key Vault müşterinin yönettiği anahtara erişimi kaybederse, tüm bağlantıları reddeder, uygun hata iletisini döndürür ve durumunu Azure portal ***erişilemez*** olarak değiştirir.

Artık bir MySQL için Azure veritabanı sunucusuna erişim gerekmiyorsa, maliyetleri durdurmak için bunu silebilirsiniz. Anahtar kasasına erişim geri yüklenene ve sunucu kullanılabilir olana kadar sunucuda başka bir eyleme izin verilmez. `Yes`(müşteri tarafından yönetilen), müşteri tarafından yönetilen bir anahtarla şifrelendiğinde erişilemeyen bir sunucuda `No` (hizmet tarafından yönetilen) olarak değişiklik de mümkün değildir. Sunucuya yeniden erişilebilmesi için anahtarı el ile yeniden doğrulamanız gerekecektir. Bu eylem, müşteri tarafından yönetilen anahtar izinleri iptal edilirken verileri yetkisiz erişimden korumak için gereklidir.

## <a name="common-errors-that-cause-the-server-to-become-inaccessible"></a>Sunucunun erişilemez duruma gelmesine neden olan yaygın hatalar

Aşağıdaki yapılandırma hataları Azure Key Vault anahtarları kullanan veri şifrelemesiyle ilgili birçok soruna neden olur:

- Anahtar Kasası kullanılamıyor veya yok:
  - Anahtar Kasası yanlışlıkla silindi.
  - Aralıklı bir ağ hatası, anahtar kasasının kullanılamaz olmasına neden olur.

- Anahtar kasasına erişim izniniz yok veya anahtar yok:
  - Anahtarın geçerliliği zaman aşımına uğradı veya yanlışlıkla silinmiş veya devre dışı bırakılmış.
  - MySQL için Azure veritabanı örneğinin yönetilen kimliği yanlışlıkla silindi.
  - MySQL için Azure veritabanı örneği için yönetilen kimliğin anahtar izinleri yetersiz. Örneğin, izinler Get, Wrap ve Unwrap içermemelidir.
  - MySQL için Azure veritabanı örneği için yönetilen kimlik izinleri iptal edildi veya silindi.

## <a name="identify-and-resolve-common-errors"></a>Sık karşılaşılan hataları tanımla ve çözümle

### <a name="errors-on-the-key-vault"></a>Anahtar kasasındaki hatalar

#### <a name="disabled-key-vault"></a>Devre dışı Anahtar Kasası

- `AzureKeyVaultKeyDisabledMessage`
- **Açıklama**: Azure Key Vault anahtarı devre dışı bırakıldığından, işlem sunucuda tamamlanamadı.

#### <a name="missing-key-vault-permissions"></a>Anahtar Kasası izinleri eksik

- `AzureKeyVaultMissingPermissionsMessage`
- **Açıklama**: sunucu, Azure Key Vault Için gereken get, Wrap ve Unwrap izinlerine sahip değil. Hizmet sorumlusuna KIMLIĞI olan eksik izinleri verin.

### <a name="mitigation"></a>Risk azaltma

- Müşteri tarafından yönetilen anahtarın anahtar kasasında mevcut olduğunu doğrulayın.
- Anahtar kasasını belirleyip Azure portal anahtar kasasına gidin.
- Anahtar URI 'sinin mevcut bir anahtarı tanımladığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[MySQL için Azure veritabanı 'nda müşteri tarafından yönetilen bir anahtarla veri şifrelemeyi ayarlamak için Azure portal kullanın](howto-data-encryption-portal.md)
