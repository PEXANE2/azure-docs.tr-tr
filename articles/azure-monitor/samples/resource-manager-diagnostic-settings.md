---
title: Tanılama ayarları için şablon örnekleri Kaynak Yöneticisi
description: Azure Izleyici tanılama ayarlarını bir Azure kaynağına uygulamak için örnek Azure Resource Manager şablonları.
ms.subservice: logs
ms.topic: sample
author: bwren
ms.author: bwren
ms.date: 09/11/2020
ms.openlocfilehash: 4b2727cfdc31dca509a4c8416010bbb40d2e46bb
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90055427"
---
# <a name="resource-manager-template-samples-for-diagnostic-settings-in-azure-monitor"></a>Azure Izleyici 'de Tanılama ayarları için şablon örnekleri Kaynak Yöneticisi
Bu makale, bir Azure kaynağı için Tanılama ayarları oluşturmak üzere örnek [Azure Resource Manager şablonları](../../azure-resource-manager/templates/template-syntax.md) içerir. Her örnek, şablona sağlanacak örnek değerleri içeren bir şablon dosyası ve bir parametre dosyası içerir.

Bir Azure kaynağı için bir tanılama ayarı oluşturmak için, şablona türünde bir kaynak ekleyin `<resource namespace>/providers/diagnosticSettings` . Bu makalede bazı kaynak türleri için örnekler sunulmaktadır, ancak aynı model diğer kaynak türlerine uygulanabilir. İzin verilen Günlükler ve ölçümler koleksiyonu her bir kaynak türü için farklılık gösterecektir.

[!INCLUDE [azure-monitor-samples](../../../includes/azure-monitor-resource-manager-samples.md)]

## <a name="diagnostic-setting-for-activity-log"></a>Etkinlik günlüğü için tanılama ayarı
Aşağıdaki örnek, şablona bir tür kaynağı ekleyerek etkinlik günlüğü için bir tanılama ayarı oluşturur `Microsoft.Insights/diagnosticSettings` .

> [!IMPORTANT]
> Etkinlik günlükleri için Tanılama ayarları, Azure kaynakları için ayarlar gibi bir kaynak grubu için değil, bir abonelik için oluşturulur. Kaynak yönetimi şablonunu dağıtmak için `New-AzSubscriptionDeployment` PowerShell veya Azure CLI için kullanın `az deployment sub create` .

### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "settingName": {
          "type": "String"
        },
        "workspaceId": {
          "type": "String"
        },
        "storageAccountId": {
          "type": "String"
        },
        "eventHubAuthorizationRuleId": {
          "type": "String"
        },
        "eventHubName": {
          "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Insights/diagnosticSettings",
            "apiVersion": "2017-05-01-preview",
            "name": "[parameters('settingName')]",
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

### <a name="parameter-file"></a>Parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "settingName": {
        "value": "Send to all locations"
      },
      "workspaceId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/MyResourceGroup/providers/microsoft.operationalinsights/workspaces/MyWorkspace"
      },
      "storageAccountId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
      },
      "eventHubAuthorizationRuleId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.EventHub/namespaces/MyNameSpace/authorizationrules/RootManageSharedAccessKey"
      },
      "eventHubName": {
        "value": "my-eventhub"
      }
  }
}
```


## <a name="diagnostic-setting-for-azure-key-vault"></a>Azure Key Vault için tanılama ayarı 
Aşağıdaki örnek, şablona türünde bir kaynak ekleyerek bir Azure Key Vault için tanılama ayarı oluşturur `Microsoft.KeyVault/vaults/providers/diagnosticSettings` .

### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "settingName": {
            "type": "String"
        },
        "vaultName": {
            "type": "String"
        },
        "workspaceId": {
            "type": "String"
        },
        "storageAccountId": {
            "type": "String"
        },
        "eventHubAuthorizationRuleId": {
            "type": "String"
        },
        "eventHubName": {
            "type": "String"
        }

    },
    "resources": [
        {
          "type": "Microsoft.KeyVault/vaults/providers/diagnosticSettings",
          "apiVersion": "2017-05-01-preview",
          "name": "[concat(parameters('vaultName'), '/Microsoft.Insights/', parameters('settingName'))]",
          "dependsOn": [],
          "properties": {
            "workspaceId": "[parameters('workspaceId')]",
            "storageAccountId": "[parameters('storageAccountId')]",
            "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
            "eventHubName": "[parameters('eventHubName')]",
            "logs": [
              {
                "category": "AuditEvent",
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
}
```

### <a name="parameter-file"></a>Parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "settingName": {
          "value": "Send to all locations"
      },
      "vaultName": {
        "value": "MyVault"
      },
      "workspaceId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/MyResourceGroup/providers/microsoft.operationalinsights/workspaces/MyWorkspace"
      },
      "storageAccountId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
      },
      "eventHubAuthorizationRuleId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.EventHub/namespaces/MyNameSpace/authorizationrules/RootManageSharedAccessKey"
      },
      "eventHubName": {
        "value": "my-eventhub"
      }
  }
}
```

## <a name="diagnostic-setting-for-azure-sql-database"></a>Azure SQL veritabanı için tanılama ayarı
Aşağıdaki örnek, bir Azure SQL veritabanı için bir tür kaynağı ekleyerek şablona bir tanılama ayarı oluşturur `microsoft.sql/servers/databases/providers/diagnosticSettings` .

### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "settingName": {
            "type": "String"
        },        
        "serverName": {
            "type": "String"
        },
        "dbName": {
            "type": "String"
        },
        "workspaceId": {
            "type": "String"
        },
        "storageAccountId": {
            "type": "String"
        },
        "eventHubAuthorizationRuleId": {
            "type": "String"
        },
        "eventHubName": {
            "type": "String"
        }

    },
    "resources": [
        {
          "type": "microsoft.sql/servers/databases/providers/diagnosticSettings",
          "apiVersion": "2017-05-01-preview",
          "name": "[concat(parameters('serverName'),'/',parameters('dbName'),'/microsoft.insights', parameters('settingName'))]",
          "dependsOn": [],
          "properties": {
            "workspaceId": "[parameters('workspaceId')]",
            "storageAccountId": "[parameters('storageAccountId')]",
            "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
            "eventHubName": "[parameters('eventHubName')]",
            "logs": [
              {
                "category": "SQLInsights",
                "enabled": true
              },
              {
                "category": "AutomaticTuning",
                "enabled": true
              },
              {
                "category": "QueryStoreRuntimeStatistics",
                "enabled": true
              },
              {
                "category": "QueryStoreWaitStatistics",
                "enabled": true
              },
              {
                "category": "Errors",
                "enabled": true
              },
              {
                "category": "DatabaseWaitStatistics",
                "enabled": true
              },
              {
                "category": "Timeouts",
                "enabled": true
              },
              {
                "category": "Blocks",
                "enabled": true
              },
              {
                "category": "Deadlocks",
                "enabled": true
              }
            ],
            "metrics": [
              {
                "category": "Basic",
                "enabled": true
              },
              {
                "category": "InstanceAndAppAdvanced",
                "enabled": true
              },
              {
                "category": "WorkloadManagement",
                "enabled": true
              }
            ]
          }
        }
    ]
}
```

### <a name="parameter-file"></a>Parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "settingName": {
          "value": "Send to all locations"
      },
      "serverName": {
        "value": "MySqlServer"
      },
      "dbName": {
        "value": "MySqlDb"
      },
      "workspaceId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/MyResourceGroup/providers/microsoft.operationalinsights/workspaces/MyWorkspace"
      },
      "storageAccountId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
      },
      "eventHubAuthorizationRuleId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.EventHub/namespaces/MyNameSpace/authorizationrules/RootManageSharedAccessKey"
      },
      "eventHubName": {
        "value": "my-eventhub"
      }
  }
}
```

## <a name="diagnostic-setting-for-recovery-services-vault"></a>Kurtarma Hizmetleri Kasası için tanılama ayarı
Aşağıdaki örnek, bir Azure kurtarma hizmetleri Kasası için bir tür kaynağı ekleyerek şablona bir tanılama ayarı oluşturur `microsoft.recoveryservices/vaults/providers/diagnosticSettings` . Bu örnek, [Azure Kaynak günlüklerinde](../platform/resource-logs.md#send-to-log-analytics-workspace)açıklandığı şekilde toplama modunu belirtir. `Dedicated` `AzureDiagnostics` Özelliği için veya belirtin `logAnalyticsDestinationType` .

### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "recoveryServicesName": {
            "type": "String"
        },
        "settingName": {
            "type": "String"
        },
        "workspaceId": {
            "type": "String"
        },
        "storageAccountId": {
            "type": "String"
        },
        "eventHubAuthorizationRuleId": {
            "type": "String"
        },
        "eventHubName": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "microsoft.recoveryservices/vaults/providers/diagnosticSettings",
            "apiVersion": "2017-05-01-preview",
            "name": "[concat(parameters('recoveryServicesName'), '/Microsoft.Insights/', parameters('settingName'))]",
            "dependsOn": [],
            "properties": {
                "workspaceId": "[parameters('workspaceId')]",
                "storageAccountId": "[parameters('storageAccountId')]",
                "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
                "eventHubName": "[parameters('eventHubName')]",
                "metrics": [],
                "logs": [
                    {
                        "category": "AzureBackupReport",
                        "enabled": false
                    },
                    {
                        "category": "CoreAzureBackup",
                        "enabled": true
                    },
                    {
                        "category": "AddonAzureBackupJobs",
                        "enabled": true
                    },
                    {
                        "category": "AddonAzureBackupAlerts",
                        "enabled": true
                    },
                    {
                        "category": "AddonAzureBackupPolicy",
                        "enabled": true
                    },
                    {
                        "category": "AddonAzureBackupStorage",
                        "enabled": true
                    },
                    {
                        "category": "AddonAzureBackupProtectedInstance",
                        "enabled": true
                    },
                    {
                        "category": "AzureSiteRecoveryJobs",
                        "enabled": false
                    },
                    {
                        "category": "AzureSiteRecoveryEvents",
                        "enabled": false
                    },
                    {
                        "category": "AzureSiteRecoveryReplicatedItems",
                        "enabled": false
                    },
                    {
                        "category": "AzureSiteRecoveryReplicationStats",
                        "enabled": false
                    },
                    {
                        "category": "AzureSiteRecoveryRecoveryPoints",
                        "enabled": false
                    },
                    {
                        "category": "AzureSiteRecoveryReplicationDataUploadRate",
                        "enabled": false
                    },
                    {
                        "category": "AzureSiteRecoveryProtectedDiskDataChurn",
                        "enabled": false
                    }
                ],
                "logAnalyticsDestinationType": "Dedicated"
            }
        }
    ]
}
```

### <a name="parameter-file"></a>Parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "settingName": {
          "value": "Send to all locations"
      },
      "recoveryServicesName": {
        "value": "my-vault"
      },
      "workspaceId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/MyResourceGroup/providers/microsoft.operationalinsights/workspaces/MyWorkspace"
      },
      "storageAccountId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
      },
      "eventHubAuthorizationRuleId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.EventHub/namespaces/MyNameSpace/authorizationrules/RootManageSharedAccessKey"
      },
      "eventHubName": {
        "value": "my-eventhub"
      }
  }
}
```

## <a name="diagnostic-setting-for-log-analytics-workspace"></a>Log Analytics çalışma alanı için tanılama ayarı
Aşağıdaki örnek, bir Log Analytics çalışma alanı Kasası için, şablona bir tür kaynağı ekleyerek bir tanılama ayarı oluşturur `Microsoft.OperationalInsights/workspaces/providers/diagnosticSettings` . Bu örnekte, çalışma alanında yürütülen sorgularla ilgili denetim verileri aynı çalışma alanına gönderilir.

### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String"
        },
        "settingName": {
            "type": "String"
        },
        "workspaceId": {
            "type": "String"
        },
        "storageAccountId": {
            "type": "String"
        },
        "eventHubAuthorizationRuleId": {
            "type": "String"
        },
        "eventHubName": {
            "type": "String"
        }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces/providers/diagnosticSettings",
            "apiVersion": "2017-05-01-preview",
            "name": "[concat(parameters('workspaceName'), '/Microsoft.Insights/', parameters('settingName'))]",
            "dependsOn": [],
            "properties": {
                "workspaceId": "[parameters('workspaceId')]",
                "storageAccountId": "[parameters('storageAccountId')]",
                "eventHubAuthorizationRuleId": "[parameters('eventHubAuthorizationRuleId')]",
                "eventHubName": "[parameters('eventHubName')]",
                "metrics": [],
                "logs": [
                    {
                        "category": "LAQueryLogs",
                        "enabled": true
                    }
                ]
            }
        }
    ]
}
```

### <a name="parameter-file"></a>Parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "settingName": {
          "value": "Send to all locations"
      },
      "workspaceName": {
        "value": "MyWorkspace"
      },
      "workspaceId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/MyResourceGroup/providers/microsoft.operationalinsights/workspaces/MyWorkspace"
      },
      "storageAccountId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.Storage/storageAccounts/mystorageaccount"
      },
      "eventHubAuthorizationRuleId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup/providers/Microsoft.EventHub/namespaces/MyNameSpace/authorizationrules/RootManageSharedAccessKey"
      },
      "eventHubName": {
        "value": "my-eventhub"
      }
  }
}
```

## <a name="diagnostic-setting-for-azure-storage"></a>Azure depolama için tanılama ayarı
Aşağıdaki örnek, depolama hesabında kullanılabilen her bir depolama hizmeti uç noktası için bir tanılama ayarı oluşturur. Hesapta kullanılabilir olan her bir depolama hizmetine bir ayar uygulanır. Kullanılabilir depolama hizmetleri depolama hesabı türüne bağlıdır. Bu şablon, hesapta yalnızca hesap için varsa bir depolama hizmeti için bir tanılama ayarı oluşturur. Kullanılabilir her hizmet için, tanılama ayarı işlem ölçümlerini ve okuma, yazma ve silme işlemlerine yönelik kaynak günlüklerinin toplanmasını mümkün tutar.

### <a name="template-file"></a>Şablon dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        },
        "settingName": {
            "type": "string"
        },
        "storageSyncName": {
            "type": "string"
        },
        "workspaceId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-10-01",
            "name": "nested",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "expressionEvaluationOptions": {
                    "scope": "inner"
                },
                "parameters": {
                    "endpoints": {
                        "value": "[reference(resourceId('Microsoft.Storage/storageAccounts', parameters('storageAccountName')), '2019-06-01', 'Full').properties.primaryEndpoints]"
                    },
                    "settingName": {
                        "value": "[parameters('settingName')]"
                    },
                    "storageAccountName": {
                        "value": "[parameters('storageAccountName')]"
                    },
                    "storageSyncName": {
                        "value": "[parameters('storageSyncName')]"
                    },
                    "workspaceId": {
                        "value": "[parameters('workspaceId')]"
                    }
                },
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {
                        "endpoints": {
                            "type": "object"
                        },
                        "settingName": {
                            "type": "String"
                        },
                        "storageAccountName": {
                            "type": "String"
                        },
                        "storageSyncName": {
                            "type": "String"
                        },
                        "workspaceId": {
                            "type": "String"
                        }
                    },
                    "variables": {
                        "hasblob": "[contains(parameters('endpoints'),'blob')]",
                        "hastable": "[contains(parameters('endpoints'),'table')]",
                        "hasfile": "[contains(parameters('endpoints'),'file')]",
                        "hasqueue": "[contains(parameters('endpoints'),'queue')]"
                    },
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts/providers/diagnosticsettings",
                            "apiVersion": "2017-05-01-preview",
                            "name": "[concat(parameters('storageAccountName'),'/Microsoft.Insights/', parameters('settingName'))]",

                            "properties": {
                                "workspaceId": "[parameters('workspaceId')]",
                                "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageSyncName'))]",
                                "metrics": [
                                    {
                                        "category": "Transaction",
                                        "enabled": true
                                    }
                                ]
                            }
                        },
                        {
                            "condition": "[variables('hasblob')]",
                            "type": "Microsoft.Storage/storageAccounts/blobServices/providers/diagnosticsettings",
                            "apiVersion": "2017-05-01-preview",
                            "name": "[concat(parameters('storageAccountName'),'/default/Microsoft.Insights/', parameters('settingName'))]",
                            "properties": {
                                "workspaceId": "[parameters('workspaceId')]",
                                "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageSyncName'))]",
                                "logs": [
                                    {
                                        "category": "StorageRead",
                                        "enabled": true
                                    },
                                    {
                                        "category": "StorageWrite",
                                        "enabled": true
                                    },
                                    {
                                        "category": "StorageDelete",
                                        "enabled": true
                                    }
                                ],
                                "metrics": [
                                    {
                                        "category": "Transaction",
                                        "enabled": true
                                    }
                                ]
                            }
                        },
                        {
                            "condition": "[variables('hastable')]",
                            "type": "Microsoft.Storage/storageAccounts/tableServices/providers/diagnosticsettings",
                            "apiVersion": "2017-05-01-preview",
                            "name": "[concat(parameters('storageAccountName'),'/default/Microsoft.Insights/', parameters('settingName'))]",

                            "properties": {
                                "workspaceId": "[parameters('workspaceId')]",
                                "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageSyncName'))]",
                                "logs": [
                                    {
                                        "category": "StorageRead",
                                        "enabled": true
                                    },
                                    {
                                        "category": "StorageWrite",
                                        "enabled": true
                                    },
                                    {
                                        "category": "StorageDelete",
                                        "enabled": true
                                    }
                                ],
                                "metrics": [
                                    {
                                        "category": "Transaction",
                                        "enabled": true
                                    }
                                ]
                            }
                        },
                        {
                            "condition": "[variables('hasfile')]",
                            "type": "Microsoft.Storage/storageAccounts/fileServices/providers/diagnosticsettings",
                            "apiVersion": "2017-05-01-preview",
                            "name": "[concat(parameters('storageAccountName'),'/default/Microsoft.Insights/', parameters('settingName'))]",
                            "properties": {
                                "workspaceId": "[parameters('workspaceId')]",
                                "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageSyncName'))]",
                                "logs": [
                                    {
                                        "category": "StorageRead",
                                        "enabled": true
                                    },
                                    {
                                        "category": "StorageWrite",
                                        "enabled": true
                                    },
                                    {
                                        "category": "StorageDelete",
                                        "enabled": true
                                    }
                                ],
                                "metrics": [
                                    {
                                        "category": "Transaction",
                                        "enabled": true
                                    }
                                ]
                            }
                        },
                        {
                            "condition": "[variables('hasqueue')]",
                            "type": "Microsoft.Storage/storageAccounts/queueServices/providers/diagnosticsettings",
                            "apiVersion": "2017-05-01-preview",
                            "name": "[concat(parameters('storageAccountName'),'/default/Microsoft.Insights/', parameters('settingName'))]",
                            "properties": {
                                "workspaceId": "[parameters('workspaceId')]",
                                "storageAccountId": "[resourceId('Microsoft.Storage/storageAccounts', parameters('storageSyncName'))]",
                                "logs": [
                                    {
                                        "category": "StorageRead",
                                        "enabled": true
                                    },
                                    {
                                        "category": "StorageWrite",
                                        "enabled": true
                                    },
                                    {
                                        "category": "StorageDelete",
                                        "enabled": true
                                    }
                                ],
                                "metrics": [
                                    {
                                        "category": "Transaction",
                                        "enabled": true
                                    }
                                ]
                            }
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="parameter-file"></a>Parametre dosyası

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountName": {
          "value": "mymonitoredstorageaccount"
      },
      "settingName": {
          "value": "Send to all locations"
      },
      "storageSyncName": {
          "value": "mystorageaccount"
      },
      "workspaceId": {
          "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/MyResourceGroup/providers/microsoft.operationalinsights/workspaces/MyWorkspace"
      }
    }
  }
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure izleyici için diğer örnek şablonları alın](resource-manager-samples.md).
* [Tanılama ayarları hakkında daha fazla bilgi edinin](../platform/diagnostic-settings.md).
