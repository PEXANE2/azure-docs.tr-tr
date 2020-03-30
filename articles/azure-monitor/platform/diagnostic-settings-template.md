---
title: Kaynak Yöneticisi şablonu kullanarak Azure'da tanılama ayarı oluşturma
description: Azure platform günlüklerini Azure Monitör Günlükleri, Azure depolama alanı veya Azure Etkinlik Hub'larına iletmek için Kaynak Yöneticisi şablonu kullanarak tanı lama ayarları oluşturun.
author: bwren
services: azure-monitor
ms.topic: conceptual
ms.date: 12/13/2019
ms.author: bwren
ms.subservice: ''
ms.openlocfilehash: a2569ca3f998030680bd7dbd872d71ccd372a25d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77672438"
---
# <a name="create-diagnostic-setting-in-azure-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak Azure'da tanılama ayarı oluşturma
Azure Monitor'daki [tanıayarları,](diagnostic-settings.md) Azure kaynakları ve bağlı oldukları Azure platformu tarafından toplanan [Platform günlüklerinin](platform-logs-overview.md) nereye gönderilen yeri belirtir. Bu makalede, farklı hedeflere platform günlükleri toplamak için tanılama ayarları oluşturmak ve yapılandırmak için bir [Azure Kaynak Yöneticisi şablonu](../../azure-resource-manager/templates/template-syntax.md) kullanmak için ayrıntılar ve örnekler sağlar.

> [!NOTE]
> Diğer Azure kaynakları için tanı ayarları gibi PowerShell veya CLI kullanarak Azure Etkinliği günlüğü için [bir tanılama ayarı oluşturamadığınız](diagnostic-settings.md) için, bu makaledeki bilgileri kullanarak Etkinlik günlüğü için bir Kaynak Yöneticisi şablonu oluşturun ve şablonu PowerShell veya CLI kullanarak dağıtın.

## <a name="deployment-methods"></a>Dağıtım yöntemleri
PowerShell ve CLI dahil olmak üzere geçerli bir yöntemi kullanarak Kaynak Yöneticisi şablonlarını dağıtabilirsiniz. Etkinlik günlüğü için tanı ayarları CLI veya `az deployment create` `New-AzDeployment` PowerShell için kullanarak bir aboneye dağıtılmalıdır. Kaynak günlükleri için tanı lama ayarları, `az group deployment create` CLI `New-AzResourceGroupDeployment` veya PowerShell için kullanan bir kaynak grubuna dağıtılmalıdır.

Ayrıntılar [için Kaynak Yöneticisi şablonları ve Azure PowerShell](../../azure-resource-manager/templates/deploy-powershell.md) ile kaynakları dağıt ve Kaynak Yöneticisi [şablonları ve Azure CLI ile kaynakları dağıtın.](../../azure-resource-manager/templates/deploy-cli.md) 





## <a name="resource-logs"></a>Kaynak günlükleri
Kaynak günlükleri için şablona `<resource namespace>/providers/diagnosticSettings` bir tür kaynağı ekleyin. Özellikler bölümü, [Tanılama Ayarları - Oluştur veya Güncelleştir'de](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings/createorupdate)açıklanan biçimi izler. Toplamak `category` istediğiniz `logs` kaynak için geçerli kategorilerin her biri için bölümde bir tane sağlayın. Kaynak `metrics` [ölçümleri](metrics-supported.md)destekliyorsa, aynı hedeflere kaynak ölçümleri toplamak için özelliği ekleyin.

Aşağıda, belirli bir kaynak için bir Günlük Analizi çalışma alanına, depolama hesabına ve olay hub'ına kaynak günlüğü kategorisi toplayan bir şablon izlenir.

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
Aşağıda, kaynak günlüklerinin bir olay hub'ına, bir depolama hesabına ve Günlük Analizi çalışma alanına akışını sağlayan otomatik ölçeklendirme ayarı için tanılama ayarı oluşturan bir örnek verilmiştir.

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
Azure Etkinliği günlüğü için bir kaynak `Microsoft.Insights/diagnosticSettings`ekleyin. Kullanılabilir kategoriler Etkinlik [Günlüğündeki Kategoriler'de](activity-log-view.md#categories-in-the-activity-log)listelenir. Aşağıda, tüm Etkinlik günlüğü kategorilerini Bir Günlük Analizi çalışma alanı, depolama hesabı ve etkinlik hub'ına toplayan bir şablon izlenir.


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
* [Azure'daki platform günlükleri](platform-logs-overview.md)hakkında daha fazla bilgi edinin.
* [Tanılama ayarları](diagnostic-settings.md)hakkında bilgi edinin.
