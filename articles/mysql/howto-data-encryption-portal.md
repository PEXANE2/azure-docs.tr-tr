---
title: Veri şifreleme - Azure portalı - MySQL için Azure Veritabanı
description: Azure portalını kullanarak MySQL için Azure Veritabanınız için veri şifrelemeyi nasıl ayarlayıp yöneteceklerinizi öğrenin.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 78a290b1e2984719645fb4d4ff253ab021a0826e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79299048"
---
# <a name="data-encryption-for-azure-database-for-mysql-by-using-the-azure-portal"></a>Azure portalını kullanarak MySQL için Azure Veritabanı için veri şifreleme

MySQL için Azure Veritabanınız için veri şifrelemesini ayarlamak ve yönetmek için Azure portalını nasıl kullanacağınızı öğrenin.

## <a name="prerequisites-for-azure-cli"></a>Azure CLI için ön koşullar

* Azure aboneliğiniz olmalı ve bu abonelikte yönetici olmalısınız.
* Azure Key Vault'ta, müşteri tarafından yönetilen bir anahtar için bir anahtar kasası ve kullanılacak bir anahtar oluşturun.
* Anahtar kasası, müşteri tarafından yönetilen anahtar olarak kullanılacak aşağıdaki özelliklere sahip olmalıdır:
  * [Yumuşak silme](../key-vault/key-vault-ovw-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Temizleme korumalı](../key-vault/key-vault-ovw-soft-delete.md#purge-protection)

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

3. **Kaydet'i**seçin.

## <a name="set-data-encryption-for-azure-database-for-mysql"></a>MySQL için Azure Veritabanı için veri şifrelemesi ayarlama

1. MySQL için Azure Veritabanı'nda, müşteri tarafından yönetilen anahtarı ayarlamak için **Veri şifrelemesini** seçin.

   ![Veri şifrelemesi vurgulanmış MySQL için Azure Veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Bir anahtar tonoz ve anahtar çifti seçebilir veya bir anahtar tanımlayıcısı girebilirsiniz.

   ![Veri şifreleme seçenekleri vurgulanmış MySQL için Azure Veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. **Kaydet'i**seçin.

4. Tüm dosyaların (geçici dosyalar dahil) tamamen şifrelenmiş olduğundan emin olmak için sunucuyu yeniden başlatın.

## <a name="restore-or-create-a-replica-of-the-server"></a>Sunucunun bir kopyasını geri yükleme veya oluşturma

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


## <a name="using-an-azure-resource-manager-template-to-enable-data-encryption"></a>Veri şifrelemesini etkinleştirmek için Azure Kaynak Yöneticisi şablonu kullanma

Azure portalıdışında, yeni ve mevcut sunucular için Azure Kaynak Yöneticisi şablonlarını kullanarak MySQL sunucusu için Azure Veritabanınızda veri şifrelemesini de etkinleştirebilirsiniz.

### <a name="for-a-new-server"></a>Yeni bir sunucu için

Sunucuya veri şifreleme etkinliği sağlamak için önceden oluşturulmuş Azure Kaynak Yöneticisi şablonlarından birini kullanın: [Veri şifrelemesi ile örnek](https://github.com/Azure/azure-mysql/tree/master/arm-templates/ExampleWithDataEncryption)

Bu Azure Kaynak Yöneticisi şablonu MySQL sunucusu için bir Azure Veritabanı oluşturur ve sunucuda veri şifrelemesini etkinleştirmek için anahtar **atlama** ve **anahtar** parametreleri olarak geçirilir.

### <a name="for-an-existing-server"></a>Varolan bir sunucu için
Ayrıca, MySQL sunucuları için mevcut Azure Veritabanınızda veri şifrelemesini etkinleştirmek için Azure Kaynak Yöneticisi şablonlarını kullanabilirsiniz.

* Özellikler nesnesindeki özelliğin altında daha önce `keyVaultKeyUri` kopyaladığınız Azure Anahtar Kasası anahtarının URI'sini geçirin.

* API sürümü olarak *2020-01-01 önizleme* kullanın.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string"
    },
    "serverName": {
      "type": "string"
    },
    "keyVaultName": {
      "type": "string",
      "metadata": {
        "description": "Key vault name where the key to use is stored"
      }
    },
    "keyVaultResourceGroupName": {
      "type": "string",
      "metadata": {
        "description": "Key vault resource group name where it is stored"
      }
    },
    "keyName": {
      "type": "string",
      "metadata": {
        "description": "Key name in the key vault to use as encryption protector"
      }
    },
    "keyVersion": {
      "type": "string",
      "metadata": {
        "description": "Version of the key in the key vault to use as encryption protector"
      }
    }
  },
  "variables": {
    "serverKeyName": "[concat(parameters('keyVaultName'), '_', parameters('keyName'), '_', parameters('keyVersion'))]"
  },
  "resources": [
    {
      "type": "Microsoft.DBforMySQL/servers",
      "apiVersion": "2017-12-01",
      "kind": "",
      "location": "[parameters('location')]",
      "identity": {
        "type": "SystemAssigned"
      },
      "name": "[parameters('serverName')]",
      "properties": {
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-05-01",
      "name": "addAccessPolicy",
      "resourceGroup": "[parameters('keyVaultResourceGroupName')]",
      "dependsOn": [
        "[resourceId('Microsoft.DBforMySQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.KeyVault/vaults/accessPolicies",
              "name": "[concat(parameters('keyVaultName'), '/add')]",
              "apiVersion": "2018-02-14-preview",
              "properties": {
                "accessPolicies": [
                  {
                    "tenantId": "[subscription().tenantId]",
                    "objectId": "[reference(resourceId('Microsoft.DBforMySQL/servers/', parameters('serverName')), '2017-12-01', 'Full').identity.principalId]",
                    "permissions": {
                      "keys": [
                        "get",
                        "wrapKey",
                        "unwrapKey"
                      ]
                    }
                  }
                ]
              }
            }
          ]
        }
      }
    },
    {
      "name": "[concat(parameters('serverName'), '/', variables('serverKeyName'))]",
      "type": "Microsoft.DBforMySQL/servers/keys",
      "apiVersion": "2020-01-01-preview",
      "dependsOn": [
        "addAccessPolicy",
        "[resourceId('Microsoft.DBforMySQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "serverKeyType": "AzureKeyVault",
        "uri": "[concat(reference(resourceId(parameters('keyVaultResourceGroupName'), 'Microsoft.KeyVault/vaults/', parameters('keyVaultName')), '2018-02-14-preview', 'Full').properties.vaultUri, 'keys/', parameters('keyName'), '/', parameters('keyVersion'))]"
      }
    }
  ]
}

```

## <a name="next-steps"></a>Sonraki adımlar

 Veri şifreleme hakkında daha fazla bilgi edinmek [için müşteri tarafından yönetilen anahtarla MySQL veri şifrelemeiçin Azure Veritabanı'na](concepts-data-encryption-mysql.md)bakın.
