---
title: Log Analytics çalışma alanı için Azure Kaynak Yöneticisi şablonu
description: Log Analytics çalışma alanlarını oluşturmak ve yapılandırmak için Azure Kaynak Yöneticisi şablonlarını kullanabilirsiniz.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 01/09/2020
ms.openlocfilehash: 60f85a30815bc1bace409b50af6332bb6622d7ca
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80477988"
---
# <a name="manage-log-analytics-workspace-using-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonlarını kullanarak Günlük Analizi çalışma alanını yönetme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Azure Monitor'da Günlük Analizi çalışma alanlarını oluşturmak ve yapılandırmak için [Azure Kaynak Yöneticisi şablonlarını](../../azure-resource-manager/templates/template-syntax.md) kullanabilirsiniz. Şablonlarla gerçekleştirebileceğiniz görevlere örnek olarak şunlar verilebilir:

* Fiyatlandırma katmanı ve kapasite rezervasyonu ayarlamayı içeren bir çalışma alanı oluşturun
* Çözüm ekleme
* Kaydedilen aramalar oluşturma
* Bilgisayar grubu oluşturma
* Windows aracısı yüklü bilgisayarlardan IIS günlüklerinin toplanmasını etkinleştirme
* Linux ve Windows bilgisayarlarından performans sayaçları toplama
* Linux bilgisayarlarda syslog'dan etkinlik toplama 
* Windows etkinlik günlüklerinden olayları toplama
* Windows bilgisayardan özel günlükleri toplama
* Azure sanal makinesine günlük analizi aracısını ekleme
* Azure tanılama kullanılarak toplanan verileri dizinlemek için günlük analitiğini yapılandırma

Bu makalede, şablonlarla gerçekleştirebileceğiniz yapılandırmanın bazılarını gösteren şablon örnekleri sağlanabilir.

## <a name="api-versions"></a>API sürümleri

Aşağıdaki tabloda, bu örnekte kullanılan kaynakların API sürümü listeleneb.r.a.

| Kaynak | Kaynak türü | API sürümü |
|:---|:---|:---|
| Çalışma alanı   | çalışma alanı    | 2017-03-15-önizleme |
| Ara      | savedAramalar | 2015-03-20 |
| Veri kaynağı | Datasources   | 2015-11-01-önizleme |
| Çözüm    | çözümler     | 2015-11-01-önizleme |

## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

Aşağıdaki örnek, yerel makinenizden gelen bir şablonu kullanarak bir çalışma alanı oluşturur. JSON şablonu yalnızca yeni çalışma alanının adını ve konumunu gerektirecek şekilde yapılandırılır. [Erişim denetim modu,](design-logs-deployment.md#access-control-mode)fiyatlandırma katmanı, bekletme ve kapasite rezervasyon düzeyi gibi diğer çalışma alanı parametreleri için belirtilen değerleri kullanır.

> [!WARNING]
> Aşağıdaki şablon bir Log Analytics çalışma alanı oluşturur ve veri toplamayı yapılandırır. Bu, fatura landırma ayarlarınızı değiştirebilir. Azure ortamınızda uygulamadan önce Bir Log Analytics çalışma alanında toplanan verilerin faturalandırmasını anlamak için [Azure Monitör Günlükleri ile kullanımı ve maliyetleri yönet'i](manage-cost-storage.md) gözden geçirin.

Kapasite rezervasyonu için, SKU `CapacityReservation` ve özellik `capacityReservationLevel`için GB değeri belirterek veri sindirilmesi için seçili bir kapasite rezervasyonu tanımlarsınız. Aşağıdaki liste, yapılandırırken desteklenen değerleri ve davranışı ayrıntılarıyla anlatır.

- Rezervasyon limitini belirledikten sonra 31 gün içinde farklı bir SKU'ya geçemezsiniz.

- Rezervasyon değerini ayarladıktan sonra, yalnızca 31 gün içinde artırabilirsiniz.

- Değeri yalnızca en fazla `capacityReservationLevel` değeri 50000 olan 100'ün katları olarak ayarlayabilirsiniz.

- Rezervasyon düzeyini artırırsanız, zamanlayıcı sıfırlanır ve bu güncelleştirmeden itibaren 31 gün daha değiştiremezsiniz.  

- Çalışma alanı için başka bir özelliği değiştirir, ancak rezervasyon sınırını aynı düzeyde tutarsanız, zamanlayıcı sıfırlanmaz. 

### <a name="create-and-deploy-template"></a>Şablon oluşturma ve dağıtma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
    "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "workspaceName": {
            "type": "String",
            "metadata": {
              "description": "Specifies the name of the workspace."
            }
        },
      "sku": {
        "type": "string",
        "allowedValues": [
          "pergb2018",
          "Free",
          "Standalone",
          "PerNode",
          "Standard",
          "Premium"
          ],
        "defaultValue": "pergb2018",
        "metadata": {
        "description": "Pricing tier: PerGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
        }
      },
      "location": {
        "type": "String",
        "allowedValues": [
        "australiacentral", 
        "australiaeast", 
        "australiasoutheast", 
        "brazilsouth",
        "canadacentral", 
        "centralindia", 
        "centralus", 
        "eastasia", 
        "eastus", 
        "eastus2", 
        "francecentral", 
        "japaneast", 
        "koreacentral", 
        "northcentralus", 
        "northeurope", 
        "southafricanorth", 
        "southcentralus", 
        "southeastasia", 
        "uksouth", 
        "ukwest", 
        "westcentralus", 
        "westeurope", 
        "westus", 
        "westus2" 
        ],
      "metadata": {
        "description": "Specifies the location in which to create the workspace."
        }
      }
    },
    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "name": "[parameters('workspaceName')]",
            "apiVersion": "2017-03-15-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "name": "[parameters('sku')]"
                },
                "retentionInDays": 120,
                "features": {
                    "searchVersion": 1,
                    "legacy": 0,
                    "enableLogAccessUsingOnlyResourcePermissions": true
                }
            }
          }
       ]
    }
    ```

   >[!NOTE]
   >Kapasite rezervasyon ayarları için bu özellikleri "sku" altında kullanın:
   >* "isim": "CapacityReservation",
   >* "capacityReservationLevel": 100

2. Gereksinimlerinizi karşılamak için şablonu edin. Parametreleri satır değerleri olarak geçirmek yerine Kaynak [Yöneticisi parametreleri dosyası](../../azure-resource-manager/templates/parameter-files.md) oluşturmayı düşünün. Hangi özelliklerin ve değerlerin desteklenildiğini öğrenmek için [Microsoft.OperationalInsights/çalışma alanları şablonu](https://docs.microsoft.com/azure/templates/microsoft.operationalinsights/workspaces) başvurularını inceleyin. 

3. Bu dosyayı yerel bir klasöre **deploylaworkspacetemplate.json** olarak kaydedin.

4. Bu şablonu dağıtmaya hazırsınız. Çalışma alanını oluşturmak için PowerShell veya komut satırını kullanarak çalışma alanı adını ve konumunu komutun bir parçası olarak belirtirsiniz. Çalışma alanı adı, tüm Azure aboneliklerinde genel olarak benzersiz olmalıdır.

   * PowerShell için şablonu içeren klasörden aşağıdaki komutları kullanın:
   
        ```powershell
        New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile deploylaworkspacetemplate.json -workspaceName <workspace-name> -location <location>
        ```

   * Komut satırı için, şablonu içeren klasörden aşağıdaki komutları kullanın:

        ```cmd
        azure config mode arm
        azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile deploylaworkspacetemplate.json --workspaceName <workspace-name> --location <location>
        ```

Dağıtımın tamamlanması birkaç dakika sürebilir. Bittiğinde, sonucu içeren aşağıdakine benzer bir ileti görürsünüz:<br><br> ![Dağıtım tamamlandığında örnek sonuç](./media/template-workspace-configuration/template-output-01.png)

## <a name="configure-a-log-analytics-workspace"></a>Günlük Analizi çalışma alanını yapılandırma

Aşağıdaki şablon örneği nasıl gösterin:

1. Çalışma alanına çözümler ekleme
2. Kaydedilmiş aramalar oluşturun. Dağıtımların kaydedilen aramaları yanlışlıkla geçersiz kılmadığından emin olmak için, kaydedilen aramaların idempotency'sini geçersiz kılmak ve korumak için "savedSearches" kaynağına bir eTag özelliği eklenmelidir.
3. Bilgisayar grubu oluşturma
4. Windows aracısı yüklü bilgisayarlardan IIS günlüklerinin toplanmasını etkinleştirme
5. Linux bilgisayarlarından Mantıksal Disk perf sayaçları toplamak (% Kullanılmış Inodes; Ücretsiz Megabayt; % Kullanılan Alan; Disk Aktarımları/sn; Disk Okuma/sn; Disk Yazma/sn)
6. Linux bilgisayarlarından syslog etkinliklerini toplayın
7. Windows bilgisayarlardan Uygulama Olay Günlüğü'nden Hata ve Uyarı olaylarını toplama
8. Windows bilgisayarlardan Bellek Kullanılabilir Mbytes performans sayacı toplama
9. Azure tanılama tarafından yazılan IIS günlüklerini ve Windows Olay günlüklerini bir depolama hesabına toplama
10. Windows bilgisayardan özel günlükleri toplama

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "Workspace name"
      }
    },
    "sku": {
      "type": "string",
      "allowedValues": [
        "PerGB2018",
        "Free",
        "Standalone",
        "PerNode",
        "Standard",
        "Premium"
      ],
      "defaultValue": "pergb2018",
      "metadata": {
        "description": "Pricing tier: pergb2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
      }
    },
    "dataRetention": {
      "type": "int",
      "defaultValue": 30,
      "minValue": 7,
      "maxValue": 730,
      "metadata": {
        "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can only have 7 days."
      }
    },
    "immediatePurgeDataOn30Days": {
      "type": "bool",
      "defaultValue": "[bool('false')]",
      "metadata": {
        "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. Use this with extreme caution. This only applies when retention is being set to 30 days."
      }
    },
    "location": {
      "type": "string",
      "allowedValues": [
        "australiacentral", 
        "australiaeast", 
        "australiasoutheast", 
        "brazilsouth",
        "canadacentral", 
        "centralindia", 
        "centralus", 
        "eastasia", 
        "eastus", 
        "eastus2", 
        "francecentral", 
        "japaneast", 
        "koreacentral", 
        "northcentralus", 
        "northeurope", 
        "southafricanorth", 
        "southcentralus", 
        "southeastasia", 
        "uksouth", 
        "ukwest", 
        "westcentralus", 
        "westeurope", 
        "westus", 
        "westus2"
      ],
      "metadata": {
        "description": "Specifies the location in which to create the workspace."
      }
    },
    "applicationDiagnosticsStorageAccountName": {
        "type": "string",
        "metadata": {
          "description": "Name of the storage account with Azure diagnostics output"
        }
    },
    "applicationDiagnosticsStorageAccountResourceGroup": {
        "type": "string",
        "metadata": {
          "description": "The resource group name containing the storage account with Azure diagnostics output"
        }
    },
    "customLogName": {
    "type": "string",
    "metadata": {
      "description": "The custom log name"
      }
     }
    },
    "variables": {
      "Updates": {
        "Name": "[Concat('Updates', '(', parameters('workspaceName'), ')')]",
        "GalleryName": "Updates"
      },
      "AntiMalware": {
        "Name": "[concat('AntiMalware', '(', parameters('workspaceName'), ')')]",
        "GalleryName": "AntiMalware"
      },
      "SQLAssessment": {
        "Name": "[Concat('SQLAssessment', '(', parameters('workspaceName'), ')')]",
        "GalleryName": "SQLAssessment"
      },
      "diagnosticsStorageAccount": "[resourceId(parameters('applicationDiagnosticsStorageAccountResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName'))]"
  },
  "resources": [
    {
      "apiVersion": "2017-03-15-preview",
      "type": "Microsoft.OperationalInsights/workspaces",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "properties": {
        "retentionInDays": "[parameters('dataRetention')]",
        "features": {
          "immediatePurgeDataOn30Days": "[parameters('immediatePurgeDataOn30Days')]"
        },
        "sku": {
          "name": "[parameters('sku')]"
        }
      },
      "resources": [
        {
          "apiVersion": "2015-03-20",
          "name": "VMSS Queries2",
          "type": "savedSearches",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "category": "VMSS",
            "eTag": "*",
            "displayName": "VMSS Instance Count",
            "query": "Event | where Source == \"ServiceFabricNodeBootstrapAgent\" | summarize AggregatedValue = count() by Computer",
            "version": 1
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsEvent1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsEvent",
          "properties": {
            "eventLogName": "Application",
            "eventTypes": [
              {
                "eventType": "Error"
              },
              {
                "eventType": "Warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsPerfCounter1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsPerformanceCounter",
          "properties": {
            "objectName": "Memory",
            "instanceName": "*",
            "intervalSeconds": 10,
            "counterName": "Available MBytes"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleIISLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "IISLogs",
          "properties": {
            "state": "OnPremiseEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslog",
          "properties": {
            "syslogName": "kern",
            "syslogSeverities": [
              {
                "severity": "emerg"
              },
              {
                "severity": "alert"
              },
              {
                "severity": "crit"
              },
              {
                "severity": "err"
              },
              {
                "severity": "warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslogCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerf1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceObject",
          "properties": {
            "performanceCounters": [
              {
                "counterName": "% Used Inodes"
              },
              {
                "counterName": "Free Megabytes"
              },
              {
                "counterName": "% Used Space"
              },
              {
                "counterName": "Disk Transfers/sec"
              },
              {
                "counterName": "Disk Reads/sec"
              },
              {
                "counterName": "Disk Writes/sec"
              }
            ],
            "objectName": "Logical Disk",
            "instanceName": "*",
            "intervalSeconds": 10
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "dataSources",
          "name": "[concat(parameters('workspaceName'), parameters('customLogName'))]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', '/', parameters('workspaceName'))]"
          ],
          "kind": "CustomLog",
          "properties": {
            "customLogName": "[parameters('customLogName')]",
            "description": "this is a description",
            "extractions": [
              {
                "extractionName": "TimeGenerated",
                "extractionProperties": {
                  "dateTimeExtraction": {
                    "regex": [
                      {
                        "matchIndex": 0,
                        "numberdGroup": null,
                        "pattern": "((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9]"
                      }
                    ]
                  }
                },
                "extractionType": "DateTime"
              }
            ],
            "inputs": [
              {
                "location": {
                  "fileSystemLocations": {
                    "linuxFileTypeLogPaths": null,
                    "windowsFileTypeLogPaths": [
                      "[concat('c:\\Windows\\Logs\\',parameters('customLogName'))]"
                    ]
                  }
                },
                "recordDelimiter": {
                  "regexDelimiter": {
                    "matchIndex": 0,
                    "numberdGroup": null,
                    "pattern": "(^.*((\\d{2})|(\\d{4}))-([0-1]\\d)-(([0-3]\\d)|(\\d))\\s((\\d)|([0-1]\\d)|(2[0-4])):[0-5][0-9]:[0-5][0-9].*$)"
                  }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerfCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-03-20",
          "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('workspaceName'))]",
          "type": "storageinsightconfigs",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "containers": [ 
              "wad-iis-logfiles" 
            ],
            "tables": [
              "WADWindowsEventLogsTable"
            ],
            "storageAccount": {
              "id": "[variables('diagnosticsStorageAccount')]",
              "key": "[listKeys(variables('diagnosticsStorageAccount'),'2015-06-15').key1]"
            }
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('Updates').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('Updates').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('Updates').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('AntiMalware').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('AntiMalware').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('AntiMalware').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('AntiMalware').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('SQLAssessment').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('SQLAssessment').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('SQLAssessment').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('SQLAssessment').GalleryName)]",
            "promotionCode": ""
          }
        }
      ]
    }
  ],
  "outputs": {
    "workspaceName": {
      "type": "string",
      "value": "[parameters('workspaceName')]"
    },
    "provisioningState": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').provisioningState]"
    },
    "source": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').source]"
    },
    "customerId": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').customerId]"
    },
    "sku": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').sku.name]"
    },
    "retentionInDays": {
      "type": "int",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').retentionInDays]"
    },
    "immediatePurgeDataOn30Days": {  
      "type": "bool",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').features.immediatePurgeDataOn30Days]"
    },
    "portalUrl": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').portalUrl]"
    }
  }
}
```

### <a name="deploying-the-sample-template"></a>Örnek şablonu dağıtma

Örnek şablonu dağıtmak için:

1. Ekli örneği bir dosyaya kaydetme, örneğin`azuredeploy.json` 
2. İstediğiniş yapılandırmaya sahip olmak için şablonu düzenleme
3. Şablonu dağıtmak için PowerShell'i veya komut satırını kullanma

#### <a name="powershell"></a>PowerShell

```powershell
New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile azuredeploy.json
```

#### <a name="command-line"></a>Komut satırı

```cmd
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile azuredeploy.json
```

## <a name="example-resource-manager-templates"></a>Örnek Kaynak Yöneticisi şablonları

Azure hızlı başlatma şablon galerisi, Günlük Analizi için aşağıdakiler dahil olmak üzere çeşitli şablonlar içerir:

* [Log Analytics VM uzantısı ile Windows çalıştıran sanal bir makine dağıtma](https://azure.microsoft.com/documentation/templates/201-oms-extension-windows-vm/)
* [Log Analytics VM uzantısı ile Linux çalıştıran sanal bir makine dağıtma](https://azure.microsoft.com/documentation/templates/201-oms-extension-ubuntu-vm/)
* [Varolan bir Günlük Analizi çalışma alanını kullanarak Azure Site Kurtarma'yı izleme](https://azure.microsoft.com/documentation/templates/asr-oms-monitoring/)
* [Varolan bir Log Analytics çalışma alanını kullanarak Azure Web Uygulamalarını izleyin](https://azure.microsoft.com/documentation/templates/101-webappazure-oms-monitoring/)
* [Log Analytics'e varolan bir depolama hesabı ekleme](https://azure.microsoft.com/resources/templates/oms-existing-storage-account/)

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynak Yöneticisi şablonu kullanarak Windows aracısını Azure VM'lerine dağıtın.](../../virtual-machines/extensions/oms-windows.md)

* [Kaynak Yöneticisi şablonu kullanarak Linux aracısını Azure VM'lerine dağıtın.](../../virtual-machines/extensions/oms-linux.md)
