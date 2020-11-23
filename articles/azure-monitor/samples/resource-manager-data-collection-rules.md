---
title: Veri toplama kuralları için Kaynak Yöneticisi şablonu örnekleri
description: Azure Izleyici 'de veri toplama kuralları ve sanal makineler arasında ilişkiler oluşturmak için örnek Azure Resource Manager şablonları.
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 11/17/2020
ms.openlocfilehash: 1c059edb1422a572011f167f7f1c02d5e87e5da2
ms.sourcegitcommit: 5ae2f32951474ae9e46c0d46f104eda95f7c5a06
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2020
ms.locfileid: "95324879"
---
# <a name="resource-manager-template-samples-for-data-collection-rules-in-azure-monitor"></a>Azure Izleyici 'de veri toplama kuralları için Kaynak Yöneticisi şablonu örnekleri
Bu makalede, Azure Izleyici 'de sanal makineler için [Log Analytics aracısını](../platform/log-analytics-agent.md) ve [Tanılama uzantısını](../platform/diagnostics-extension-overview.md) dağıtmak ve yapılandırmak üzere örnek [Azure Resource Manager şablonlar](../../azure-resource-manager/templates/template-syntax.md) bulunur. Her örnek, şablona sağlanacak örnek değerleri içeren bir şablon dosyası ve bir parametre dosyası içerir.

[!INCLUDE [azure-monitor-samples](../../../includes/azure-monitor-resource-manager-samples.md)]


## <a name="create-association-between-azure-vm-and-rule"></a>Azure VM ile kural arasında ilişki oluşturma

Aşağıdaki örnek Azure Izleyici aracısını bir Windows Azure sanal makinesine yüklemektedir. Bir Azure sanal makinesi ve bir veri toplama kuralı arasında bir ilişki oluşturulur.

### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string",
            "metadata": {
                "description": "Name of the virtual machine."
            }
        },
        "associationName": {
            "type": "string",
            "metadata": {
                "description": "Name of the association."
            }
        },
        "dataCollectionRuleId": {
            "type": "string",
            "metadata": {
                "description": "Resource ID of the data collection rule."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.Compute/virtualMachines/providers/dataCollectionRuleAssociations",
            "name": "[concat(parameters('vmName'),'/microsoft.insights/', parameters('associationName'))]",
            "apiVersion": "2019-11-01-preview",
            "properties": {
                "description": "Association of data collection rule. Deleting this association will break the data collection for this virtual machine.",
                "dataCollectionRuleId": "[parameters('dataCollectionRuleId')]"
            }
        }
    ]
}
```

### <a name="parameter-file"></a>Parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "vmName": {
        "value": "my-windows-vm"
      },
      "location": {
        "value": "eastus"
      }
  }
}
```

## <a name="create-association-between-azure-arc-and-rule"></a>Azure Arc ve kuralıyla ilişki oluşturma

Aşağıdaki örnek Azure Izleyici aracısını bir Windows Azure sanal makinesine yüklemektedir. Azure Arc etkin sunucu makinesi ve bir veri toplama kuralı arasında bir ilişki oluşturulur.

### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "vmName": {
            "type": "string",
            "metadata": {
                "description": "Name of the virtual machine."
            }
        },
        "associationName": {
            "type": "string",
            "metadata": {
                "description": "Name of the association."
            }
        },
        "dataCollectionRuleId": {
            "type": "string",
            "metadata": {
                "description": "Resource ID of the data collection rule."
            }
        }
    },
    "resources": [
        {
            "type": "Microsoft.HybridCompute/machines/providers/dataCollectionRuleAssociations",
            "name": "[concat(parameters('machineName'),'/microsoft.insights/', parameters('associationName'))]",
            "apiVersion": "2019-11-01-preview",
            "properties": {
                "description": "Association of data collection rule. Deleting this association will break the data collection for this Arc server.",
                "dataCollectionRuleId": "[parameters('dataCollectionRuleId')]"
            }
        }
    ]
}
```

### <a name="parameter-file"></a>Parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "vmName": {
        "value": "my-windows-vm"
      },
      "location": {
        "value": "eastus"
      }
  }
}
```


## <a name="next-steps"></a>Sonraki adımlar

* [Azure izleyici için diğer örnek şablonları alın](resource-manager-samples.md).
* [Log Analytics Aracısı hakkında daha fazla bilgi edinin](../platform/log-analytics-agent.md).
* [Tanılama uzantısı hakkında daha fazla bilgi edinin](../platform/diagnostics-extension-overview.md).
