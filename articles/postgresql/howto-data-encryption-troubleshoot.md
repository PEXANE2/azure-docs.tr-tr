---
title: PostgreSQL için Azure veritabanı için veri şifreleme-tek sunuculu sorun giderme
description: PostgreSQL için Azure veritabanı-tek sunucu için veri şifreleme sorunlarını nasıl giderebileceğinizi öğrenin
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 6d2e027c47bef3186f95c2183b316b5c15511070
ms.sourcegitcommit: f255f869c1dc451fd71e0cab340af629a1b5fb6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/16/2020
ms.locfileid: "77371507"
---
# <a name="troubleshooting-data-encryption-with-customer-managed-keys-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda müşteri tarafından yönetilen anahtarlarla veri şifreleme sorunlarını giderme-tek sunucu
Bu makalede, bir PostgreSQL için Azure veritabanı 'nda oluşan genel sorunların/hataların nasıl tanımlanacağına ve çözümleneceği, müşteri tarafından yönetilen anahtar kullanılarak veri şifrelemesi ile yapılandırılmış tek bir sunucu.

## <a name="introduction"></a>Giriş
Veri şifreleme Azure Key Vault bir müşteri tarafından yönetilen anahtar kullanacak şekilde yapılandırıldığında, sunucunun kullanılabilir kalması için bu anahtara sürekli erişim gerekir. Sunucu, Azure Key Vault müşterinin yönettiği anahtara erişimi kaybederse, sunucu uygun hata iletisiyle tüm bağlantıları reddetmeye başlar ve Azure portal durumunu ***erişilemez*** olarak değiştirir.

PostgreSQL için erişilemeyen bir Azure veritabanı-tek sunucu artık gerekmiyorsa, maliyetleri durdurmak için hemen silinebilir. Azure Anahtar Kasası 'na erişim geri yüklenene ve sunucu kullanılabilir olana kadar sunucudaki diğer tüm eylemlere izin verilmez. "Evet" (müşteri tarafından yönetilen) seçeneğini, erişilemeyen sunucuda ' Hayır ' (hizmet tarafından yönetilen) olarak değiştirme işlemi, bir sunucu müşteri tarafından yönetildiğinde de mümkün değildir. Sunucuyu yeniden kullanılabilir hale getirmek için anahtarı el ile doğrulamanız gerekir. Bu, müşteri tarafından yönetilen anahtar izinleri iptal edildiğinde verileri yetkisiz erişimden korumak için gereklidir.

## <a name="common-errors-causing-server-to-become-inaccessible"></a>Sunucunun erişilemez hale gelmesine neden olan yaygın hatalar

Azure Key Vault ile veri şifrelemeyi kullandığınızda oluşan çoğu sorun aşağıdaki yanlış yapılandırma nedeniyle oluşur-

Anahtar Kasası kullanılamıyor veya yok

* Anahtar Kasası yanlışlıkla silindi.
* Aralıklı bir ağ hatası, anahtar kasasının kullanılamaz olmasına neden olur.

Anahtar kasasına erişim izni yok veya anahtar yok

* Anahtar yanlışlıkla silindi, devre dışı bırakıldı veya anahtarın geçerliliği zaman aşımına uğradı.
* PostgreSQL için Azure veritabanı-tek sunuculu örnek yönetilen kimlik yanlışlıkla silindi.
* Yönetilmeyen PostgreSQL için Azure veritabanı 'na verilen izinler, anahtarlar için yeterli değildir (Get, Wrap ve Unwrap dahil değildir).
* PostgreSQL için Azure veritabanı 'nın, tek sunuculu örnek yönetilen kimliğin izinleri iptal edildi veya silindi.

## <a name="identify-and-resolve-common-errors"></a>Sık karşılaşılan hataları tanımla ve çözümle
### <a name="errors-on-the-key-vault"></a>Anahtar kasasındaki hatalar

#### <a name="disabled-key-vault"></a>Devre dışı Anahtar Kasası
* AzureKeyVaultKeyDisabledMessage
* **Açıklama** : Azure Key Vault anahtarı devre dışı bırakıldığından, işlem sunucuda tamamlanamadı.

#### <a name="missing-key-vault-permissions"></a>Anahtar Kasası izinleri eksik
* AzureKeyVaultMissingPermissionsMessage
* Sunucu, Azure Key Vault izinleri için gereken get, Wrap ve Unwrap izinlerine sahip değil. Hizmet sorumlusuna KIMLIĞI olan eksik izinleri verin.

### <a name="mitigation"></a>Risk azaltma
* Key Vault müşterinin yönettiği anahtarın mevcut olduğunu onaylayın:
* Anahtar kasasını belirleyip Azure portal anahtar kasasına gidin.
* Anahtar URI 'SI tarafından tanımlanan anahtarın mevcut olduğundan emin olun.


## <a name="next-steps"></a>Sonraki adımlar
[Azure Portal kullanarak PostgreSQL Için Azure veritabanınız için müşteri tarafından yönetilen bir anahtarla veri şifrelemeyi ayarlayın](howto-data-encryption-portal.md).