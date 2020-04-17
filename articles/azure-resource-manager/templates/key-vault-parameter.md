---
title: Şablon ile Key Vault gizli
description: Dağıtım sırasında parametre olarak anahtar kasasından bir sırrın nasıl geçirilir olduğunu gösterir.
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: d21a7d727091b427fee59e22db6a77a495a4eab7
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458275"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Dağıtım sırasında güvenli parametre değerini geçirmek için Azure Anahtar Kasası'nı kullanın

Şablonunuza veya parametre dosyanıza doğrudan güvenli bir değer (parola gibi) koymak yerine, dağıtım sırasında değeri Azure [Anahtar Kasasından](../../key-vault/general/overview.md) alabilirsiniz. Parametre dosyanızdaki anahtar kasasına ve gizliye başvurarak değeri alırsınız. Değer hiçbir zaman açıklanamaz çünkü yalnızca anahtar kasa kimliğine başvurursunuz. Anahtar kasası, dağıtmakta olduğunuz kaynak grubundan farklı bir abonelikte bulunabilir.

Bu makalede, şablon parametresi olarak hassas bir değer geçirme senaryosu na odaklanın. Sanal makine özelliğini Anahtar Kasa'daki sertifikanın URL'sine ayarlama senaryosunu kapsamaz. Bu senaryonun hızlı başlangıç şablonu için, [Azure Anahtar Kasasından Bir Sertifikayı Sanal Makine'ye](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-winrm-keyvault-windows)yükleyin'e bakın.

## <a name="deploy-key-vaults-and-secrets"></a>Anahtar kasaları ve sırları dağıtın

Şablon dağıtımı sırasında anahtar kasasına `enabledForTemplateDeployment` erişmek için, `true`anahtar tonozuna ' ya ayarlı.

Zaten bir Anahtar Kasanız varsa, şablon dağıtımlarına izin verdiğinden emin olun.

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az keyvault update  --name ExampleVault --enabled-for-template-deployment true
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
Set-AzKeyVaultAccessPolicy -VaultName ExampleVault -EnabledForTemplateDeployment
```

---

Yeni bir Key Vault oluşturmak ve bir sır eklemek için şunları kullanın:

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create --name ExampleGroup --location centralus
az keyvault create \
  --name ExampleVault \
  --resource-group ExampleGroup \
  --location centralus \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name ExampleVault --name "ExamplePassword" --value "hVFkk965BuUv"
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

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

Anahtar kasasahibi olarak, otomatik olarak sırlarını oluşturma erişimi ne var. Sırlar ile çalışan kullanıcı anahtar kasasının sahibi değilse, aşağıdakilerle erişim izni ver:

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az keyvault set-policy \
  --upn <user-principal-name> \
  --name ExampleVault \
  --secret-permissions set delete get list
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
$userPrincipalName = "<Email Address of the deployment operator>"

Set-AzKeyVaultAccessPolicy `
  -VaultName ExampleVault `
  -UserPrincipalName <user-principal-name> `
  -PermissionsToSecrets set,delete,get,list
```

---

Önemli kasalar oluşturma ve sır ekleme hakkında daha fazla bilgi için bkz:

- [CLI kullanarak bir sırrı ayarlayın ve alın](../../key-vault/secrets/quick-create-cli.md)
- [Powershell'i kullanarak bir sırrı ayarlayın ve alın](../../key-vault/secrets/quick-create-powershell.md)
- [Portalı kullanarak bir sırrı ayarlayın ve alın](../../key-vault/secrets/quick-create-portal.md)
- [.NET kullanarak bir sırrı ayarlayın ve alın](../../key-vault/secrets/quick-create-net.md)
- [Node.js kullanarak bir sırrı ayarlayın ve alın](../../key-vault/secrets/quick-create-node.md)

## <a name="grant-access-to-the-secrets"></a>Sırlara erişim izni ver

Şablonu dağıtan kullanıcının kaynak `Microsoft.KeyVault/vaults/deploy/action` grubunun kapsamı ve anahtar kasası için izin alması gerekir. [Sahibi](../../role-based-access-control/built-in-roles.md#owner) ve [Katılımcı](../../role-based-access-control/built-in-roles.md#contributor) rolleri hem bu erişimi verir. Anahtar kasasını oluşturduysanız, sahibi sizsiniz, bu yüzden izniniz var.

Aşağıdaki yordam, minimum izinle bir rolün nasıl oluşturulup oluşturulabildiğini ve kullanıcının nasıl atayılabildiğini gösterir

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
    "00000000-0000-0000-0000-00000000000000000000000" yerine abonelik kimliği ile değiştirin.

2. JSON dosyasını kullanarak yeni rol oluşturun:

    # <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

    ```azurecli-interactive
    az role definition create --role-definition "<path-to-role-file>"
    az role assignment create \
      --role "Key Vault resource manager template deployment operator" \
      --assignee <user-principal-name> \
      --resource-group ExampleGroup
    ```

    # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

    ```azurepowershell-interactive
    New-AzRoleDefinition -InputFile "<path-to-role-file>"
    New-AzRoleAssignment `
      -ResourceGroupName ExampleGroup `
      -RoleDefinitionName "Key Vault resource manager template deployment operator" `
      -SignInName <user-principal-name>
    ```

    ---

    Örnekler, kaynak grubu düzeyinde kullanıcıya özel rolü atar.

[Yönetilen Uygulama](../managed-applications/overview.md)şablonuna sahip bir Anahtar Kasası kullanırken, Appliance **Resource Provider** hizmet ilkesine erişim izni vermelisiniz. Daha fazla bilgi için Azure [Yönetilen Uygulamaları dağıtırken Access Key Vault gizli](../managed-applications/key-vault-access.md)bakın.

## <a name="reference-secrets-with-static-id"></a>Statik kimlikli başvuru sırları

Bu yaklaşımla, şablona değil, parametre dosyasındaki anahtar kasasına başvurursunuz. Aşağıdaki resim, parametre dosyasının gizliye nasıl başvurulup bu değeri şablona nasıl aktardığını gösterir.

![Kaynak Yöneticisi anahtar kasa tümleştirmeStatik Kimlik diyagramı](./media/key-vault-parameter/statickeyvault.png)

[Öğretici: Azure Anahtar Kasası'nı Kaynak Yöneticisi Şablonu dağıtımına entegre](./template-tutorial-use-key-vault.md) etmek bu yöntemi kullanır.

Aşağıdaki şablon, yönetici parolası içeren bir SQL sunucusu dağıtlar. Parola parametresi güvenli bir dize olarak ayarlanır. Ancak şablon, bu değerin nereden geldiğini belirtmez.

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
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "name": "[parameters('sqlServerName')]",
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

Şimdi, önceki şablon için bir parametre dosyası oluşturun. Parametre dosyasında, şablondaki parametrenin adıyla eşleşen bir parametre belirtin. Parametre değeri için, anahtar kasasından gizli başvuru. Anahtar kasasının kaynak tanımlayıcısını ve sırrın adını geçirerek gizliye başvurursunuz:

Aşağıdaki parametre dosyasında, anahtar kasa sıcağa zaten var olmalı ve kaynak kimliği için statik bir değer sağlarsınız.

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

Geçerli sürüm dışında gizli bir sürümünü kullanmanız gerekiyorsa, `secretVersion` özelliği kullanın.

```json
"secretName": "ExamplePassword",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Şablonu dağıtın ve parametre dosyasına geçirin:

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

```azurecli-interactive
az group create --name SqlGroup --location westus2
az deployment group create \
  --resource-group SqlGroup \
  --template-uri <template-file-URI> \
  --parameters <parameter-file>
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri <template-file-URI> `
  -TemplateParameterFile <parameter-file>
```

---

## <a name="reference-secrets-with-dynamic-id"></a>Dinamik kimliği olan başvuru sırları

Önceki bölümde parametreden anahtar kasa gizli için statik bir kaynak kimliği geçmek için nasıl gösterdi. Ancak, bazı senaryolarda, geçerli dağıtıma bağlı olarak değişen bir anahtar kasa sırrına başvurmanız gerekir. Veya, parametre dosyasında bir başvuru parametresi oluşturmak yerine parametre değerlerini şablona geçirmek isteyebilirsiniz. Her iki durumda da, bağlantılı bir şablon kullanarak önemli bir kasa gizli için kaynak kimliğini dinamik olarak oluşturabilirsiniz.

Şablon ifadeleri parametre dosyasında izin verilmediği için parametreler dosyasındaki kaynak kimliğini dinamik olarak oluşturamazsınız.

Üst şablonunuzda, iç içe geçen şablonu ekleyin ve dinamik olarak oluşturulan kaynak kimliğini içeren bir parametrede geçirin. Aşağıdaki resim, bağlı şablondaki bir parametrenin gizliye nasıl başvurulabilir olduğunu gösterir.

![Dinamik Kimlik](./media/key-vault-parameter/dynamickeyvault.png)

Aşağıdaki şablon dinamik olarak anahtar kasa kimliğini oluşturur ve bir parametre olarak geçirir.

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
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "name": "dynamicSecret",
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
              "type": "Microsoft.Sql/servers",
              "apiVersion": "2018-06-01-preview",
              "name": "[variables('sqlServerName')]",
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

- Anahtar kasaları hakkında genel bilgi için Azure [Anahtar Kasası nedir?](../../key-vault/general/overview.md)
- Önemli sırlara başvurmanın tam örnekleri [için, Anahtar Kasa örneklerine](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples)bakın.
