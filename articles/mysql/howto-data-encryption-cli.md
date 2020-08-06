---
title: Veri şifreleme-Azure CLı-MySQL için Azure veritabanı
description: Azure CLı kullanarak MySQL için Azure veritabanı için veri şifrelemeyi ayarlamayı ve yönetmeyi öğrenin.
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: how-to
ms.date: 03/30/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: eb83cd4fe7e98b1cde6dcee5d3f25fa5e35f1d2c
ms.sourcegitcommit: fbb66a827e67440b9d05049decfb434257e56d2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87799828"
---
# <a name="data-encryption-for-azure-database-for-mysql-by-using-the-azure-cli"></a>Azure CLı kullanarak MySQL için Azure veritabanı için veri şifreleme

Azure CLı kullanarak MySQL için Azure veritabanı için veri şifrelemeyi ayarlama ve yönetme hakkında bilgi edinin.

## <a name="prerequisites-for-azure-cli"></a>Azure CLı önkoşulları

* Bu abonelikte bir Azure aboneliğiniz olması ve bir yönetici olmanız gerekir.
* Bir anahtar kasası ve müşterinin yönettiği anahtar için kullanılacak bir anahtar oluşturun. Ayrıca, anahtar kasasında Temizleme korumasını ve geçici silmeyi etkinleştirin.

  ```azurecli-interactive
  az keyvault create -g <resource_group> -n <vault_name> --enable-soft-delete true -enable-purge-protection true
  ```

* Oluşturulan Azure Key Vault, MySQL için Azure veritabanı 'nın veri şifrelemesi için kullanılacak anahtarı oluşturun.

  ```azurecli-interactive
  az keyvault key create --name <key_name> -p software --vault-name <vault_name>
  ```

* Mevcut bir anahtar kasasını kullanabilmeniz için, müşteri tarafından yönetilen anahtar olarak kullanmak için aşağıdaki özelliklere sahip olmalıdır:

  * [Geçici silme](../key-vault/general/soft-delete-overview.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [Korumalı Temizleme](../key-vault/general/soft-delete-overview.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* Anahtar, müşteri tarafından yönetilen anahtar olarak kullanmak için aşağıdaki özniteliklere sahip olmalıdır:
  * Sona erme tarihi yok
  * Devre dışı değil
  * **Al**, **sarmalama**, **geri** alınamaz işlemleri gerçekleştirin

## <a name="set-the-right-permissions-for-key-operations"></a>Anahtar işlemleri için doğru izinleri ayarla

1. MySQL için Azure veritabanı 'nın yönetilen kimliğini almanın iki yolu vardır.

   ### <a name="create-an-new-azure-database-for-mysql-server-with-a-managed-identity"></a>Yönetilen bir kimlikle MySQL için yeni bir Azure veritabanı sunucusu oluşturun.

   ```azurecli-interactive
   az mysql server create --name -g <resource_group> --location <locations> --storage-size size>  -u <user>-p <pwd> --backup-retention <7> --sku-name <sku name> -geo-redundant-backup <Enabled/Disabled>  --assign-identity
   ```

   ### <a name="update-an-existing-the-azure-database-for-mysql-server-to-get-a-managed-identity"></a>Yönetilen bir kimlik almak için mevcut bir MySQL için Azure veritabanı sunucusunu güncelleştirin.

   ```azurecli-interactive
   az mysql server update --name  <server name>  -g <resource_group> --assign-identity
   ```

2. MySQL sunucusunun adı olan **asıl öğe**için **anahtar izinlerini** (**Get**, **Wrap**, **Unwrap**) ayarlayın.

    ```azurecli-interactive
    az keyvault set-policy --name -g <resource_group> --key-permissions get unwrapKey wrapKey --object-id <principal id of the server>
    ```

## <a name="set-data-encryption-for-azure-database-for-mysql"></a>MySQL için Azure veritabanı için veri şifrelemeyi ayarlama

1. Azure Key Vault oluşturulan anahtarı kullanarak MySQL için Azure veritabanı için veri şifrelemeyi etkinleştirin.

    ```azurecli-interactive
    az mysql server key create –name  <server name>  -g <resource_group> --kid <key url>
    ```

    Anahtar URL 'si:`https://YourVaultName.vault.azure.net/keys/YourKeyName/01234567890123456789012345678901>`

## <a name="using-data-encryption-for-restore-or-replica-servers"></a>Geri yükleme veya çoğaltma sunucuları için veri şifrelemeyi kullanma

MySQL için Azure veritabanı, Key Vault ' de depolanan bir müşterinin yönetilen anahtarıyla şifrelendikten sonra, sunucunun yeni oluşturulan kopyası da şifrelenir. Bu yeni kopyayı yerel veya coğrafi geri yükleme işlemi aracılığıyla ya da bir çoğaltma (yerel/bölge) işlemi aracılığıyla yapabilirsiniz. Bu nedenle, şifrelenmiş bir MySQL sunucusu için, şifrelenmiş bir geri yüklenmiş sunucu oluşturmak için aşağıdaki adımları kullanabilirsiniz.

### <a name="creating-a-restoredreplica-server"></a>Geri yüklenen/çoğaltılan sunucu oluşturma

* [Geri yükleme sunucusu oluşturma](howto-restore-server-cli.md) 
* [Okuma çoğaltması sunucusu oluşturma](howto-read-replicas-cli.md) 

### <a name="once-the-server-is-restored-revalidate-data-encryption-the-restored-server"></a>Sunucu geri yüklendikten sonra, geri yüklenen sunucu için veri şifrelemeyi yeniden doğrula

*   Çoğaltma sunucusu için kimlik ata
```azurecli-interactive
az mysql server update --name  <server name>  -g <resoure_group> --assign-identity
```

*   Geri yüklenen/çoğaltma sunucusu için kullanılması gereken mevcut anahtarı al

```azurecli-interactive
az mysql server key list --name  '<server_name>'  -g '<resource_group_name>'
```

*   Geri yüklenen/çoğaltma sunucusu için yeni kimliğin ilkesini ayarlama
  
```azurecli-interactive
az keyvault set-policy --name <keyvault> -g <resoure_group> --key-permissions get unwrapKey wrapKey --object-id <principl id of the server returned by the step 1>
```

* Geri yüklenen/çoğaltılan sunucuyu şifreleme anahtarıyla yeniden doğrulama

```azurecli-interactive
az mysql server key create –name  <server name> -g <resource_group> --kid <key url>
```

## <a name="additional-capability-for-the-key-being-used-for-the-azure-database-for-mysql"></a>MySQL için Azure veritabanı için kullanılan anahtar için ek özellik

### <a name="get-the-key-used"></a>Kullanılan anahtarı al

```azurecli-interactive
az mysql server key show --name  <server name>  -g <resource_group> --kid <key url>
```

Anahtar URL 'si:`https://YourVaultName.vault.azure.net/keys/YourKeyName/01234567890123456789012345678901>`

### <a name="list-the-key-used"></a>Kullanılan anahtarı listeleyin

```azurecli-interactive
az mysql server key list --name  <server name>  -g <resource_group>
```

### <a name="drop-the-key-being-used"></a>Kullanılan anahtarı bırakın

```azurecli-interactive
az mysql server key delete -g <resource_group> --kid <key url>
```

## <a name="using-an-azure-resource-manager-template-to-enable-data-encryption"></a>Veri şifrelemeyi etkinleştirmek için Azure Resource Manager şablonu kullanma

Azure portal dışında, yeni ve mevcut sunucular için Azure Resource Manager şablonlarını kullanarak MySQL için Azure veritabanı sunucusunda veri şifrelemeyi de etkinleştirebilirsiniz.

### <a name="for-a-new-server"></a>Yeni bir sunucu için

Sunucuda veri şifrelemesi etkinken sunucu sağlamak için önceden oluşturulmuş Azure Resource Manager şablonlarından birini kullanın: [veri şifreleme Ile örnek](https://github.com/Azure/azure-mysql/tree/master/arm-templates/ExampleWithDataEncryption)

Bu Azure Resource Manager şablonu, MySQL için Azure veritabanı sunucusu oluşturur ve sunucuda veri şifrelemeyi etkinleştirmek için **anahtar** **Kasası** ve parametre olarak geçirilen anahtarı kullanır.

### <a name="for-an-existing-server"></a>Var olan bir sunucu için

Ayrıca, mevcut MySQL için Azure veritabanı sunucularınızda veri şifrelemeyi etkinleştirmek üzere Azure Resource Manager şablonları kullanabilirsiniz.

* Daha önce bir özellik altında kopyaladığınız Azure Key Vault anahtarının kaynak KIMLIĞINI `Uri` Özellikler nesnesinde geçirin.

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
