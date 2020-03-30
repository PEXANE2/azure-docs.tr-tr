---
title: Azure Kaynak Yöneticisi şablonu kullanarak Azure Veri Gezgini'nde müşteri tarafından yönetilen anahtarları yapılandırın
description: Bu makalede, Azure Kaynak Yöneticisi şablonunu kullanarak Azure Veri Gezgini'nde verilerinizde müşteri tarafından yönetilen anahtar şifrelemesinin nasıl yapılandırılabildiğini açıklanmaktadır.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: 454a80089b5f74d4a70015ffcd03d0212e8c08a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80297892"
---
# <a name="configure-customer-managed-keys-using-the-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonunu kullanarak müşteri tarafından yönetilen anahtarları yapılandırın

> [!div class="op_single_selector"]
> * [Portal](customer-managed-keys-portal.md)
> * [C #](customer-managed-keys-csharp.md)
> * [Azure Resource Manager şablonu](customer-managed-keys-resource-manager.md)

[!INCLUDE [data-explorer-configure-customer-managed-keys](../../includes/data-explorer-configure-customer-managed-keys.md)]

[!INCLUDE [data-explorer-configure-customer-managed-keys part 2](../../includes/data-explorer-configure-customer-managed-keys-b.md)]

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma

Bu bölümde, Azure Kaynak Yöneticisi şablonlarını kullanarak müşteri tarafından yönetilen anahtarları yapılandırAbilirsiniz. Varsayılan olarak, Azure Veri Gezgini şifrelemesi Microsoft tarafından yönetilen anahtarları kullanır. Bu adımda, Azure Veri Gezgini kümenizi müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırın ve kümeyle ilişkilendirecek anahtarı belirtin.

Azure portalını kullanarak veya PowerShell'i kullanarak Azure Kaynak Yöneticisi şablonuna dağıtabilirsiniz.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "clusterName": {
      "type": "string",
      "defaultValue": "[concat('kusto', uniqueString(resourceGroup().id))]",
      "metadata": {
        "description": "Name of the cluster to create"
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
  "variables": {},
  "resources": [
    {
      "name": "[parameters('clusterName')]",
      "type": "Microsoft.Kusto/clusters",
      "sku": {
        "name": "Standard_D13_v2",
        "tier": "Standard",
        "capacity": 2
      },
      "apiVersion": "2019-09-07",
      "location": "[parameters('location')]",
      "properties": {
        "keyVaultProperties": {
          "keyVaultUri": "<keyVaultUri>",
          "keyName": "<keyName>",
          "keyVersion": "<keyVersion"
        }
      }
    }
  ]
}
```

## <a name="update-the-key-version"></a>Anahtar sürümünü güncelleştirme

Anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için kümeyi güncelleştirmeniz gerekir. İlk olarak, anahtarın en son sürümünü almak için arayın. `Get-AzKeyVaultKey` Daha [sonra, müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandır'da](#configure-encryption-with-customer-managed-keys)gösterildiği gibi anahtarın yeni sürümünü kullanmak için kümenin anahtar kasa özelliklerini güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da Güvenli Azure Veri Gezgini kümeleri](security.md)
* [Azure Veri Gezgini kümeniz için yönetilen kimlikleri yapılandırma](managed-identities.md)
* Şifrelemeyi hazır layarak [Azure Veri Gezgini - Azure portalında kümenizi güvenli](manage-cluster-security.md) hale verin.
* [C kullanarak müşteri tarafından yönetilen anahtarları yapılandırma #](customer-managed-keys-csharp.md)

