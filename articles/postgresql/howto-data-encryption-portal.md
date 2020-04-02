---
title: Veri şifreleme - Azure portalı - PostgreSQL için Azure Veritabanı için - Tek sunucu
description: Azure portalını kullanarak PostgreSQL Single sunucusu için Azure Veritabanınız için veri şifrelemeyi nasıl ayarlayıp yöneteceklerinizi öğrenin.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 847e3c612a200743fa08cf939c9995ebb6f3dbfc
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80520341"
---
# <a name="data-encryption-for-azure-database-for-postgresql-single-server-by-using-the-azure-portal"></a>Azure portalını kullanarak PostgreSQL Tek sunucu için Azure Veritabanı için veri şifreleme

PostgreSQL Tek sunucu için Azure Veritabanınız için veri şifrelemesini ayarlamak ve yönetmek için Azure portalını nasıl kullanacağınızı öğrenin.

## <a name="prerequisites-for-azure-cli"></a>Azure CLI için ön koşullar

* Azure aboneliğiniz olmalı ve bu abonelikte yönetici olmalısınız.
* Azure Key Vault'ta, müşteri tarafından yönetilen bir anahtar için kullanılacak bir anahtar kasası ve anahtar oluşturun.
* Anahtar kasası, müşteri tarafından yönetilen anahtar olarak kullanılacak aşağıdaki özelliklere sahip olmalıdır:
  * [Yumuşak silme](../key-vault/key-vault-ovw-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -test -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Temizleme korumalı](../key-vault/key-vault-ovw-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Anahtar, müşteri tarafından yönetilen bir anahtar olarak kullanılacak aşağıdaki özniteliklere sahip olmalıdır:
  * Son kullanma tarihi yok
  * Devre dışı bırakılmadı
  * Anahtarı al, sarve anahtar işlemlerini gerçekleştirebilme ve açma

## <a name="set-the-right-permissions-for-key-operations"></a>Önemli işlemler için doğru izinleri ayarlama

1. Key Vault'ta **Erişim İlkeleri** > **Ekle'yi**seçin.

   ![Erişim ilkeleri ve Access Ekle İlkesi vurgulanan Key Vault ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. **Anahtar izinlerini**seçin ve PostgreSQL sunucusunun adı olan **Al**, **Sarg,** **Aç**ve **Asıl'** u seçin. Sunucu müdürünüz varolan ilkeler listesinde bulunamazsa, bunu kaydetmeniz gerekir. Veri şifrelemesini ilk kez ayarlamaya çalıştığınızda sunucu anaparanızı kaydetmeniz istenir ve bu başarısız olur.  

   ![Erişim ilkesine genel bakış](media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png)

3. **Kaydet'i**seçin.

## <a name="set-data-encryption-for-azure-database-for-postgresql-single-server"></a>PostgreSQL Tek sunucu için Azure Veritabanı için veri şifrelemesi ayarlama

1. PostgreSQL için Azure Veritabanı'nda, müşteri tarafından yönetilen anahtarı ayarlamak için **Veri şifrelemesini** seçin.

   ![Veri şifrelemesi vurgulanmış PostgreSQL için Azure Veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Bir anahtar tonoz ve anahtar çifti seçebilir veya bir anahtar tanımlayıcısı girebilirsiniz.

   ![Veri şifreleme seçenekleri vurgulanan PostgreSQL için Azure Veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. **Kaydet'i**seçin.

4. Tüm dosyaların (geçici dosyalar dahil) tamamen şifrelenmiş olduğundan emin olmak için sunucuyu yeniden başlatın.

## <a name="using-data-encryption-for-restore-or-replica-servers"></a>Geri yükleme veya çoğaltma sunucuları için Veri şifrelemesi kullanma

PostgreSQL için Azure Veritabanı Tek sunucu, müşterinin yönetilen anahtarıyla Key Vault'ta depolandıktan sonra, sunucunun yeni oluşturulan herhangi bir kopyası da şifrelenir. Bu yeni kopyayı yerel veya coğrafi geri yükleme işlemi veya yineleme (yerel/çapraz bölge) işlemi aracılığıyla yapabilirsiniz. Yani, şifreli bir PostgreSQL sunucusu için, şifreli bir geri yüklenen sunucu oluşturmak için aşağıdaki adımları kullanabilirsiniz.

1. Sunucunuzda **Genel Bakış** > **Geri Yükleme'yi**seçin.

   ![PostgreSQL için Azure Veritabanı'nın ekran görüntüsü, Genel Bakış ve Geri Yükleme vurgulanmış](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Veya çoğaltma özellikli bir sunucu için, **Ayarlar** başlığı altında **Çoğaltma'yı**seçin.

   ![PostgreSQL için Azure Veritabanı ekran görüntüsü, Çoğaltma vurgulanır](media/concepts-data-access-and-security-data-encryption/postgresql-replica.png)

2. Geri yükleme işlemi tamamlandıktan sonra oluşturulan yeni sunucu birincil sunucunun anahtarıyla şifrelenir. Ancak, sunucudaki özellikler ve seçenekler devre dışı bırakılır ve sunucuya erişilemez. Yeni sunucunun kimliğine anahtar kasasına erişmek için henüz izin verilmediği için bu durum herhangi bir veri işlemesini önler.

   ![PostgreSQL için Azure Veritabanı ekran görüntüsü, erişilemez durum vurgulanır](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Sunucunun erişilebilir olmasını sağlamak için, geri yüklenen sunucudaki anahtarı yeniden geçersiz kılın. **Veri Şifreleme** > **yeniden doğrula anahtarını**seçin.

   > [!NOTE]
   > Yeni sunucunun hizmet sorumlusunun anahtar kasasına erişmesi gerektiğinden, ilk yeniden doğrulama denemesi başarısız olur. Hizmet ilkesini oluşturmak için, hata gösterecek ancak hizmet ilkesini oluşturan **Yeniden Doğrula tuşu'nu**seçin. Bundan sonra, bu makalede daha önce [bu adımlara](#set-the-right-permissions-for-key-operations) bakın.

   ![PostgreSQL için Azure Veritabanı ekran görüntüsü, yeniden doğrulama adımı vurgulanır](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Yeni sunucuya anahtar kasa erişim vermek zorunda kalacak.

4. Hizmet ilkesini kaydettikten sonra anahtarı yeniden geçersiz kılın ve sunucu normal işlevini devam ettirin.

   ![PostgreSQL için Azure Veritabanı'nın ekran görüntüsü, geri yüklenen işlevselliği gösteriyor](media/concepts-data-access-and-security-data-encryption/restore-successful.png)

## <a name="next-steps"></a>Sonraki adımlar

 Veri şifreleme hakkında daha fazla bilgi edinmek [için, müşteri tarafından yönetilen anahtara sahip PostgreSQL Tek sunucu veri şifrelemeiçin Azure Veritabanı'na](concepts-data-encryption-postgresql.md)bakın.
