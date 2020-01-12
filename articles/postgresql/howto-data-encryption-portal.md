---
title: Portalı kullanarak PostgreSQL için Azure veritabanı için veri şifreleme tek sunucu
description: Azure portal kullanarak PostgreSQL için Azure veritabanı için veri şifrelemeyi ayarlama ve yönetme hakkında bilgi edinin
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/10/2020
ms.openlocfilehash: 51c99ca0788900397c922e31e44f121a7ae9caa6
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75904269"
---
# <a name="data-encryption-for-azure-database-for-postgresql-single-server-using-portal"></a>Portalı kullanarak PostgreSQL için Azure veritabanı için veri şifreleme tek sunucu

Bu makalede, PostgreSQL için Azure veritabanı tek sunucu için veri şifrelemeyi ayarlamak üzere Azure portal kullanmak üzere ayarlama ve yönetme hakkında bilgi edineceksiniz.

## <a name="prerequisites-for-powershell"></a>PowerShell önkoşulları

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.
* Azure PowerShell yüklü ve çalışıyor olmanız gerekir.
* Müşteri tarafından yönetilen anahtar için kullanmak üzere bir Azure Key Vault ve anahtar oluşturun.
* Anahtar Kasası, müşteri tarafından yönetilen anahtar olarak kullanmak için şu özelliğe sahip olmalıdır
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

2. **Anahtar izinleri** altında, PostgreSQL sunucusunun adı olan **Al**, **sarmalama**, **Unwrap** ve **sorumluyu** seçin.

   ![Erişim ilkesine genel bakış](media/concepts-data-access-and-security-data-encryption/access-policy-warp-unwrap.png)

3. Ayarları **kaydedin** .

## <a name="setting-data-encryption-for-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure veritabanı için veri şifrelemeyi ayarlama tek sunucu

1. **PostgreSQL Için Azure veritabanı**'nda, müşteri tarafından yönetilen anahtar kurulumunu ayarlamak Için **veri şifrelemeyi** seçin.

   ![Veri şifrelemeyi ayarlama](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Bir **Key Vault** ve **anahtar** çifti seçebilir ya da **anahtar tanımlayıcısını**geçirebilirsiniz.

   ![Key Vault ayarlama](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. Ayarları **kaydedin** .

4. Tüm dosyaların (geçici dosyalar dahil) tam şifrelendiğinden emin olmak için sunucu yeniden başlatması gerekir.

## <a name="restoring-or-creating-replica-of-the-server-which-has-data-encryption-enabled"></a>Veri şifrelemesi etkin olan sunucunun çoğaltmasını geri yükleme veya oluşturma

PostgreSQL için Azure veritabanı tek sunucu, Key Vault depolanan müşterinin yönetilen anahtarıyla, yerel veya coğrafi geri yükleme işlemi ya da bir çoğaltma (yerel/çapraz bölge) işlemi gibi, sunucunun yeni oluşturulmuş bir kopyası ile şifrelendikten sonra. Bu nedenle, şifrelenmiş bir PostgreSQL sunucusu için, şifrelenmiş geri yüklenmiş bir sunucu oluşturmak üzere aşağıdaki adımları izleyebilirsiniz.

1. Sunucunuzda **genel bakış**' ı seçin ve ardından **geri yükle**' yi seçin.

   ![Başlat-geri yükle](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Ya da bir çoğaltma etkin sunucu için, **Ayarlar** başlığı altında **çoğaltma** ' yı seçin.

   ![Başlatma-çoğaltma](media/concepts-data-access-and-security-data-encryption/postgresql-replica.png)

2. Geri yükleme işlemi tamamlandıktan sonra oluşturulan yeni sunucu, birincil sunucunun anahtarıyla şifrelenmiş veriler olur. Ancak, sunucudaki Özellikler ve seçenekler devre dışı bırakılır ve sunucu **erişilemeyen** bir durumda işaretlenir. Bu, yeni sunucunun kimliğine Key Vault erişim izni verilmediğinden bu yana yapılan tüm veri düzenlemesini önlemektir.

   ![Sunucuyu erişilemez olarak işaretle](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)


3. Erişilemeyen durumu onarmak için, geri yüklenen sunucuda anahtarı yeniden doğrulamanız gerekir.

   ![sunucuyu yeniden doğrula](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Key Vault yeni sunucuya erişim sağlamanız gerekir. 

4. Anahtarı yeniden doğruladıktan sonra sunucu normal işlevini sürdürür.

   ![Normal sunucu geri yüklendi](media/concepts-data-access-and-security-data-encryption/restore-successful.png)


## <a name="next-steps"></a>Sonraki adımlar

 Veri şifreleme hakkında daha fazla bilgi için bkz. [Azure veri şifrelemesi nedir?](concepts-data-encryption-postgresql.md).