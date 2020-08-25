---
title: Eylem grupları için şablon örnekleri Kaynak Yöneticisi
description: Azure Izleyici eylem gruplarını dağıtmaya yönelik örnek Azure Resource Manager şablonları.
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 05/18/2020
ms.openlocfilehash: fda9a7e39f1bb103a203cdfa61a5044c51c692f2
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2020
ms.locfileid: "83854375"
---
# <a name="resource-manager-template-samples-for-action-groups-in-azure-monitor"></a>Azure Izleyici 'de eylem grupları için Kaynak Yöneticisi şablonu örnekleri
Bu makale, Azure Izleyici 'de [eylem grupları](../platform/action-groups.md) oluşturmak için örnek [Azure Resource Manager şablonlar](../../azure-resource-manager/templates/template-syntax.md) içerir. Her örnek, şablona sağlanacak örnek değerleri içeren bir şablon dosyası ve bir parametre dosyası içerir.

[!INCLUDE [azure-monitor-samples](../../../includes/azure-monitor-resource-manager-samples.md)]

## <a name="create-an-action-group"></a>Bir eylem grubu oluşturun
Aşağıdaki örnek bir eylem grubu oluşturur.


### <a name="template-file"></a>Şablon dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name within the resource group for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name up to 12 characters for the Action group."
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/actionGroups",
      "apiVersion": "2018-03-01",
      "name": "[parameters('actionGroupName')]",
      "location": "Global",
      "properties": {
        "groupShortName": "[parameters('actionGroupShortName')]",
        "enabled": true,
        "smsReceivers": [
          {
            "name": "contosoSMS",
            "countryCode": "1",
            "phoneNumber": "5555551212"
          },
          {
            "name": "contosoSMS2",
            "countryCode": "1",
            "phoneNumber": "5555552121"
          }
        ],
        "emailReceivers": [
          {
            "name": "contosoEmail",
            "emailAddress": "devops@contoso.com"
          },
          {
            "name": "contosoEmail2",
            "emailAddress": "devops2@contoso.com"
          }
        ],
        "webhookReceivers": [
          {
            "name": "contosoHook",
            "serviceUri": "http://requestb.in/1bq62iu1"
          },
          {
            "name": "contosoHook2",
            "serviceUri": "http://requestb.in/1bq62iu2"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
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
      "actionGroupName": {
        "value": "My Action Group"
      },
      "actionGroupShortName": {
        "value": "mygroup"
      }
  }
}
```



## <a name="next-steps"></a>Sonraki adımlar

* [Azure izleyici için diğer örnek şablonları alın](resource-manager-samples.md).
* [Eylem grupları hakkında daha fazla bilgi edinin](../platform/action-groups.md).

