---
title: Veri şifreleme sorunlarını giderme-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı 'nda veri şifreleme sorunlarını nasıl giderebileceğinizi öğrenin-tek sunucu
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 2902ff17ac14a48f1a11259339c2ab1bc4595980
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79299269"
---
# <a name="troubleshoot-data-encryption-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda veri şifreleme sorunlarını giderme-tek sunucu

Bu makale, müşteri tarafından yönetilen anahtar kullanılarak veri şifrelemesi ile yapılandırıldığında PostgreSQL için Azure veritabanı 'nın tek sunuculu dağıtımında oluşabilecek yaygın sorunları belirlemenize ve çözmenize yardımcı olur.

## <a name="introduction"></a>Giriş

Veri şifrelemesini Azure Key Vault bir müşteri tarafından yönetilen anahtar kullanacak şekilde yapılandırdığınızda, sunucu anahtara sürekli erişim gerektirir. Sunucu, Azure Key Vault müşterinin yönettiği anahtara erişimi kaybederse, tüm bağlantıları reddeder, uygun hata iletisini döndürür ve durumunu Azure portal ***erişilemez*** olarak değiştirir.

Artık PostgreSQL için Azure veritabanı 'na erişilemiyor sunucusuna ihtiyacınız yoksa, maliyetleri durdurmak için bunu silebilirsiniz. Anahtar kasasına erişim geri yüklenene ve sunucu kullanılabilir olana kadar sunucuda başka bir eyleme izin verilmez. Ayrıca, müşteri tarafından yönetilen bir anahtarla şifrelendiğinde erişilemeyen bir sunucuda `Yes`veri şifreleme seçeneğini (müşteri tarafından `No` yönetilen) olarak değiştirmek mümkün değildir. Sunucuya yeniden erişilebilmesi için anahtarı el ile yeniden doğrulamanız gerekecektir. Bu eylem, müşteri tarafından yönetilen anahtar izinleri iptal edilirken verileri yetkisiz erişimden korumak için gereklidir.

## <a name="common-errors-causing-server-to-become-inaccessible"></a>Sunucunun erişilemez hale gelmesine neden olan yaygın hatalar

Aşağıdaki yapılandırma hataları Azure Key Vault anahtarları kullanan veri şifrelemesiyle ilgili birçok soruna neden olur:

- Anahtar Kasası kullanılamıyor veya yok:
  - Anahtar Kasası yanlışlıkla silindi.
  - Aralıklı bir ağ hatası, anahtar kasasının kullanılamaz olmasına neden olur.

- Anahtar kasasına erişim izniniz yok veya anahtar yok:
  - Anahtarın geçerliliği zaman aşımına uğradı veya yanlışlıkla silinmiş veya devre dışı bırakılmış.
- PostgreSQL için Azure veritabanı örneğinin yönetilen kimliği yanlışlıkla silindi.
  - PostgreSQL için Azure veritabanı örneği için yönetilen kimliğin anahtar izinleri yetersiz. Örneğin, izinler Get, Wrap ve Unwrap içermemelidir.
  - PostgreSQL için Azure veritabanı örneği için yönetilen kimlik izinleri iptal edildi veya silindi.

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

[PostgreSQL için Azure veritabanı 'nda müşteri tarafından yönetilen bir anahtarla veri şifrelemeyi ayarlamak için Azure portal kullanın](howto-data-encryption-portal.md)
