---
title: Yönetilen uygulamayı dağıtırken Key Vault'u kullanma
description: Yönetilen Uygulamaları dağıtırken Azure Anahtar Kasası'nda erişim sırlarını nasıl kullanacağımı gösterir
author: tfitzmac
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: f434ad6e19c89f248fec948c0a049fabb0f7c476
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79248443"
---
# <a name="access-key-vault-secret-when-deploying-azure-managed-applications"></a>Azure Yönetilen Uygulamaları dağıtırken Anahtar Kasası gizli erişimine

Dağıtım sırasında parametre olarak güvenli bir değeri (parola gibi) geçirmeniz gerektiğinde, değeri Azure [Anahtar Kasasından](../../key-vault/key-vault-overview.md)alabilirsiniz. Yönetilen Uygulamaları dağıtırken Anahtar Kasası'na erişmek **için, Appliance Resource Provider** hizmet ilkesine erişim izni vermelisiniz. Yönetilen Uygulamalar hizmeti, işlemleri çalıştırmak için bu kimliği kullanır. Dağıtım sırasında Bir Anahtar Kasasından başarıyla bir değer almak için, servis sorumlusunun Anahtar Kasası'na erişebilmesi gerekir.

Bu makalede, Yönetilen Uygulamalar ile çalışacak şekilde Anahtar Kasası nasıl yapılandırılabildiğini açıklanmaktadır.

## <a name="enable-template-deployment"></a>Şablon dağıtımını etkinleştirme

1. Portalda, Anahtar Kasanızı seçin.

1. **Erişim ilkeleri**'ni seçin.   

   ![Erişim ilkelerini seçin](./media/key-vault-access/select-access-policies.png)

1. **Gelişmiş erişim ilkelerini görüntülemek için tıklayın**'ı seçin.

   ![Gelişmiş erişim ilkelerini göster](./media/key-vault-access/advanced.png)

1. **Şablon dağıtımı için Azure Kaynak Yöneticisi'ne erişimi etkinleştir'i**seçin. Ardından **Kaydet**’i seçin.

   ![Şablon dağıtımını etkinleştirme](./media/key-vault-access/enable-template.png)

## <a name="add-service-as-contributor"></a>Katılımcı olarak hizmet ekleme

1. **Erişim denetimi (IAM) seçeneğini**belirleyin.

   ![Erişim denetimini seçin](./media/key-vault-access/access-control.png)

1. **Rol ataması ekle**’yi seçin.

   ![ekle bağlantısını seçme](./media/key-vault-access/add-access-control.png)

1. Rol için **Katkıda Bulunan'ı** seçin. Cihaz **Kaynak Sağlayıcısı'nı** arayın ve kullanılabilir seçeneklerarasından seçin.

   ![Sağlayıcı ara](./media/key-vault-access/search-provider.png)

1. **Kaydet'i**seçin.

## <a name="reference-key-vault-secret"></a>Referans Anahtar Vault gizli

Bir anahtarı Niçin Yönetilen Uygulamanızdaki şablona geçirmek için [bağlantılı veya iç içe alınmış](../templates/linked-templates.md) bir şablon kullanmanız ve bağlı veya iç içe geçen şablonparametrelerinde Anahtar Kasası'na başvurmanız gerekir. Anahtar Kasası'nın kaynak kimliğini ve sırrın adını sağlayın.

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

Yönetilen Uygulama dağıtımı sırasında erişilebilir olacak şekilde Anahtar Kasanızı yapılandırıldınız.

* Bir değeri şablon parametresi olarak Anahtar Kasasından geçirme hakkında bilgi için, [dağıtım sırasında güvenli parametre değerini geçmek için Azure Anahtar Kasası'nı kullanın'a](../templates/key-vault-parameter.md)bakın.
* Yönetilen uygulama örnekleri [için, Azure yönetilen uygulamalar için Örnek projelere](sample-projects.md)bakın.
* Yönetilen bir uygulamaya ait bir kullanıcı arabirimi tanım dosyası oluşturma hakkında bilgi için [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md) konusunu inceleyin.
