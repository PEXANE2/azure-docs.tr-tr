---
title: Kaynak Yöneticisi şablonu kullanarak Azure 'da tanılama ayarı oluşturma
description: Azure platformu günlüklerini Azure Izleyici günlüklerine, Azure depolama 'ya veya Azure Event Hubs iletmek için bir Kaynak Yöneticisi şablonu kullanarak tanılama ayarları oluşturun.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: a2569ca3f998030680bd7dbd872d71ccd372a25d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77672438"
---
# <a name="create-diagnostic-setting-in-azure-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak Azure 'da tanılama ayarı oluşturma
Azure Izleyici 'de [Tanılama ayarları](diagnostic-settings.md) Azure kaynakları tarafından toplanan [Platform günlüklerinin](platform-logs-overview.md) ve bağımlı oldukları Azure platformunun nereden gönderileceğini belirtir. Bu makalede, farklı hedeflere platform günlüklerini toplamak için tanılama ayarlarını oluşturmak ve yapılandırmak üzere bir [Azure Resource Manager şablonu](../../azure-resource-manager/templates/template-syntax.md) kullanmak için Ayrıntılar ve örnekler sağlanmaktadır.

> [!NOTE]
> Diğer Azure kaynakları için Tanılama ayarları gibi PowerShell veya CLı kullanarak Azure etkinlik günlüğü için [bir tanılama ayarı oluşturamıyoruz](diagnostic-settings.md) , bu makaledeki bilgileri kullanarak etkinlik günlüğü için bir kaynak yöneticisi şablonu oluşturun ve şablonu POWERSHELL veya CLI kullanarak dağıtın.

## <a name="deployment-methods"></a>Dağıtım yöntemleri
PowerShell ve CLı dahil geçerli herhangi bir yöntemi kullanarak Kaynak Yöneticisi şablonlarını dağıtabilirsiniz. Etkinlik günlüğü için tanılama ayarlarının CLı veya `az deployment create` `New-AzDeployment` PowerShell için kullanarak bir aboneliğe dağıtılması gerekir. Kaynak günlükleri için Tanılama ayarları, CLı veya `az group deployment create` `New-AzResourceGroupDeployment` PowerShell için kullanarak bir kaynak grubuna dağıtmalıdır.

Ayrıntılar için bkz. [Kaynak Yöneticisi şablonlarıyla kaynakları dağıtma ve Azure PowerShell](../../azure-resource-manager/templates/deploy-powershell.md) ve [Kaynak Yöneticisi ŞABLONLARı ve Azure CLI ile kaynak dağıtma](../../azure-resource-manager/templates/deploy-cli.md) . 





## <a name="resource-logs"></a>Kaynak günlükleri
Kaynak günlükleri için, şablona türünde `<resource namespace>/providers/diagnosticSettings` bir kaynak ekleyin. Özellikler bölümü, [Tanılama ayarları-oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate)bölümünde açıklanan biçimi izler. Toplamak istediğiniz `category` kaynak için `logs` geçerli kategorilerin her biri için bölümünde bir sağlar. Kaynak ölçümlerini `metrics` [destekliyorsa](metrics-supported.md), kaynak ölçümlerini aynı hedeflere toplayacak özelliği ekleyin.

Aşağıda, belirli bir kaynak için bir Log Analytics çalışma alanına, depolama hesabına ve Olay Hub 'ına bir kaynak günlüğü kategorisi toplayan bir şablon verilmiştir.

```json
"resources": [
  {
    "type": "/<resource namespace>/providers/diagnosticSettings",
    "name": "[concat(parameters('resourceName'),'/microsoft.insights/', parameters('settingName'))]",
    "dependsOn": [
      "[<resource Id for which resource logs will be enabled>]"
    ],
    "apiVersion": "2017-05-01-preview",
    "properties": {
      "name": "[parameters('settingName')]",
      "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName'))]",
      "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
      "eventHubName": "[parameters('eventHubName')]",
      "workspaceId": "[parameters('workspaceId')]",
      "logs": [
        {
          "category": "<category name>",
          "enabled": true
        }
      ],
      "metrics": [
        {
          "category": "AllMetrics",
          "enabled": true
        }
      ]
    }
  }
]
```



### <a name="example"></a>Örnek
Aşağıda, kaynak günlüklerinin bir olay hub 'ına, depolama hesabına ve Log Analytics çalışma alanına akışını sağlayan bir otomatik ölçeklendirme ayarı için bir tanılama ayarı oluşturan bir örnek verilmiştir.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "autoscaleSettingName": {
            "type": "string",
            "metadata": {
                "description": "The name of the autoscale setting"
            }
        },
        "settingName": {
            "type": "string",
            "metadata": {
                "description": "The name of the diagnostic setting"
            }
        },
        "workspaceId": {
            "type": "string",
            "metadata": {
                "description": "ResourceIDl of the Log Analytics workspace in which resource logs should be saved."
            }
        },
        "storageAccountId": {
            "type": "string",
            "metadata": {
              "description": "ResourceID of the Storage Account in which resource logs should be saved."
            }
        },
        "eventHubAuthorizationRuleId": {
            "type": "string",
            "metadata": {
              "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
            }
        },
        "eventHubName": {
            "type": "string",
            "metadata": {
                "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
            }
        }
    },
    "variables": {},
    "resources": [
    {
      "type": "microsoft.insights/autoscalesettings/providers/diagnosticSettings",
      "apiVersion": "2017-05-01-preview",
      "name": "[concat(parameters('autoscaleSettingName'),'/microsoft.insights/', parameters('settingName'))]",
      "dependsOn": [
        "[resourceId('Microsoft.Insights/autoscalesettings', parameters('autoscaleSettingName'))]"
      ],
      "properties": {
        "workspaceId": "[parameters('workspaceId')]",
        "storageAccountId": "[parameters('storageAccountId')]",
        "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
        "eventHubName": "[parameters('eventHubName')]",
        "logs": [
          {
            "category": "AutoscaleScaleActions",
            "enabled": true
          },
          {
            "category": "AutoscaleEvaluations",
            "enabled": true
          }
        ]
      }
    }
  ]
}
```

## <a name="activity-log"></a>Etkinlik günlüğü
Azure etkinlik günlüğü için türünde `Microsoft.Insights/diagnosticSettings`bir kaynak ekleyin. Kullanılabilir Kategoriler, [etkinlik günlüğündeki kategorilerde](activity-log-view.md#categories-in-the-activity-log)listelenmiştir. Aşağıda, tüm etkinlik günlüğü kategorilerini bir Log Analytics çalışma alanına, depolama hesabına ve Olay Hub 'ına toplayan bir şablon verilmiştir.


```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "settingName": {
            "type": "string",
            "metadata": {
                "description": "The name of the diagnostic setting"
            }
        },
        "workspaceId": {
            "type": "string",
            "metadata": {
                "description": "ResourceID of the Log Analytics workspace in which resource logs should be saved."
            }
        },
        "storageAccountId": {
            "type": "string",
            "metadata": {
              "description": "ResourceID of the Storage Account in which resource logs should be saved."
            }
        },
        "eventHubAuthorizationRuleId": {
            "type": "string",
            "metadata": {
              "description": "Resource ID of the event hub authorization rule for the Event Hubs namespace in which the event hub should be created or streamed to."
            }
        },
        "eventHubName": {
            "type": "string",
            "metadata": {
                "description": "Optional. Name of the event hub within the namespace to which logs are streamed. Without this, an event hub is created for each log category."
            }
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Insights/diagnosticSettings",
            "apiVersion": "2017-05-01-preview",
            "name": "[parameters('settingName')]",
            "location": "global",
            "properties": {
                "workspaceId": "[parameters('workspaceId')]",
                "storageAccountId": "[parameters('storageAccountId')]",
                "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
                "eventHubName": "[parameters('eventHubName')]",
                "logs": [
                    {
                        "category": "Administrative",
                        "enabled": true
                    },
                    {
                        "category": "Security",
                        "enabled": true
                    },
                    {
                        "category": "ServiceHealth",
                        "enabled": true
                    },
                    {
                        "category": "Alert",
                        "enabled": true
                    },
                    {
                        "category": "Recommendation",
                        "enabled": true
                    },
                    {
                        "category": "Policy",
                        "enabled": true
                    },
                    {
                        "category": "Autoscale",
                        "enabled": true
                    },
                    {
                        "category": "ResourceHealth",
                        "enabled": true
                    }
                ]
            }
        }
    ]
}
```


## <a name="next-steps"></a>Sonraki adımlar
* [Azure 'da platform günlükleri](platform-logs-overview.md)hakkında daha fazla bilgi edinin.
* [Tanılama ayarları](diagnostic-settings.md)hakkında bilgi edinin.
