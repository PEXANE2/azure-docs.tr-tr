---
title: Veri şifreleme - Azure portalı - MySQL için Azure Veritabanı
description: Azure portalını kullanarak MySQL için Azure Veritabanınız için veri şifrelemeyi nasıl ayarlayıp yöneteceklerinizi öğrenin.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 9d1e89919647d9d94b287618da2f9a77278425a5
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81459092"
---
# <a name="data-encryption-for-azure-database-for-mysql-by-using-the-azure-portal"></a>Azure portalını kullanarak MySQL için Azure Veritabanı için veri şifreleme

MySQL için Azure Veritabanınız için veri şifrelemesini ayarlamak ve yönetmek için Azure portalını nasıl kullanacağınızı öğrenin.

## <a name="prerequisites-for-azure-cli"></a>Azure CLI için ön koşullar

* Azure aboneliğiniz olmalı ve bu abonelikte yönetici olmalısınız.
* Azure Key Vault'ta, müşteri tarafından yönetilen bir anahtar için bir anahtar kasası ve kullanılacak bir anahtar oluşturun.
* Anahtar kasası, müşteri tarafından yönetilen anahtar olarak kullanılacak aşağıdaki özelliklere sahip olmalıdır:
  * [Yumuşak silme](../key-vault/general/overview-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Temizleme korumalı](../key-vault/general/overview-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Anahtar, müşteri tarafından yönetilen bir anahtar olarak kullanılacak aşağıdaki özniteliklere sahip olmalıdır:
  * Son kullanma tarihi yok
  * Devre dışı bırakılmadı
  * Get, wrap tuşu, anahtar işlemlerini yapabilme

## <a name="set-the-right-permissions-for-key-operations"></a>Önemli işlemler için doğru izinleri ayarlama

1. Key Vault'ta **Erişim İlkeleri** > **Ekle'yi**seçin.

   ![Erişim ilkeleri ve Access Ekle İlkesi vurgulanan Key Vault ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. **Anahtar izinlerini**seçin ve MySQL sunucusunun adı olan Al **,** **Sarg,** **Aç**ve **Asıl'** u seçin. Sunucu müdürünüz varolan ilkeler listesinde bulunamazsa, bunu kaydetmeniz gerekir. Veri şifrelemesini ilk kez ayarlamaya çalıştığınızda sunucu anaparanızı kaydetmeniz istenir ve bu başarısız olur.

   ![Erişim ilkesine genel bakış](media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png)

3. **Kaydet**’i seçin.

## <a name="set-data-encryption-for-azure-database-for-mysql"></a>MySQL için Azure Veritabanı için veri şifrelemesi ayarlama

1. MySQL için Azure Veritabanı'nda, müşteri tarafından yönetilen anahtarı ayarlamak için **Veri şifrelemesini** seçin.

   ![Veri şifrelemesi vurgulanmış MySQL için Azure Veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Bir anahtar tonoz ve anahtar çifti seçebilir veya bir anahtar tanımlayıcısı girebilirsiniz.

   ![Veri şifreleme seçenekleri vurgulanmış MySQL için Azure Veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. **Kaydet**’i seçin.

4. Tüm dosyaların (geçici dosyalar dahil) tamamen şifrelenmiş olduğundan emin olmak için sunucuyu yeniden başlatın.

## <a name="using-data-encryption-for-restore-or-replica-servers"></a>Geri yükleme veya çoğaltma sunucuları için Veri şifrelemesi kullanma

MySQL için Azure Veritabanı, Key Vault'ta saklanan bir müşterinin yönetilen anahtarıyla şifrelendikten sonra, sunucunun yeni oluşturulan herhangi bir kopyası da şifrelenir. Bu yeni kopyayı yerel veya coğrafi geri yükleme işlemi veya yineleme (yerel/çapraz bölge) işlemi aracılığıyla yapabilirsiniz. Yani, şifreli bir MySQL sunucusu için, şifreli bir geri yüklenen sunucu oluşturmak için aşağıdaki adımları kullanabilirsiniz.

1. Sunucunuzda **Genel Bakış** > **Geri Yükleme'yi**seçin.

   ![MySQL için Azure Veritabanı'nın ekran görüntüsü, Genel Bakış ve Geri Yükleme vurgulanmış](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Veya çoğaltma özellikli bir sunucu için, **Ayarlar** başlığı altında **Çoğaltma'yı**seçin.

   ![Çoğaltma vurgulanmış MySQL için Azure Veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/mysql-replica.png)

2. Geri yükleme işlemi tamamlandıktan sonra oluşturulan yeni sunucu birincil sunucunun anahtarıyla şifrelenir. Ancak, sunucudaki özellikler ve seçenekler devre dışı bırakılır ve sunucuya erişilemez. Yeni sunucunun kimliğine anahtar kasasına erişmek için henüz izin verilmediği için bu durum herhangi bir veri işlemesini önler.

   ![MySQL için Azure Veritabanı ekran görüntüsü, erişilemeengelli durum vurgulanır](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Sunucunun erişilebilir olmasını sağlamak için, geri yüklenen sunucudaki anahtarı yeniden geçersiz kılın. **Veri şifrelemesi** > **yeniden doğrula'yı**seçin.

   > [!NOTE]
   > Yeni sunucunun hizmet sorumlusunun anahtar kasasına erişmesi gerektiğinden, ilk yeniden doğrulama denemesi başarısız olur. Hizmet ilkesini oluşturmak için, hata gösterecek ancak hizmet ilkesini oluşturan **Yeniden Doğrula tuşu'nu**seçin. Bundan sonra, bu makalede daha önce [bu adımlara](#set-the-right-permissions-for-key-operations) bakın.

   ![MySQL için Azure Veritabanı ekran görüntüsü, yeniden doğrulama adımı vurgulanır](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Yeni sunucuya anahtar kasa erişim vermek zorunda kalacak.

4. Hizmet ilkesini kaydettikten sonra anahtarı yeniden geçersiz kılın ve sunucu normal işlevini devam ettirin.

   ![MySQL için Azure Veritabanı ekran görüntüsü, geri yüklenen işlevselliği gösteren](media/concepts-data-access-and-security-data-encryption/restore-successful.png)

## <a name="next-steps"></a>Sonraki adımlar

 Veri şifreleme hakkında daha fazla bilgi edinmek [için müşteri tarafından yönetilen anahtarla MySQL veri şifrelemeiçin Azure Veritabanı'na](concepts-data-encryption-mysql.md)bakın.
