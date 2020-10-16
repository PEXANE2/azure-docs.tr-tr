---
title: Azure kılavuzu-Azure Resource Manager şablonu kullanarak Azure Anahtar Kasası ve kasa erişim ilkesi oluşturma | Microsoft Docs
description: Azure Resource Manager şablonu kullanarak Azure Anahtar Kasası ve kasa erişim ilkelerinin nasıl oluşturulacağını gösterir.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: how-to
ms.date: 10/5/2020
ms.author: mbaldwin
ms.openlocfilehash: cf19561005fe2e98b7b5cf6812ff9224fd9474dc
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91804409"
---
# <a name="how-to-create-azure-key-vault-and-vault-access-policy-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak Azure Key Vault ve kasa erişim ilkesi oluşturma

[Azure Key Vault](../general/overview.md) , anahtarlar, parolalar, sertifikalar ve diğer gizli diziler gibi gizli dizileri için güvenli bir depo sağlayan bir bulut hizmetidir. Bu kılavuz, bir Anahtar Kasası oluşturmak için bir Azure Resource Manager şablonu dağıtma işlemine odaklanır (ARM şablonu).

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi gerçekleştirmek için:

* Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="create-key-vault-resource-manager-template"></a>Key Vault Kaynak Yöneticisi şablonu oluştur

Aşağıdaki şablonda Anahtar Kasası oluşturmanın temel bir yolu gösterilmektedir. Bazı değerler şablon içinde belirtilmiştir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "keyVaultName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the key vault."
      }
    },
    "skuName": {
      "type": "string",
      "defaultValue": "Standard",
      "allowedValues": [
        "Standard",
        "Premium"
      ],
      "metadata": {
        "description": "Specifies whether the key vault is a standard vault or a premium vault."
      }
    }
   },
  "resources": [
    {
      "type": "Microsoft.KeyVault/vaults",
      "apiVersion": "2019-09-01",
      "name": "[parameters('keyVaultName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "enabledForDeployment": "false",
        "enabledForDiskEncryption": "false",
        "enabledForTemplateDeployment": "false",
        "tenantId": "[subscription().tenantId]",
        "accessPolicies": [],
        "sku": {
          "name": "[parameters('skuName')]",
          "family": "A"
        },
        "networkAcls": {
          "defaultAction": "Allow",
          "bypass": "AzureServices"
        }
      }
    }
  ]
}

```

Key Vault şablon ayarları hakkında daha fazla bilgi için bkz. [Key Vault ARM şablonu başvurusu](https://docs.microsoft.com/azure/templates/microsoft.keyvault/vaults).

> [!IMPORTANT]
> Şablon yeniden dağıtılırsa, anahtar kasasındaki mevcut erişim ilkelerini geçersiz kılar. `accessPolicies`Anahtar kasasına erişimi gevleşmemek için özelliği mevcut erişim ilkeleriyle doldurmanız önerilir. 

## <a name="add-access-policy-to-key-vault-resource-manager-template"></a>Key Vault Kaynak Yöneticisi şablonuna erişim ilkesi Ekle

Tüm Anahtar Kasası şablonunu yeniden dağıtmanız gerekmeden, mevcut anahtar kasasına erişim ilkeleri dağıtabilirsiniz. Aşağıdaki şablonda, erişim ilkeleri oluşturmanın temel bir yolu gösterilmektedir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "keyVaultName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the key vault."
      }
    },
    "objectId": {
      "type": "string",
      "metadata": {
        "description": "Specifies the object ID of a user, service principal or security group in the Azure Active Directory tenant for the vault. The object ID must be unique for the list of access policies. Get it by using Get-AzADUser or Get-AzADServicePrincipal cmdlets."
      }
    },
    "keysPermissions": {
      "type": "array",
      "defaultValue": [
        "list"
      ],
      "metadata": {
        "description": "Specifies the permissions to keys in the vault. Valid values are: all, encrypt, decrypt, wrapKey, unwrapKey, sign, verify, get, list, create, update, import, delete, backup, restore, recover, and purge."
      }
    },
    "secretsPermissions": {
      "type": "array",
      "defaultValue": [
        "list"
      ],
      "metadata": {
        "description": "Specifies the permissions to secrets in the vault. Valid values are: all, get, list, set, delete, backup, restore, recover, and purge."
      }
    },
    "certificatePermissions": {
      "type": "array",
      "defaultValue": [
        "list"
      ],
      "metadata": {
        "description": "Specifies the permissions to certificates in the vault. Valid values are: all,  create, delete, update, deleteissuers, get, getissuers, import, list, listissuers, managecontacts, manageissuers,  recover, backup, restore, setissuers, and purge."
      }
    },
  "resources": [
     {
      "type": "Microsoft.KeyVault/vaults/accessPolicies",
      "name": "[concat(parameters('keyVaultName'), '/add')]",
      "apiVersion": "2019-09-01",
      "properties": {
        "accessPolicies": [
          {
            "tenantId": "[subscription().tenantId]",
            "objectId": "[parameters('objectId')]",
            "permissions": {
              "keys": "[parameters('keysPermissions')]",
              "secrets": "[parameters('secretsPermissions')]",
              "certificates": [parameters('certificatesPermissions')]
            }
          }
        ]
      }
    }
  ]
}

```
Key Vault şablon ayarları hakkında daha fazla bilgi için bkz. [Key Vault ARM şablonu başvurusu](https://docs.microsoft.com/azure/templates/microsoft.keyvault/vaults/accesspolicies).

## <a name="other-available-key-vault-resource-manager-templates"></a>Diğer kullanılabilir Key Vault Kaynak Yöneticisi şablonları

Key Vault nesneleri için kullanılabilen diğer Kaynak Yöneticisi şablonları vardır:

| Gizli Diziler | Anahtarlar | Sertifikalar |
|--|--|--|
|[Hızlı Başlangıç](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-template)<br>[Başvuru](https://docs.microsoft.com/azure/templates/microsoft.keyvault/vaults/secrets)|Yok|Yok|

Burada bulabileceğiniz daha fazla Key Vault şablonu: [Key Vault Kaynak Yöneticisi başvurusu](https://docs.microsoft.com/azure/templates/microsoft.keyvault/allversions)

## <a name="deploy-the-templates"></a>Şablonları dağıtma

Aşağıdaki kılavuzda yer alarak, ' Düzenleyicinizde kendi şablonunuzu derleme ' seçeneğini kullanarak yukarıdaki şablonları dağıtmak için Azure portal kullanabilirsiniz: [kaynakları özel şablondan dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/templates/deploy-portal#deploy-resources-from-custom-template)

Ayrıca, yukarıdaki şablonları dosyalara kaydedebilir ve aşağıdaki komutları kullanabilirsiniz:  [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) ve [az Group Deployment Create](/cli/azure/group/deployment#az-group-deployment-create):

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile key-vault-template.json
```

```azurecli
az group deployment create --resource-group ExampleGroup --template-file key-vault-template.json
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Devam etmeyi ve sonraki hızlı başlangıç ve öğreticilerde çalışmayı planlıyorsanız, bu kaynakları yerinde bırakabilirsiniz. Kaynaklara artık gerek kalmadığında, anahtar kasasını ve ilgili kaynakları silen kaynak grubunu silin. Azure CLı veya Azure PowerShell kullanarak kaynak grubunu silmek için aşağıdaki adımları kullanın.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="resources"></a>Kaynaklar

- [Azure Key Vault genel bakışını](../general/overview.md) okuyun
- [Azure Resource Manager](../../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin
- [En iyi uygulamaları](../general/best-practices.md) gözden geçirin Azure Key Vault

## <a name="next-steps"></a>Sonraki adımlar

- [Anahtar kasasına güvenli erişim](secure-your-key-vault.md)
- [Anahtar kasasında kimlik doğrulama](authentication.md)
- [Geliştirici Kılavuzu Azure Key Vault](developers-guide.md)
