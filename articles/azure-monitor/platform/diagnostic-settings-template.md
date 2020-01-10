---
title: Kaynak Yöneticisi şablonu kullanarak Azure 'da tanılama ayarı oluşturma
description: Azure platformu günlüklerini Azure Izleyici günlüklerine, Azure depolama 'ya veya Azure Event Hubs iletmek için bir Kaynak Yöneticisi şablonu kullanarak tanılama ayarları oluşturun.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: b549cc0e890a122a04984baa2348831fc51abe08
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "75531012"
---
# <a name="create-diagnostic-setting-in-azure-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak Azure 'da tanılama ayarı oluşturma
Azure Izleyici 'de [Tanılama ayarları](diagnostic-settings.md) Azure kaynakları tarafından toplanan [Platform günlüklerinin](platform-logs-overview.md) ve bağımlı oldukları Azure platformunun nereden gönderileceğini belirtir. Bu makalede, farklı hedeflere platform günlüklerini toplamak için tanılama ayarlarını oluşturmak ve yapılandırmak üzere bir [Azure Resource Manager şablonu](../../azure-resource-manager/resource-group-authoring-templates.md) kullanmak için Ayrıntılar ve örnekler sağlanmaktadır. 

> [!NOTE]
> Diğer Azure kaynakları için Tanılama ayarları gibi PowerShell veya CLı kullanarak Azure etkinlik günlüğü için [bir tanılama ayarı oluşturamıyoruz](diagnostic-settings.md) , bu makaledeki bilgileri kullanarak etkinlik günlüğü için bir kaynak yöneticisi şablonu oluşturun ve şablonu POWERSHELL veya CLI kullanarak dağıtın.

## <a name="deployment-methods"></a>Dağıtım yöntemleri
PowerShell ve CLı dahil geçerli herhangi bir yöntemi kullanarak Kaynak Yöneticisi şablonlarını dağıtabilirsiniz. Etkinlik günlüğü için tanılama ayarlarının, CLı için `az deployment create` veya PowerShell için `New-AzDeployment` kullanarak bir aboneliğe dağıtılması gerekir. Kaynak günlükleri için tanılama ayarlarının, CLı için `az group deployment create` veya PowerShell için `New-AzResourceGroupDeployment` kullanarak bir kaynak grubuna dağıtılması gerekir. 

Ayrıntılar için bkz. [Kaynak Yöneticisi şablonlarıyla kaynakları dağıtma ve Azure PowerShell](../../azure-resource-manager/resource-group-template-deploy.md) ve [Kaynak Yöneticisi ŞABLONLARı ve Azure CLI ile kaynak dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md) . 





## <a name="resource-logs"></a>Kaynak günlükleri
Kaynak günlükleri için, şablona `<resource namespace>/providers/diagnosticSettings` türünde bir kaynak ekleyin. Özellikler bölümü, [Tanılama ayarları-oluştur veya Güncelleştir](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate)bölümünde açıklanan biçimi izler. Toplamak istediğiniz kaynak için geçerli kategorilerin her biri için `logs` bölümünde bir `category` sağlayın. [Kaynak ölçümlerini destekliyorsa](metrics-supported.md), kaynak ölçümlerini aynı hedeflere toplamak için `metrics` özelliğini ekleyin.

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
Azure etkinlik günlüğü için `Microsoft.Insights/diagnosticSettings`türünde bir kaynak ekleyin. Kullanılabilir Kategoriler, [etkinlik günlüğündeki kategorilerde](activity-log-view.md#categories-in-the-activity-log)listelenmiştir. Aşağıda, tüm etkinlik günlüğü kategorilerini bir Log Analytics çalışma alanına, depolama hesabına ve Olay Hub 'ına toplayan bir şablon verilmiştir.


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
