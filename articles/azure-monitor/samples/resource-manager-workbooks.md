---
title: Çalışma kitapları için şablon Kaynak Yöneticisi örnekleri
description: Azure Izleyici çalışma kitaplarını dağıtmaya yönelik örnek Azure Resource Manager şablonları.
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 05/18/2020
ms.openlocfilehash: 72e6e085135c07e962dea6253af7d40333295a86
ms.sourcegitcommit: 1f25aa993c38b37472cf8a0359bc6f0bf97b6784
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83854501"
---
# <a name="resource-manager-template-samples-for-workbooks-in-azure-monitor"></a>Azure Izleyici 'de çalışma kitapları için Kaynak Yöneticisi şablonu örnekleri
Bu makale, Azure Izleyici 'de çalışma kitapları oluşturmak için örnek [Azure Resource Manager şablonlar](../../azure-resource-manager/templates/template-syntax.md) içerir. Her örnek, şablona sağlanacak örnek değerleri içeren bir şablon dosyası ve bir parametre dosyası içerir.

[!INCLUDE [azure-monitor-samples](../../../includes/azure-monitor-resource-manager-samples.md)]

Çalışma kitapları karmaşık olabilir, bu nedenle, çalışma kitabını Azure portal oluşturmak ve ardından bir Kaynak Yöneticisi şablonu oluşturmak için tipik bir stratejidir. [Çalışma kitaplarını dağıtmak için Azure Resource Manager şablonda](../platform/workbooks-automate.md)bu yöntemin ayrıntılarına bakın.

## <a name="create-a-workbook"></a>Çalışma kitabı oluşturma
Aşağıdaki örnek, basit bir çalışma kitabı oluşturur.


### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workbookDisplayName":  {             
            "type":"string",
            "defaultValue": "My Workbook",
            "metadata": {
                "description": "The friendly name for the workbook that is used in the Gallery or Saved List. Needs to be unique in the scope of the resource group and source" 
            }
        },
        "workbookType":  {             
            "type":"string",
            "defaultValue": "tsg",
            "metadata": {
                "description": "The gallery that the workbook will been shown under. Supported values include workbook, `tsg`, Azure Monitor, etc." 
            }
        },
        "workbookSourceId":  {             
            "type":"string",
            "defaultValue": "<insert-your-resource-id-here>",
            "metadata": {
                "description": "The id of resource instance to which the workbook will be associated" 
            }
        },
        "workbookId": {
            "type":"string",
            "defaultValue": "[newGuid()]",
            "metadata": {
                "description": "The unique guid for this workbook instance" 
            }
        }
    },    
    "resources": [
        {
            "name": "[parameters('workbookId')]",
            "type": "Microsoft.Insights/workbooks",
            "location": "[resourceGroup().location]",
            "kind": "shared",
            "apiVersion": "2018-06-17-preview",
            "dependsOn": [],
            "properties": {
                "displayName": "[parameters('workbookDisplayName')]",
                "serializedData": "{\"version\":\"Notebook/1.0\",\"items\":[{\"type\":1,\"content\":\"{\\\"json\\\":\\\"Hello World!\\\"}\",\"conditionalVisibility\":null}],\"isLocked\":false}",
                "version": "1.0",
                "sourceId": "[parameters('workbookSourceId')]",
                "category": "[parameters('workbookType')]"
            }
        }
    ],
    "outputs": {
        "workbookId": {
            "type": "string",
            "value": "[resourceId( 'Microsoft.Insights/workbooks', parameters('workbookId'))]"
        }
    }
}
```

### <a name="parameter-file"></a>Parametre dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workbookDisplayName": {
            "value": "Sample Hello World workbook"
        },
      "workbookType": {
        "value": "workbook"
      },
      "workbookSourceId": {
        "value": "Azure Monitor"
      }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure izleyici için diğer örnek şablonları alın](resource-manager-samples.md).
* [Eylem grupları hakkında daha fazla bilgi edinin](../platform/workbooks-overview.md).
