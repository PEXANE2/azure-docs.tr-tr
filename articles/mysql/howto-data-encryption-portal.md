---
title: Azure portal kullanarak MySQL için Azure veritabanı için veri şifreleme
description: Azure portal kullanarak MySQL için Azure veritabanı için veri şifrelemeyi ayarlamayı ve yönetmeyi öğrenin.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 01/13/2020
ms.openlocfilehash: 55c155dc4396672c02322c6c5727dac57d0ac8ef
ms.sourcegitcommit: 668b3480cb637c53534642adcee95d687578769a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2020
ms.locfileid: "78898725"
---
# <a name="data-encryption-for-azure-database-for-mysql-by-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı için veri şifreleme

MySQL için Azure veritabanınız için veri şifrelemeyi ayarlamak ve yönetmek üzere Azure portal nasıl kullanacağınızı öğrenin.

## <a name="prerequisites-for-azure-cli"></a>Azure CLı önkoşulları

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.
* Azure Key Vault ' de, bir anahtar kasası ve müşterinin yönettiği anahtar için kullanılacak bir anahtar oluşturun.
* Anahtar Kasası, müşteri tarafından yönetilen anahtar olarak kullanmak için aşağıdaki özelliklere sahip olmalıdır:
  * [Geçici silme](../key-vault/key-vault-ovw-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Korumalı Temizleme](../key-vault/key-vault-ovw-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Anahtar, müşteri tarafından yönetilen anahtar olarak kullanmak için aşağıdaki özniteliklere sahip olmalıdır:
  * Sona erme tarihi yok
  * Devre dışı değil
  * Al, sarmalama tuşu, anahtar sarmalama işlemini geri alabilir

## <a name="set-the-right-permissions-for-key-operations"></a>Anahtar işlemleri için doğru izinleri ayarla

1. Key Vault, erişim **Ilkesi ekle** > erişim **ilkeleri** ' ni seçin.

   ![Erişim ilkeleriyle Key Vault ekran görüntüsü ve erişim Ilkesi vurgulandı](media/concepts-data-access-and-security-data-encryption/show-access-policy-overview.png)

2. **Anahtar izinleri**' ni seçin ve MySQL sunucusunun adı olan Al, **sarmalama**, **geri** **Al**ve **asıl**' ı seçin. Sunucu sorumlunuz mevcut sorumlular listesinde bulunamazsa, kaydolmanız gerekir. Veri şifrelemeyi ilk kez ayarlamaya çalıştığınızda sunucu sorumlunuzu kaydetmeniz istenir ve başarısız olur.

   ![Erişim ilkesine genel bakış](media/concepts-data-access-and-security-data-encryption/access-policy-wrap-unwrap.png)

3. **Kaydet**’i seçin.

## <a name="set-data-encryption-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için veri şifrelemeyi ayarlama

1. MySQL için Azure veritabanı 'nda, müşteri tarafından yönetilen anahtarı ayarlamak için **veri şifreleme** ' yi seçin.

   ![Veri şifrelemesi vurgulanmış şekilde MySQL için Azure veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/data-encryption-overview.png)

2. Bir anahtar kasası ve anahtar çifti seçebilir ya da bir anahtar tanımlayıcı girebilirsiniz.

   ![Veri şifreleme seçenekleri vurgulanmış olan MySQL için Azure veritabanı 'nın ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/setting-data-encryption.png)

3. **Kaydet**’i seçin.

4. Tüm dosyaların (geçici dosyalar dahil) tamamen şifrelendiğinden emin olmak için sunucuyu yeniden başlatın.

## <a name="restore-or-create-a-replica-of-the-server"></a>Sunucu çoğaltmasını geri yükleme veya oluşturma

MySQL için Azure veritabanı, Key Vault ' de depolanan bir müşterinin yönetilen anahtarıyla şifrelendikten sonra, sunucunun yeni oluşturulan kopyası da şifrelenir. Bu yeni kopyayı yerel veya coğrafi geri yükleme işlemi aracılığıyla ya da bir çoğaltma (yerel/bölge) işlemi aracılığıyla yapabilirsiniz. Bu nedenle, şifrelenmiş bir MySQL sunucusu için, şifrelenmiş bir geri yüklenmiş sunucu oluşturmak için aşağıdaki adımları kullanabilirsiniz.

1. Sunucunuzda **genel bakış** > **geri yükleme**' yi seçin.

   ![MySQL için Azure veritabanı 'Na genel bakış ve geri yükleme vurgulanmış ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/show-restore.png)

   Ya da çoğaltma özellikli bir sunucu için, **Ayarlar** başlığı altında **çoğaltma**' yı seçin.

   ![Çoğaltma vurgulanmış şekilde, MySQL için Azure veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/mysql-replica.png)

2. Geri yükleme işlemi tamamlandıktan sonra oluşturulan yeni sunucu birincil sunucunun anahtarıyla şifrelenir. Ancak, sunucudaki Özellikler ve seçenekler devre dışı bırakılır ve sunucuya erişilemez. Bu, tüm veri düzenlemesini engeller, çünkü yeni sunucu kimliği henüz anahtar kasasına erişmek için izin verilmemiş.

   ![Erişilemeyen durum vurgulanacak MySQL için Azure veritabanı ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/show-restore-data-encryption.png)

3. Sunucuyu erişilebilir hale getirmek için geri yüklenen sunucuda anahtarı yeniden doğrulayın. **Anahtarı yeniden doğrulamak** > **veri şifrelemeyi** seçin.

   > [!NOTE]
   > Yeni sunucunun hizmet sorumlusunun anahtar kasasına erişim izni verilmesi gerektiğinden, ilk yeniden doğrulama denemesi başarısız olur. Hizmet sorumlusunu oluşturmak için **anahtarı yeniden doğrula**' yı seçin, bu, bir hatayı gösterir, ancak hizmet sorumlusu oluşturur. Bundan sonra bu makalede daha önce bahsedilen [adımlara](#set-the-right-permissions-for-key-operations) bakın.

   ![Yeniden doğrulama adımıyla MySQL için Azure veritabanı 'nın ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/show-revalidate-data-encryption.png)

   Yeni sunucuya Anahtar Kasası erişimi sağlamanız gerekir.

4. Hizmet sorumlusu kaydedildikten sonra anahtarı yeniden doğruladıktan sonra sunucu normal işlevselliğini sürdürür.

   ![MySQL için Azure veritabanı 'nın, geri yüklenen işlevselliği gösteren ekran görüntüsü](media/concepts-data-access-and-security-data-encryption/restore-successful.png)


## <a name="using-an-azure-resource-manager-template-to-enable-data-encryption"></a>Veri şifrelemeyi etkinleştirmek için Azure Resource Manager şablonu kullanma

Azure portal dışında, yeni ve mevcut sunucular için Azure Resource Manager şablonlarını kullanarak MySQL için Azure veritabanı sunucusunda veri şifrelemeyi de etkinleştirebilirsiniz.

### <a name="for-a-new-server"></a>Yeni bir sunucu için

Sunucuda veri şifrelemesi etkinken sunucu sağlamak için önceden oluşturulmuş Azure Resource Manager şablonlarından birini kullanın: [veri şifreleme Ile örnek](https://github.com/Azure/azure-mysql/tree/master/arm-templates/ExampleWithDataEncryption)

Bu Azure Resource Manager şablonu, MySQL için Azure veritabanı sunucusu oluşturur ve sunucuda veri şifrelemeyi etkinleştirmek için **anahtar** **Kasası** ve parametre olarak geçirilen anahtarı kullanır.

### <a name="for-an-existing-server"></a>Var olan bir sunucu için
Ayrıca, mevcut MySQL için Azure veritabanı sunucularınızda veri şifrelemeyi etkinleştirmek üzere Azure Resource Manager şablonları kullanabilirsiniz.

* Daha önce kopyaladığınız Azure Key Vault anahtarının URI 'sini Properties nesnesine `keyVaultKeyUri` özelliğinin altında geçirin.

* API sürümü olarak *2020-01-01-Preview* kullanın.

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

 Veri şifreleme hakkında daha fazla bilgi edinmek için bkz. [müşteri tarafından yönetilen anahtarla MySQL Için Azure veritabanı veri şifrelemesi](concepts-data-encryption-mysql.md).
