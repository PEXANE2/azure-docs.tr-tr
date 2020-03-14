---
title: Yönetilen uygulamayı dağıttığınızda Key Vault kullan
description: Yönetilen uygulamalar dağıtıldığında Azure Key Vault erişim sırlarının nasıl kullanılacağını gösterir
author: tfitzmac
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: f434ad6e19c89f248fec948c0a049fabb0f7c476
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79248443"
---
# <a name="access-key-vault-secret-when-deploying-azure-managed-applications"></a>Azure yönetilen uygulamalar dağıtıldığında erişim Key Vault gizliliği

Dağıtım sırasında bir parametre olarak güvenli bir değer (parola gibi) geçirmeniz gerektiğinde değeri bir [Azure Key Vault](../../key-vault/key-vault-overview.md)alabilirsiniz. Yönetilen uygulamaları dağıttığınızda Key Vault erişmek için, **gereç kaynak sağlayıcısı** hizmet sorumlusuna erişim vermeniz gerekir. Yönetilen uygulamalar hizmeti, işlemleri çalıştırmak için bu kimliği kullanır. Dağıtım sırasında Key Vault bir değeri başarıyla almak için, hizmet sorumlusu Key Vault erişebilmelidir.

Bu makalede, Key Vault yönetilen uygulamalarla çalışacak şekilde nasıl yapılandırılacağı açıklanır.

## <a name="enable-template-deployment"></a>Şablon dağıtımını etkinleştir

1. Portalda Key Vault seçin.

1. **Erişim ilkeleri**'ni seçin.   

   ![Erişim ilkelerini seçin](./media/key-vault-access/select-access-policies.png)

1. **Gelişmiş erişim ilkelerini görüntülemek için tıklayın**'ı seçin.

   ![Gelişmiş erişim ilkelerini göster](./media/key-vault-access/advanced.png)

1. **Şablon dağıtımı için Azure Resource Manager erişimi etkinleştir**' i seçin. Ardından **Kaydet**’i seçin.

   ![Şablon dağıtımını etkinleştir](./media/key-vault-access/enable-template.png)

## <a name="add-service-as-contributor"></a>Hizmet katkıda bulunan olarak ekle

1. **Erişim denetimi (IAM)** öğesini seçin.

   ![Erişim denetimi seçin](./media/key-vault-access/access-control.png)

1. **Rol ataması Ekle**' yi seçin.

   ![Ekle 'yi seçin](./media/key-vault-access/add-access-control.png)

1. Rol için **katkıda bulunan** öğesini seçin. **Gereç kaynak sağlayıcısını** arayın ve kullanılabilir seçeneklerden seçin.

   ![Sağlayıcı ara](./media/key-vault-access/search-provider.png)

1. **Kaydet**’i seçin.

## <a name="reference-key-vault-secret"></a>Key Vault gizli dizi başvurusu

Bir Key Vault gizli anahtar yönetilen uygulamanızdaki bir şablona geçirmek için [bağlantılı veya iç içe geçmiş bir şablon](../templates/linked-templates.md) kullanmanız ve bağlantılı veya iç içe şablon için parametrelerde Key Vault başvurmanız gerekir. Key Vault kaynak KIMLIĞINI ve gizli dizi adını sağlayın.

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

Key Vault yönetilen bir uygulamanın dağıtımı sırasında erişilebilir olacak şekilde yapılandırdınız.

* Bir Key Vault değeri şablon parametresi olarak geçirme hakkında daha fazla bilgi için bkz. [dağıtım sırasında güvenli parametre değeri geçirmek için Azure Key Vault kullanma](../templates/key-vault-parameter.md).
* Yönetilen uygulama örnekleri için bkz. [Azure yönetilen uygulamalar Için örnek projeler](sample-projects.md).
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.
