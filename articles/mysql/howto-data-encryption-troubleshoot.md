---
title: Sorun Giderme veri şifreleme - MySQL için Azure Veritabanı
description: MySQL için Azure Veritabanı'nda veri şifreleme sorununa nasıl sorun gidermeyi öğrenin
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 02/13/2020
ms.openlocfilehash: 42956d115590fd322d2851fd546c505a76a851fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79297049"
---
# <a name="troubleshoot-data-encryption-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'nda veri şifreleme sorunu

Bu makalede, müşteri tarafından yönetilen bir anahtar kullanılarak veri şifreleme ile yapılandırıldığında MySQL için Azure Veritabanı'nda oluşabilecek sık karşılaşılan sorunların nasıl tanımlanabileceği ve çözüleceği açıklanmaktadır.

## <a name="introduction"></a>Giriş

Veri şifrelemeyi Azure Key Vault'ta müşteri tarafından yönetilen bir anahtar kullanacak şekilde yapılandırdığınızda, sunucular anahtara sürekli erişim gerektirir. Sunucu Azure Key Vault'ta müşteri tarafından yönetilen anahtara erişimini kaybederse, tüm bağlantıları reddeder, uygun hata iletisini döndürecek ve durumunu Azure portalında ***erişilemez*** olarak değiştirir.

MySQL sunucusu için artık erişilemeyen bir Azure Veritabanına ihtiyacınız yoksa, maliyetlere neden olmak için veritabanını silebilirsiniz. Anahtar kasasına erişim geri yüklenene ve sunucu kullanılabilir olana kadar sunucudaki başka hiçbir eyleme izin verilmez. Müşteri tarafından yönetilen bir anahtarla şifrelendiğinde `Yes`erişilemeyen bir `No` sunucuda veri şifreleme seçeneğini (müşteri tarafından yönetilen) (hizmet yönetimi) arasında değiştirmek de mümkün değildir. Sunucuya yeniden erişilemeden önce anahtarı el ile yeniden doğrulamanız gerekir. Bu eylem, müşteri tarafından yönetilen anahtara izinler iptal edilirken verileri yetkisiz erişime karşı korumak için gereklidir.

## <a name="common-errors-that-cause-the-server-to-become-inaccessible"></a>Sunucunun erişilememelerine neden olan sık karşılaşılan hatalar

Aşağıdaki yanlış yapılandırmalar, Azure Anahtar Kasası anahtarlarını kullanan veri şifrelemeyle ilgili sorunların çoğuna neden olur:

- Anahtar kasası kullanılamıyor veya yok:
  - Anahtar kasası kazara silinmiş.
  - Aralıklı ağ hatası, anahtar kasasının kullanılamamasına neden olur.

- Anahtar kasasına erişmek için izinlere sahip değilsiniz veya anahtar yok:
  - Anahtarın süresi doldu veya yanlışlıkla silindi veya devre dışı bırakıldı.
  - MySQL örneği için Azure Veritabanı'nın yönetilen kimliği yanlışlıkla silindi.
  - MySQL örneği için Azure Veritabanı'nın yönetilen kimliği, yeterli anahtar izinlerine sahiptir. Örneğin, izinler Al, Sargı ve Aç'ı içermez.
  - MySQL örneği için Azure Veritabanı'na yönelik yönetilen kimlik izinleri iptal edildi veya silindi.

## <a name="identify-and-resolve-common-errors"></a>Sık karşılaşılan hataları belirleme ve çözme

### <a name="errors-on-the-key-vault"></a>Anahtar kasasındaki hatalar

#### <a name="disabled-key-vault"></a>Devre dışı anahtar kasası

- `AzureKeyVaultKeyDisabledMessage`
- **Açıklama**: Azure Anahtar Kasası anahtarı devre dışı bırakıldığından işlem sunucuda tamamlanamadı.

#### <a name="missing-key-vault-permissions"></a>Eksik anahtar kasası izinleri

- `AzureKeyVaultMissingPermissionsMessage`
- **Açıklama**: Sunucuda Azure Anahtar Kasası için gerekli Get, Wrap ve Unwrap izinleri yoktur. Eksik izinleri kimlikle servis müdürüne verir.

### <a name="mitigation"></a>Risk azaltma

- Müşteri tarafından yönetilen anahtarın anahtar kasasında bulunduğunu doğrulayın.
- Anahtar kasasını belirleyin ve Azure portalındaki anahtar kasasına gidin.
- URI anahtarının mevcut bir anahtarı tanımladığından emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[MySQL için Azure Veritabanı'nda müşteri tarafından yönetilen bir anahtarla veri şifrelemeayarlamak için Azure portalını kullanın](howto-data-encryption-portal.md)
