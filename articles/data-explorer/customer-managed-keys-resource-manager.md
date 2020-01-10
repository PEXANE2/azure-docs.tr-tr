---
title: Azure Resource Manager şablonunu kullanarak Azure Veri Gezgini 'de müşteri tarafından yönetilen anahtarlar yapılandırma
description: Bu makalede, Azure Resource Manager şablonunu kullanarak Azure Veri Gezgini verilerinizde müşteri tarafından yönetilen anahtarlar şifrelemesini yapılandırma açıklanmaktadır.
author: saguiitay
ms.author: itsagui
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: bff8c50cdece803e030c0975a9b14ac5739baaf7
ms.sourcegitcommit: 02160a2c64a5b8cb2fb661a087db5c2b4815ec04
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75725812"
---
# <a name="configure-customer-managed-keys-using-the-azure-resource-manager-template"></a>Azure Resource Manager şablonunu kullanarak müşteri tarafından yönetilen anahtarları yapılandırma

> [!div class="op_single_selector"]
> * [C#](create-cluster-database-csharp.md)
> * [Azure Resource Manager şablonu](create-cluster-database-resource-manager.md)

[!INCLUDE [data-explorer-configure-customer-managed-keys](../../includes/data-explorer-configure-customer-managed-keys.md)]

## <a name="configure-encryption-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma

Bu bölümde, müşteri tarafından yönetilen anahtarları Azure Resource Manager şablonları kullanarak yapılandırırsınız. Azure Veri Gezgini şifrelemesi, varsayılan olarak Microsoft tarafından yönetilen anahtarları kullanır. Bu adımda, Azure Veri Gezgini kümenizi müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırın ve kümeyle ilişkilendirilecek anahtarı belirtin.

Azure portal veya PowerShell 'i kullanarak Azure Resource Manager şablonu dağıtabilirsiniz.

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

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, yeni sürümü kullanmak için kümeyi güncelleştirmeniz gerekir. İlk olarak, anahtarın en son sürümünü almak için `Get-AzKeyVaultKey` çağırın. Ardından, [müşteri tarafından yönetilen anahtarlarla şifrelemeyi yapılandırma](#configure-encryption-with-customer-managed-keys)bölümünde gösterildiği gibi, kümenin Anahtar Kasası özelliklerini anahtarın yeni sürümünü kullanacak şekilde güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da Azure Veri Gezgini kümelerini güvenli hale getirme](security.md)
* [Azure Veri Gezgini kümeniz için Yönetilen kimlikler yapılandırma](managed-identities.md)
* Rest 'de şifrelemeyi etkinleştirerek [Azure Veri Gezgini Azure Portal kümenizin güvenliğini sağlayın](manage-cluster-security.md) .
* [Müşteri tarafından yönetilen anahtarları kullanarak yapılandırmaC#](customer-managed-keys-csharp.md)

