---
title: Portal kullanarak MySQL için Azure veritabanı için veri şifreleme
description: Azure portal kullanarak MySQL için Azure veritabanı için veri şifrelemeyi ayarlama ve yönetme hakkında bilgi edinin
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/10/2020
ms.openlocfilehash: 10af869a631b620c2c75aa69722dc03df15f8539
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903853"
---
# <a name="data-encryption-for-azure-database-for-mysql-server-using-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı sunucusu için veri şifreleme

Bu makalede, MySQL için Azure veritabanı için veri şifrelemeyi kurmak üzere Azure portal kullanmak üzere nasıl ayarlanacağını ve yönetileceğini öğreneceksiniz.

## <a name="prerequisites-for-powershell"></a>PowerShell önkoşulları

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.
* Azure PowerShell yüklü ve çalışıyor olmanız gerekir.
* Müşteri tarafından yönetilen anahtar için kullanmak üzere bir Azure Key Vault ve anahtar oluşturun.
* Key Vault, müşteri tarafından yönetilen anahtar olarak kullanmak için aşağıdaki özelliğe sahip olmalıdır
    * [Geçici Silme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)

        ```azurecli-interactive
        az resource update --id $(az keyvault show --name \ <key_valut_name> -test -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
        ```
    
    * [Korumalı Temizleme](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete#purge-protection)

        ```azurecli-interactive
        az keyvault update --name <key_valut_name> --resource-group <resource_group_name>  --enable-purge-protection true
        ```
* Anahtar, müşteri tarafından yönetilen anahtar için kullanılacak aşağıdaki özniteliklere sahip olmalıdır.
    * Sona erme tarihi yok
    * Devre dışı değil
    * Al, sarmalama tuşu, anahtar sarmalama işlemini geri alabilir

## <a name="setting-the-right-permissions-for-key-operations"></a>Anahtar işlemleri için doğru izinleri ayarlama

1. Azure Key Vault, **erişim ilkelerini** seçin ve **erişim ilkesi ekleyin** 

   ![Erişim ilkesine genel bakış](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. **Anahtar izinlerini** seçin **Al**, **sarmalama**, **Unwrap** ve MySQL sunucusunun adı olan **sorumluyu** seçin.

   ![Erişim ilkesine genel bakış](media/concepts-data-access-and-security-data-encryption/access-policy-warp-unwrap.png)

3. Ayarları **kaydedin** .

## <a name="setting-data-encryption-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için veri şifrelemeyi ayarlama

1. **MySQL Için Azure veritabanı**'nda, müşteri tarafından yönetilen anahtar kurulumunu ayarlamak Için **veri şifrelemeyi** seçin.

   ![Veri şifrelemeyi ayarlama](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Bir **Key Vault** ve **anahtar** çifti seçebilir ya da **anahtar tanımlayıcısını**geçirebilirsiniz.

   ![Key Vault ayarlama](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. Ayarları **kaydedin** .

4. Tüm dosyaların (geçici dosyalar dahil) tam şifrelendiğinden emin olmak için sunucu yeniden başlatması gerekir.

## <a name="restoring-or-creating-replica-of-the-server-which-has-data-encryption-enabled"></a>Veri şifrelemesi etkin olan sunucunun çoğaltmasını geri yükleme veya oluşturma

MySQL için Azure veritabanı, Key Vault depolanan müşterinin yönetilen anahtarı ile şifrelendikten sonra, sunucunun yerel veya coğrafi geri yükleme işlemi ya da bir çoğaltma (yerel/çapraz bölge) işlemi gibi yeni oluşturulan bir kopyası. Bu nedenle, şifrelenmiş bir MySQL sunucusu için, şifrelenmiş bir geri yüklenmiş sunucu oluşturmak üzere aşağıdaki adımları izleyebilirsiniz.

1. Sunucunuzda **genel bakış**' ı seçin ve ardından **geri yükle**' yi seçin.

   ![Başlat-geri yükle](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Ya da bir çoğaltma etkin sunucu için, **Ayarlar** başlığı altında **çoğaltma** ' yı seçin.

   ![Başlatma-çoğaltma](media/concepts-data-access-and-security-data-encryption/mysql-replica.png)

2. Geri yükleme işlemi tamamlandıktan sonra oluşturulan yeni sunucu, birincil sunucuyu şifrelemek için kullanılan anahtarla şifrelenir. Ancak, sunucudaki Özellikler ve seçenekler devre dışı bırakılır ve sunucu **erişilemeyen** bir durumda işaretlenir. Bu, yeni sunucunun kimliğine Key Vault erişim izni verilmediğinden bu yana yapılan tüm veri düzenlemesini önlemektir.

   ![Sunucuyu erişilemez olarak işaretle](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Erişilemeyen durumu onarmak için, geri yüklenen sunucuda anahtarı yeniden doğrulamanız gerekir.

   ![sunucuyu yeniden doğrula](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Key Vault yeni sunucuya erişim sağlamanız gerekir. 

4. Anahtarı yeniden doğruladıktan sonra sunucu normal işlevini sürdürür.

   ![Normal sunucu geri yüklendi](media/concepts-data-access-and-security-data-encryption/restore-successful.png)


## <a name="next-steps"></a>Sonraki adımlar

 Veri şifreleme hakkında daha fazla bilgi için bkz. [Azure veri şifrelemesi nedir?](concepts-data-encryption-mysql.md).

