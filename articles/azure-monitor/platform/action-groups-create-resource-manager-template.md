---
title: Kaynak Yöneticisi şablonları ile eylem grupları oluşturma
description: Azure Kaynak Yöneticisi şablonu kullanarak eylem grubu oluşturmayı öğrenin.
author: dkamstra
services: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 50ad9d57b24fab9ee57c2f9caae8f4c39d2681f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669089"
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu içeren bir eylem grubu oluşturma
Bu makalede, eylem gruplarını yapılandırmak için [Azure Kaynak Yöneticisi şablonu](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authoring-templates) nasıl kullanılacağı gösterilmektedir. Şablonları kullanarak, belirli uyarı türlerinde yeniden kullanılabilen eylem grupları otomatik olarak ayarlayabilirsiniz. Bu eylem grupları, bir uyarı tetiklendiğinde tüm doğru taraflara bildirilir.

Temel adımlar şunlardır:

1. Eylem grubunun nasıl oluşturulurulur açıklayan bir JSON dosyası olarak şablon oluşturun.

2. [Herhangi bir dağıtım yöntemini](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)kullanarak şablonu dağıtın.

İlk olarak, eylem tanımlarının şablonda sabit kodlandığı bir eylem grubu için Kaynak Yöneticisi şablonu nasıl oluşturulacağını açıklarız. İkinci olarak, şablon dağıtıldığında webhook yapılandırma bilgilerini giriş parametreleri olarak alan bir şablonun nasıl oluşturulabileceğimizi açıklarız.

## <a name="resource-manager-templates-for-an-action-group"></a>Eylem grubu için Kaynak Yöneticisi şablonları

Kaynak Yöneticisi şablonu kullanarak bir eylem grubu oluşturmak için, türünün `Microsoft.Insights/actionGroups`bir kaynak oluşturursunuz. Sonra ilgili tüm özellikleri doldurun. Burada, bir eylem grubu oluşturan iki örnek şablon verilmiştir.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
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

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "actionGroupName": {
      "type": "string",
      "metadata": {
        "description": "Unique name (within the Resource Group) for the Action group."
      }
    },
    "actionGroupShortName": {
      "type": "string",
      "metadata": {
        "description": "Short name (maximum 12 characters) for the Action group."
      }
    },
    "webhookReceiverName": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service Name."
      }
    },    
    "webhookServiceUri": {
      "type": "string",
      "metadata": {
        "description": "Webhook receiver service URI."
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
        ],
        "emailReceivers": [
        ],
        "webhookReceivers": [
          {
            "name": "[parameters('webhookReceiverName')]",
            "serviceUri": "[parameters('webhookServiceUri')]"
          }
        ]
      }
    }
  ],
  "outputs":{
      "actionGroupResourceId":{
          "type":"string",
          "value":"[resourceId('Microsoft.Insights/actionGroups',parameters('actionGroupName'))]"
      }
  }
}
```


## <a name="next-steps"></a>Sonraki adımlar
* [Eylem grupları](../../azure-monitor/platform/action-groups.md)hakkında daha fazla bilgi edinin.
* [Uyarılar](alerts-overview.md)hakkında daha fazla bilgi edinin.
* Kaynak Yöneticisi [şablonu kullanarak nasıl uyarı ekleyeceğinizi](../../azure-monitor/platform/alerts-activity-log.md)öğrenin.

