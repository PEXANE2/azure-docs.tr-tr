---
title: Kaynak Yöneticisi şablonlarıyla eylem grupları oluşturma
description: Azure Resource Manager şablonu kullanarak bir eylem grubu oluşturmayı öğrenin.
author: dkamstra
services: azure-monitor
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: dukek
ms.subservice: alerts
ms.openlocfilehash: 5c02812d4c97b94667fcddcb275243e7a9b36b29
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87321913"
---
# <a name="create-an-action-group-with-a-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla bir eylem grubu oluşturma
Bu makalede, eylem gruplarını yapılandırmak için bir [Azure Resource Manager şablonunun](../../azure-resource-manager/templates/template-syntax.md) nasıl kullanılacağı gösterilmektedir. Şablonları kullanarak, belirli uyarı türlerinde yeniden kullanılabilen eylem gruplarını otomatik olarak ayarlayabilirsiniz. Bu eylem grupları, bir uyarı tetiklendiğinde tüm doğru taraflara bildirimde bulunduğundan emin olur.

Temel adımlar şunlardır:

1. Eylem grubunun nasıl oluşturulacağını açıklayan bir JSON dosyası olarak şablon oluşturun.

2. [Herhangi bir dağıtım yöntemini](../../azure-resource-manager/templates/deploy-powershell.md)kullanarak şablonu dağıtın.

İlk olarak, eylem tanımlarının şablonda sabit kodlandığı bir eylem grubu için bir Kaynak Yöneticisi şablonu oluşturma işlemi anlatılmaktadır. İkincisi, şablon dağıtıldığında, Web kancası yapılandırma bilgilerini giriş parametresi olarak alan bir şablonun nasıl oluşturulacağını açıklamaktadır.

## <a name="resource-manager-templates-for-an-action-group"></a>Eylem grubu için Kaynak Yöneticisi şablonları

Kaynak Yöneticisi şablonu kullanarak bir eylem grubu oluşturmak için, türünde bir kaynak oluşturursunuz `Microsoft.Insights/actionGroups` . Ardından ilgili tüm özellikleri doldurursunuz. Bir eylem grubu oluşturan iki örnek şablon aşağıda verilmiştir.

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
* [Eylem grupları](./action-groups.md)hakkında daha fazla bilgi edinin.
* [Uyarılar](alerts-overview.md)hakkında daha fazla bilgi edinin.
* [Kaynak Yöneticisi şablonu kullanarak uyarı](./alerts-activity-log.md)eklemeyi öğrenin.

