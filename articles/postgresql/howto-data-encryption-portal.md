---
title: Portalı kullanarak PostgreSQL için Azure veritabanı için veri şifreleme tek sunucu
description: Azure portal kullanarak PostgreSQL için Azure veritabanı için veri şifrelemeyi ayarlamayı ve yönetmeyi öğrenin.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 9c935ad8f77e2f8a6198a8ac095e0cc60c025a72
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76028627"
---
# <a name="data-encryption-for-azure-database-for-postgresql-single-server-using-portal"></a>Portalı kullanarak PostgreSQL için Azure veritabanı için veri şifreleme tek sunucu

Bu makalede, PostgreSQL için Azure veritabanı tek sunucu için veri şifrelemeyi ayarlamak üzere Azure portal kullanmak üzere ayarlama ve yönetme hakkında bilgi edineceksiniz.

## <a name="prerequisites-for-cli"></a>CLı önkoşulları

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.
* Müşteri tarafından yönetilen anahtar için kullanmak üzere bir Azure Key Vault ve anahtar oluşturun.
* Anahtar Kasası, müşteri tarafından yönetilen anahtar olarak kullanmak için aşağıdaki özelliğe sahip olmalıdır:
  * [Geçici Silme](../key-vault/key-vault-ovw-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -test -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Korumalı Temizleme](../key-vault/key-vault-ovw-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Anahtar, müşteri tarafından yönetilen anahtar için kullanılacak aşağıdaki özniteliklere sahip olmalıdır.
  * Sona erme tarihi yok
  * Devre dışı değil
  * _Al_, _sarmalama tuşu_ve _sarmalama anahtar_ işlemlerini gerçekleştirebiliyor

## <a name="setting-the-right-permissions-for-key-operations"></a>Anahtar işlemleri için doğru izinleri ayarlama

1. Azure Key Vault, **erişim ilkelerini**seçin ve **erişim ilkesi ekleyin**.

   ![Erişim ilkesine genel bakış](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. **Anahtar izinleri**altında, PostgreSQL sunucusunun adı olan Al, **sarmalama**, **geri** **Al**ve **asıl**' ı seçin. Sunucu sorumlunuz var olan sorumlular listesinde bulunamazsa, ilk kez veri şifrelemeyi ayarlamaya çalışırken kaydolmanız gerekir, bu da başarısız olur.  

   ![Erişim ilkesine genel bakış](media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png)

3. Ayarları **kaydedin** .

## <a name="setting-data-encryption-for-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure veritabanı için veri şifrelemeyi ayarlama tek sunucu

1. **PostgreSQL Için Azure veritabanı**'nda, müşteri tarafından yönetilen anahtar kurulumunu ayarlamak Için **veri şifrelemeyi** seçin.

   ![Veri şifrelemeyi ayarlama](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Bir **Key Vault** ve **anahtar** çifti seçebilir ya da **anahtar tanımlayıcısını**geçirebilirsiniz.

   ![Key Vault ayarlama](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. Ayarları **kaydedin** .

4. Tüm dosyaların ( **geçici dosyalar**dahil) tam şifrelendiğinden emin olmak için sunucu **yeniden başlatması** **gerekir**.

## <a name="restoring-or-creating-replica-of-the-server-which-has-data-encryption-enabled"></a>Veri şifrelemesi etkin olan sunucunun çoğaltmasını geri yükleme veya oluşturma

PostgreSQL için Azure veritabanı tek sunucu, Key Vault depolanan müşterinin yönetilen anahtarıyla, yerel veya coğrafi geri yükleme işlemi ya da bir çoğaltma (yerel/çapraz bölge) işlemi gibi, sunucunun yeni oluşturulmuş bir kopyası ile şifrelendikten sonra. Bu nedenle, şifrelenmiş bir PostgreSQL sunucusu için, şifrelenmiş geri yüklenmiş bir sunucu oluşturmak üzere aşağıdaki adımları izleyebilirsiniz.

1. Sunucunuzda **genel bakış**' ı seçin ve ardından **geri yükle**' yi seçin.

   ![Başlat-geri yükle](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Ya da çoğaltma özellikli bir sunucu için, **Ayarlar** başlığı altında aşağıda gösterildiği gibi **çoğaltma**' yı seçin:

   ![Başlatma-çoğaltma](media/concepts-data-access-and-security-data-encryption/postgresql-replica.png)

2. Geri yükleme işlemi tamamlandıktan sonra oluşturulan yeni sunucu, birincil sunucunun anahtarıyla şifrelenmiş veriler olur. Ancak, sunucudaki Özellikler ve seçenekler devre dışı bırakılır ve sunucu **erişilemeyen** bir durumda işaretlenir. Bu davranış, yeni sunucunun kimliğine Key Vault erişim izni verilmediğinden, herhangi bir veri işlemesini engellemek için tasarlanmıştır.

   ![Sunucuyu erişilemez olarak işaretle](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Erişilemeyen durumu onarmak için, geri yüklenen sunucuda anahtarı yeniden doğrulamanız gerekir. **Veri şifreleme** bölmesini ve sonra **anahtarı yeniden doğrula** düğmesini seçin.

   > [!NOTE]
   > Yeni sunucunun hizmet sorumlusunun anahtar kasasına erişim izni verilmesi gerektiğinden, ilk yeniden doğrulama denemesi başarısız olur. Hizmet sorumlusu oluşturmak için, bir hata veren ancak hizmet sorumlusu üreten **anahtarı yeniden doğrula**' yı seçin. Bundan sonra yukarıdaki 2. [bölümde bulunan](#setting-the-right-permissions-for-key-operations) adımlara bakın.

   ![sunucuyu yeniden doğrula](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Key Vault yeni sunucuya erişim sağlamanız gerekir.

4. Hizmet sorumlusu kaydedildikten sonra anahtarı yeniden doğrulamanız gerekir ve sunucu normal işlevselliğini sürdürür.

   ![Normal sunucu geri yüklendi](media/concepts-data-access-and-security-data-encryption/restore-successful.png)

## <a name="next-steps"></a>Sonraki adımlar

 Veri şifreleme hakkında daha fazla bilgi için bkz. [Azure veri şifrelemesi nedir?](concepts-data-encryption-postgresql.md).
