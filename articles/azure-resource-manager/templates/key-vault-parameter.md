---
title: Şablonla gizli Key Vault
description: Dağıtım sırasında anahtar kasasından bir parametre olarak bir parolanın nasıl geçirileceğini gösterir.
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 75757d36be62289023f96adcdb2f81589b54281b
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75689673"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Dağıtım sırasında güvenli parametre değeri geçirmek için Azure Key Vault kullanma

Güvenli bir değer (parola gibi) doğrudan şablon veya parametre dosyanıza koymak yerine, bir dağıtım sırasında [Azure Key Vault](../../key-vault/key-vault-overview.md) değeri alabilirsiniz. Değeri, parametre dosyanızda anahtar kasası ve gizli dizi ile başvurarak alırsınız. Yalnızca kendi Anahtar Kasası KIMLIĞINE başvurduğundan değer hiçbir şekilde gösterilmez. Anahtar Kasası, dağıtmakta olduğunuz kaynak grubundan farklı bir abonelikte bulunabilir.

Bu makale, bir şablon parametresi olarak ' de hassas bir değer geçirme senaryosuna odaklanır. Bir sanal makine özelliğini bir Key Vault sertifikanın URL 'sine ayarlama senaryosunu kapsamaz. Bu senaryonun hızlı başlangıç şablonu için bkz. bir [sanal makineye Azure Key Vault sertifika yüklemeyi](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows).

## <a name="deploy-key-vaults-and-secrets"></a>Anahtar kasaları ve gizli dizileri dağıtma

Şablon dağıtımı sırasında bir anahtar kasasına erişmek için, anahtar kasasındaki `enabledForTemplateDeployment` `true`olarak ayarlayın.

Zaten bir Key Vault varsa, şablon dağıtımlarının izin verdiğinden emin olun.

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az keyvault update  --name ExampleVault --enabled-for-template-deployment true
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy -VaultName ExampleVault -EnabledForTemplateDeployment
```

---

Yeni bir Key Vault oluşturmak ve bir gizli dizi eklemek için şunu kullanın:

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create --name ExampleGroup --location centralus
az keyvault create \
  --name ExampleVault \
  --resource-group ExampleGroup \
  --location centralus \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name ExampleVault --name "ExamplePassword" --value "hVFkk965BuUv"
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name ExampleGroup -Location centralus
New-AzKeyVault `
  -VaultName ExampleVault `
  -resourceGroupName ExampleGroup `
  -Location centralus `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString 'hVFkk965BuUv' -AsPlainText -Force
$secret = Set-AzKeyVaultSecret -VaultName ExampleVault -Name 'ExamplePassword' -SecretValue $secretvalue
```

---

Anahtar kasasının sahibi olarak otomatik olarak gizli dizi oluşturma erişiminiz vardır. Gizli dizi ile çalışan Kullanıcı anahtar kasasının sahibi değilse, şu ile erişim izni verin:

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az keyvault set-policy \
  --upn <user-principal-name> \
  --name ExampleVault \
  --secret-permissions set delete get list
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$userPrincipalName = "<Email Address of the deployment operator>"

Set-AzKeyVaultAccessPolicy `
  -VaultName ExampleVault `
  -UserPrincipalName <user-principal-name> `
  -PermissionsToSecrets set,delete,get,list
```

---

Anahtar kasaları oluşturma ve gizli dizileri ekleme hakkında daha fazla bilgi için bkz.:

- [CLı kullanarak bir gizli dizi ayarlama ve alma](../../key-vault/quick-create-cli.md)
- [PowerShell kullanarak bir gizli dizi ayarlama ve alma](../../key-vault/quick-create-powershell.md)
- [Portalı kullanarak bir gizli dizi ayarlama ve alma](../../key-vault/quick-create-portal.md)
- [.NET kullanarak bir gizli dizi ayarlama ve alma](../../key-vault/quick-create-net.md)
- [Node. js kullanarak bir gizli dizi ayarlama ve alma](../../key-vault/quick-create-node.md)

## <a name="grant-access-to-the-secrets"></a>Gizli dizileri için erişim izni verme

Şablonu dağıtan Kullanıcı, kaynak grubunun ve anahtar kasasının kapsamı için `Microsoft.KeyVault/vaults/deploy/action` iznine sahip olmalıdır. [Sahip](../../role-based-access-control/built-in-roles.md#owner) ve [katkıda bulunan](../../role-based-access-control/built-in-roles.md#contributor) rollerinin her ikisi de bu erişimi verir. Anahtar Kasası oluşturduysanız, izninizin olması gerekir.

Aşağıdaki yordamda, en düşük izinle bir rol oluşturma ve kullanıcının nasıl atanacağı gösterilmektedir

1. Özel bir rol tanımı JSON dosyası oluşturun:

    ```json
    {
      "Name": "Key Vault resource manager template deployment operator",
      "IsCustom": true,
      "Description": "Lets you deploy a resource manager template with the access to the secrets in the Key Vault.",
      "Actions": [
        "Microsoft.KeyVault/vaults/deploy/action"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    "00000000-0000-0000-0000-000000000000" öğesini abonelik KIMLIĞIYLE değiştirin.

2. JSON dosyasını kullanarak yeni rolü oluşturun:

    # <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

    ```azurecli-interactive
    az role definition create --role-definition "<path-to-role-file>"
    az role assignment create \
      --role "Key Vault resource manager template deployment operator" \
      --assignee <user-principal-name> \
      --resource-group ExampleGroup
    ```

    # <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

    ```azurepowershell-interactive
    New-AzRoleDefinition -InputFile "<path-to-role-file>"
    New-AzRoleAssignment `
      -ResourceGroupName ExampleGroup `
      -RoleDefinitionName "Key Vault resource manager template deployment operator" `
      -SignInName <user-principal-name>
    ```

    ---

    Örnekler, kullanıcıya kaynak grubu düzeyinde özel rol atar.  

[Yönetilen bir uygulama](../managed-applications/overview.md)şablonuyla bir Key Vault kullanırken, **gereç kaynak sağlayıcısı** hizmet sorumlusuna erişim vermeniz gerekir. Daha fazla bilgi için bkz. [Azure yönetilen uygulamalar dağıtıldığında erişim Key Vault gizliliği](../managed-applications/key-vault-access.md).

## <a name="reference-secrets-with-static-id"></a>Statik KIMLIK ile başvuru gizli dizileri

Bu yaklaşımda, şablon değil, parametre dosyasındaki anahtar kasasına başvurun. Aşağıdaki görüntüde, parametre dosyasının gizli dizi ile nasıl başvurduğu gösterilmektedir ve bu değer şablona geçirilir.

![Kaynak Yöneticisi Anahtar Kasası tümleştirme statik KIMLIK diyagramı](./media/key-vault-parameter/statickeyvault.png)

[Öğretici: Kaynak Yöneticisi Şablon dağıtımı Azure Key Vault tümleştirme](./template-tutorial-use-key-vault.md) bu yöntemi kullanır.

Aşağıdaki şablon, yönetici parolası içeren bir SQL Server dağıtır. Password parametresi güvenli bir dizeye ayarlanır. Ancak, şablon bu değerin nereden geldiğini belirtmez.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminLogin": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    },
    "sqlServerName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "name": "[parameters('sqlServerName')]",
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "administratorLogin": "[parameters('adminLogin')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      }
    }
  ],
  "outputs": {
  }
}
```

Şimdi, önceki şablon için bir parametre dosyası oluşturun. Parametre dosyasında, şablondaki parametrenin adıyla eşleşen bir parametre belirtin. Parametre değeri için anahtar kasasından gizli dizi başvurusu yapın. Anahtar kasasının kaynak tanımlayıcısını ve gizli anahtar adını geçirerek gizli dizi başvurusunu alırsınız:

Aşağıdaki parametre dosyasında, Anahtar Kasası parolasının zaten mevcut olması ve kaynak KIMLIĞI için statik bir değer sağlamanız gerekir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminLogin": {
            "value": "exampleadmin"
        },
        "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "ExamplePassword"
            }
        },
        "sqlServerName": {
            "value": "<your-server-name>"
        }
    }
}
```

Parolanın geçerli sürümden farklı bir sürümünü kullanmanız gerekiyorsa `secretVersion` özelliğini kullanın.

```json
"secretName": "ExamplePassword",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Şablonu dağıtın ve parametre dosyasını geçirin:

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create --name SqlGroup --location westus2
az group deployment create \
    --resource-group SqlGroup \
    --template-uri <template-file-URI> \
    --parameters <parameter-file>
```

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri <template-file-URI> `
  -TemplateParameterFile <parameter-file>
```

---

## <a name="reference-secrets-with-dynamic-id"></a>Dinamik KIMLIK ile başvuru gizli dizileri

Önceki bölümde, bir statik kaynak KIMLIĞININ parametresinden Anahtar Kasası gizli dizisi için nasıl geçirilediği gösterildi. Ancak bazı senaryolarda, geçerli dağıtıma göre farklılık gösteren bir Anahtar Kasası gizliliğine başvurmanız gerekir. Ya da parametre değerlerini parametre dosyasında bir başvuru parametresi oluşturmak yerine şablona geçirmek isteyebilirsiniz. Her iki durumda da, bağlantılı bir şablon kullanarak bir Anahtar Kasası parolasının kaynak KIMLIĞINI dinamik olarak oluşturabilirsiniz.

Parametreler dosyasında şablon ifadelerine izin verilmediğinden, Parametreler dosyasında dinamik olarak kaynak KIMLIĞI oluşturamazsınız. 

Ana şablonunuzda, iç içe geçmiş şablonu ekleyin ve dinamik olarak üretilen kaynak KIMLIĞINI içeren bir parametreyi geçirin. Aşağıdaki görüntüde, bağlantılı şablondaki bir parametrenin gizli dizi nasıl başvurduğu gösterilmektedir.

![Dinamik KIMLIK](./media/key-vault-parameter/dynamickeyvault.png)

Aşağıdaki şablon Anahtar Kasası KIMLIĞINI dinamik olarak oluşturur ve bir parametre olarak geçirir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "The location where the resources will be deployed."
            }
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "The name of the keyvault that contains the secret."
            }
        },
        "secretName": {
            "type": "string",
            "metadata": {
                "description": "The name of the secret."
            }
        },
        "vaultResourceGroupName": {
            "type": "string",
            "metadata": {
                "description": "The name of the resource group that contains the keyvault."
            }
        },
        "vaultSubscription": {
            "type": "string",
            "defaultValue": "[subscription().subscriptionId]",
            "metadata": {
                "description": "The name of the subscription that contains the keyvault."
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "dynamicSecret",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "expressionEvaluationOptions": {
                    "scope": "inner"
                },
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "adminLogin": {
                            "type": "string"
                        },
                        "adminPassword": {
                            "type": "securestring"
                        },
                        "location": {
                            "type": "string"
                        }
                    },
                    "variables": {
                        "sqlServerName": "[concat('sql-', uniqueString(resourceGroup().id, 'sql'))]"
                    },
                    "resources": [
                        {
                            "name": "[variables('sqlServerName')]",
                            "type": "Microsoft.Sql/servers",
                            "apiVersion": "2018-06-01-preview",
                            "location": "[parameters('location')]",
                            "properties": {
                                "administratorLogin": "[parameters('adminLogin')]",
                                "administratorLoginPassword": "[parameters('adminPassword')]"
                            }
                        }
                    ],
                    "outputs": {
                        "sqlFQDN": {
                            "type": "string",
                            "value": "[reference(variables('sqlServerName')).fullyQualifiedDomainName]"
                        }
                    }
                },
                "parameters": {
                    "location": {
                        "value": "[parameters('location')]"
                    },
                    "adminLogin": {
                        "value": "ghuser"
                    },
                    "adminPassword": {
                        "reference": {
                            "keyVault": {
                                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                            },
                            "secretName": "[parameters('secretName')]"
                        }
                    }
                }
            }
        }
    ],
    "outputs": {
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Anahtar kasaları hakkında genel bilgi için bkz. [Azure Key Vault nedir?](../../key-vault/key-vault-overview.md).
- Anahtar gizli dizileri ile ilgili tüm örnekler için, [Key Vault örneklere](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)bakın.
