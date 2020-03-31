---
title: REST API ve şablonu yla kaynakları dağıtma
description: Kaynakları Azure'a dağıtmak için Azure Kaynak Yöneticisi ve Kaynak Yöneticisi REST API'yi kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
ms.topic: conceptual
ms.date: 06/04/2019
ms.openlocfilehash: 9cdb7b668e5170917b41ef49639bd9a17e538766
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80153242"
---
# <a name="deploy-resources-with-arm-templates-and-resource-manager-rest-api"></a>ARM şablonları ve Kaynak Yöneticisi REST API ile kaynakları dağıtma

Bu makalede, kaynaklarınızı Azure'a dağıtmak için Azure Kaynak Yöneticisi (ARM) şablonlarıyla Kaynak Yöneticisi REST API'sinin nasıl kullanılacağı açıklanmaktadır.

Şablonunuzu istek gövdesine veya bir dosyaya bağlantıya ekleyebilirsiniz. Bir dosya kullanırken, yerel bir dosya veya URI üzerinden kullanılabilen harici bir dosya olabilir. Şablonunuzun bir depolama hesabında olması durumunda, şablona erişimi kısıtlayabilir ve dağıtım sırasında paylaşılan bir erişim imzası (SAS) belirteci sağlayabilirsiniz.

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınızı bir yönetim grubuna, bir Azure aboneliğine veya kaynak grubuna hedefleyebilirsiniz. Çoğu durumda, bir kaynak grubuna dağıtımları hedeflersiniz. Belirtilen kapsamda ilkeleri ve rol atamaları uygulamak için yönetim grubunu veya abonelik dağıtımlarını kullanın. Ayrıca, bir kaynak grubu oluşturmak ve kaynakları ona dağıtmak için abonelik dağıtımlarını da kullanırsınız. Dağıtımın kapsamına bağlı olarak, farklı komutlar kullanırsınız.

Bir kaynak **grubuna**dağıtmak [için, Dağıtımlar](/rest/api/resources/deployments/createorupdate)' ı kullanın - Oluştur . İstek aşağıdakilere gönderilir:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Bir **aboneye**dağıtmak için [Dağıtımlar '](/rest/api/resources/deployments/createorupdateatsubscriptionscope)ı kullanın - Abonelik Kapsamında Oluştur . İstek aşağıdakilere gönderilir:

```HTTP
PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Abonelik düzeyi dağıtımları hakkında daha fazla bilgi için [bkz.](deploy-to-subscription.md)

Bir yönetim **grubuna**dağıtmak [için, Dağıtımları kullanın - Yönetim Grubu Kapsamı'nda Oluştur.](/rest/api/resources/deployments/createorupdateatmanagementgroupscope) İstek aşağıdakilere gönderilir:

```HTTP
PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{groupId}/providers/Microsoft.Resources/deployments/{deploymentName}?api-version=2019-05-01
```

Yönetim grubu düzeyindeki dağıtımlar hakkında daha fazla bilgi için [bkz.](deploy-to-management-group.md)

Bu makaledeki örneklerkaynak grubu dağıtımlarını kullanır.

## <a name="deploy-with-the-rest-api"></a>REST API ile dağıtma

1. Kimlik doğrulama belirteçleri de dahil olmak üzere [ortak parametreleri ve üstbilgi](/rest/api/azure/)ayarlayın.

1. Varolan bir kaynak grubunuz yoksa, bir kaynak grubu oluşturun. Abonelik kimliğinizi, yeni kaynak grubunun adını ve çözümünüz için ihtiyacınız olan konumu sağlayın. Daha fazla bilgi için [bkz.](/rest/api/resources/resourcegroups/createorupdate)

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2019-05-01
   ```

   Gibi bir istek gövdesi ile:

   ```json
   {
    "location": "West US",
    "tags": {
      "tagname1": "tagvalue1"
    }
   }
   ```

1. [Bir şablon dağıtımını doğrula](/rest/api/resources/deployments/validate) işlemini çalıştırarak dağıtımınızı gerçekleştirmeden önce doğrulayın. Dağıtımı sınarken, dağıtımı yürürken tam olarak yaptığınız parametreleri sağlayın (bir sonraki adımda gösterilir).

1. Bir şablon dağıtmak için abonelik kimliğinizi, kaynak grubunun adını, istek URI'sinde dağıtımın adını sağlayın.

   ```HTTP
   PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2019-05-01
   ```

   İstek gövdesinde, şablon ve parametre dosyanıza bir bağlantı sağlayın. Parametre dosyası hakkında daha fazla bilgi için [kaynak yöneticisi parametre dosyası oluştur'a](parameter-files.md)bakın.

   **Modun** Artımlı olarak ayarlıolduğuna dikkat **edin.** Tam bir dağıtım çalıştırmak için **modu** **Tamamla'ya**ayarlayın. Şablonunuzda olmayan kaynakları yanlışlıkla silebileceğiniz için tam modu kullanırken dikkatli olun.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental"
    }
   }
   ```

    Yanıt içeriğini günlüğe kaydetmek istiyorsanız, istek içeriği isteyin veya her ikisi de isteğe **hata ayıklama** ekleyin.

   ```json
   {
    "properties": {
      "templateLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
        "contentVersion": "1.0.0.0"
      },
      "parametersLink": {
        "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
        "contentVersion": "1.0.0.0"
      },
      "mode": "Incremental",
      "debugSetting": {
        "detailLevel": "requestContent, responseContent"
      }
    }
   }
   ```

    Depolama hesabınızı paylaşılan erişim imzası (SAS) belirteci kullanacak şekilde ayarlayabilirsiniz. Daha fazla bilgi için [bkz.](/rest/api/storageservices/delegating-access-with-a-shared-access-signature)

    Bir parametre (parola gibi) için hassas bir değer sağlamanız gerekiyorsa, bu değeri bir anahtar kasasına ekleyin. Önceki örnekte gösterildiği gibi dağıtım sırasında anahtar kasasını alın. Daha fazla bilgi için [dağıtım sırasında güvenli değerleri geçir'e](key-vault-parameter.md)bakın.

1. Şablon ve parametreler için dosyalara bağlanmak yerine, bunları istek gövdesine ekleyebilirsiniz. Aşağıdaki örnek, şablon ve parametre satırlı istek gövdesini gösterir:

   ```json
   {
      "properties": {
      "mode": "Incremental",
      "template": {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
          "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_LRS",
            "allowedValues": [
              "Standard_LRS",
              "Standard_GRS",
              "Standard_ZRS",
              "Premium_LRS"
            ],
            "metadata": {
              "description": "Storage Account type"
            }
          },
          "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
              "description": "Location for all resources."
            }
          }
        },
        "variables": {
          "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
        },
        "resources": [
          {
            "type": "Microsoft.Storage/storageAccounts",
            "apiVersion": "2018-02-01",
            "name": "[variables('storageAccountName')]",
            "location": "[parameters('location')]",
            "sku": {
              "name": "[parameters('storageAccountType')]"
            },
            "kind": "StorageV2",
            "properties": {}
          }
        ],
        "outputs": {
          "storageAccountName": {
            "type": "string",
            "value": "[variables('storageAccountName')]"
          }
        }
      },
      "parameters": {
        "location": {
          "value": "eastus2"
        }
      }
    }
   }
   ```

1. Şablon dağıtımının durumunu almak için [Dağıtımlar '](/rest/api/resources/deployments/get)ı kullanın - Get .

   ```HTTP
   GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2018-05-01
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Bir hata aldığınızda başarılı bir dağıtıma geri dönmek [için, başarılı dağıtımhatasında Rollback'i](rollback-on-error.md)görün.
- Kaynak grubunda bulunan ancak şablonda tanımlanmayan kaynakların nasıl işleyeceğini belirtmek için Azure [Kaynak Yöneticisi dağıtım modlarına](deployment-modes.md)bakın.
- Eşzamanlı REST işlemlerini işleme hakkında bilgi edinmek için eş [senkronize Azure işlemlerini izleyin'e](../management/async-operations.md)bakın.
- Şablonlar hakkında daha fazla bilgi edinmek için [bkz.](template-syntax.md)

