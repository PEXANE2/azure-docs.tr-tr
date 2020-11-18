---
title: Konuk sistem durumunu VM'ler için Azure İzleyici etkinleştirme (Önizleme)
description: Aboneliğinizdeki VM'ler için Azure İzleyici Konuk durumunun nasıl etkinleştirileceğini ve VM 'Lerin nasıl ekleneceğini açıklar.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/16/2020
ms.openlocfilehash: 6033ca5b0eaf6845d14407832c776dd8e006226b
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94687150"
---
# <a name="enable-azure-monitor-for-vms-guest-health-preview"></a>Konuk sistem durumunu VM'ler için Azure İzleyici etkinleştirme (Önizleme)
VM'ler için Azure İzleyici Konuk sistem durumu, düzenli aralıklarla örneklendiği bir dizi performans ölçümlerine göre tanımlanan bir sanal makinenin durumunu görüntülemenizi sağlar. Bu makalede, aboneliğinizde bu özelliğin nasıl etkinleştirileceği ve her sanal makine için konuk izlemenin nasıl etkinleştirileceği açıklanır.

## <a name="current-limitations"></a>Geçerli sınırlamalar
VM'ler için Azure İzleyici Konuk sistem durumu, genel önizlemede aşağıdaki sınırlamalara sahiptir:

- Şu anda yalnızca Azure sanal makineleri desteklenmektedir. Sunucular için Azure Arc Şu anda desteklenmiyor.
- Sanal makinenin aşağıdaki işletim sistemlerinden birini çalıştırması gerekir: 
  - Ubuntu 16,04 LTS, Ubuntu 18,04 LTS
  - Windows Server 2012 veya üzeri
- Sanal makinenin aşağıdaki bölgelerden birinde bulunması gerekir:
  - Avustralya Güneydoğu
  - Central US
  - Doğu ABD
  - Doğu ABD 2
  - EUAP Doğu ABD 2
  - Kuzey Avrupa
  - Güneydoğu Asya
  - Güney Birleşik Krallık
  - West Europe
  - Batı ABD
  - Batı ABD 2
- Log Analytics çalışma alanı aşağıdaki bölgelerden birinde bulunmalıdır:
  - Doğu ABD
  - EUAP Doğu ABD 2
  - Batı Avrupa Bölgesi

## <a name="prerequisites"></a>Ön koşullar

- Sanal makinenin VM'ler için Azure İzleyici için eklendi olması gerekir.
- Ekleme adımlarının yürütüldüğü kullanıcının, sanal makine ve veri toplama kuralının bulunduğu aboneliğe en az katkıda bulunan düzeyinde erişimi olmalıdır.
- Gerekli Azure Kaynak sağlayıcılarının aşağıdaki bölümde açıklandığı gibi kayıtlı olması gerekir.


## <a name="register-required-azure-resource-providers"></a>Gerekli Azure Kaynak sağlayıcılarını kaydetme
Aşağıdaki Azure Kaynak sağlayıcılarının, VM'ler için Azure İzleyici Konuk sistem durumunu etkinleştirmek üzere aboneliğiniz için kayıtlı olması gerekir. 

- Microsoft. WorkloadMonitor
- Microsoft. Insights

[Azure kaynak sağlayıcıları ve türleri](../../azure-resource-manager/management/resource-providers-and-types.md)bölümünde açıklandığı gibi, bir kaynak sağlayıcısını kaydetmek için kullanılabilir yöntemlerden herhangi birini kullanabilirsiniz. Ayrıca, Azure Resource Manager kimliği doğrulanmış çağrı yapmak için armclient, Postman veya başka bir yöntem kullanarak aşağıdaki örnek komutu da kullanabilirsiniz:

```
POST https://management.azure.com/subscriptions/[subscriptionId]/providers/Microsoft.WorkloadMonitor/register?api-version=2019-10-01
POST https://management.azure.com/subscriptions/[subscriptionId]/providers/Microsoft.Insights/register?api-version=2019-10-01
```


## <a name="enable-a-virtual-machine-using-the-azure-portal"></a>Azure portal kullanarak bir sanal makineyi etkinleştirme
Azure portal bir sanal makine için konuk sistem durumunu etkinleştirdiğinizde, gerekli tüm yapılandırma sizin için gerçekleştirilir. Bu, veri toplama kuralının oluşturulmasını, sanal makineye konuk sistem durumu uzantısının yüklenmesini ve veri toplama kuralıyla bir ilişki oluşturmayı içerir.

VM'ler için Azure İzleyici ' deki **Başlarken** görünümünden, bir sanal makine için yükseltme iletisinin yanındaki bağlantıya tıklayın ve ardından **Yükselt** düğmesine tıklayın. Ayrıca, birden fazla sanal makineyi birlikte yükseltmek için seçebilirsiniz.

![Sanal makinede sağlık özelliğini etkinleştir](media/vminsights-health-enable/enable-agent.png)


## <a name="enable-a-virtual-machine-using-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak bir sanal makineyi etkinleştirme
Azure Resource Manager kullanarak sanal makinelerin etkinleştirilmesi için gereken üç adım vardır.

- Veri toplama kuralı oluştur.
- Her sanal makineye konuk sistem durumu uzantısını yükler
- Sanal makine ve veri toplama kuralı arasında bir ilişki oluşturun.

### <a name="create-data-collection-rule-dcr"></a>Veri toplama kuralı (DCR) oluştur

> [!NOTE]
> Bir sanal makineyi Azure portal kullanarak etkinleştirirseniz, burada açıklanan veri toplama kuralı sizin için oluşturulur. Bu durumda, bu adımı gerçekleştirmeniz gerekmez.

VM'ler için Azure İzleyici Konuk durumundaki izleyicilerin yapılandırması, [veri toplama kurallarında (DCR)](../platform/data-collection-rule-overview.md)depolanır. Konuk sistem durumu uzantısına sahip sanal makinelerin tüm izleyicilerini etkinleştirmek için aşağıdaki Kaynak Yöneticisi şablonunda tanımlanan veri toplama kuralını yükler. Konuk sistem durumu uzantısına sahip her sanal makinenin bu kuralla bir ilişkilendirilmesi gerekir.

> [!NOTE]
> İzleyicilerin varsayılan yapılandırmasını, [VM'ler için Azure izleyici Konuk durumunda Izlemeyi yapılandırma (Önizleme)](vminsights-health-configure.md)bölümünde açıklandığı şekilde değiştirmek için ek veri toplama kuralları oluşturabilirsiniz.

Şablon aşağıdaki parametreler için değer gerektirir:

- **Defaulthealthdatacollectionrulename**: şablonda tanımlanmış varsayılan adı tut.
- **destinationWorkspaceResourceId**: sanal makine veri toplama için kullanılan Log Analytics çalışma ALANıNıN kaynak kimliği.
- **Datacollectionrulelocation**: veri toplama kuralının bölgesi. Bunun Log Analytics çalışma alanının bölgesiyle eşleşmesi gerekir.


[Kaynak Yöneticisi şablonlar için herhangi bir dağıtım yöntemini](../../azure-resource-manager/templates/deploy-powershell.md)kullanarak şablonu dağıtın. Aşağıdaki komutlar, PowerShell veya Azure CLı kullanarak şablon ve parametre dosyalarını dağıtır.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
New-AzResourceGroupDeployment -Name GuestHealthDataCollectionRule -ResourceGroupName my-resource-group -TemplateFile Health.DataCollectionRule.template.json -TemplateParameterFile Health.DataCollectionRule.template.parameters.json
```

# <a name="cli"></a>[CLI](#tab/cli)

```cli
az deployment group create --name GuestHealthDataCollectionRule --resource-group my-resource-group --template-file Health.DataCollectionRule.template.json --parameters Health.DataCollectionRule.template.parameters.json
```

---



```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "defaultHealthDataCollectionRuleName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the data collection rule to create."
      },
      "defaultValue": "Microsoft-VMInsights-Health"
    },
    "destinationWorkspaceResourceId": {
      "type": "string",
      "metadata": {
        "description": "Specifies the Azure resource ID of the Log Analytics workspace to use to store virtual machine health data."
      }
    },
    "dataCollectionRuleLocation": {
      "type": "string",
      "metadata": {
        "description": "The location code in which the data colleciton rule should be deployed. Examples: eastus, westeurope, etc"
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Insights/dataCollectionRules",
      "name": "[parameters('defaultHealthDataCollectionRuleName')]",
      "location": "[parameters('dataCollectionRuleLocation')]",
      "apiVersion": "2019-11-01-preview",
      "properties": {
        "description": "Data collection rule for VM Insights health.",
        "dataSources": {
          "extensions": [
            {
              "name": "Microsoft-VMInsights-Health",
              "streams": [
                "Microsoft-HealthStateChange"
              ],
              "extensionName": "HealthExtension",
              "extensionSettings": {
                "schemaVersion": "1.0",
                "contentVersion": "",
                "healthRuleOverrides": [
                  {
                    "scopes": [ "*" ],
                    "monitors": ["root"],
                    "alertConfiguration": {
                      "isEnabled": true
                    }
                  }
                ]
              }
            }
          ]
        },
        "destinations": {
          "logAnalytics": [
            {
              "workspaceResourceId": "[parameters('destinationWorkspaceResourceId')]",
              "name": "Microsoft-HealthStateChange-Dest"
            }
          ]
        },
        "dataFlows": [
          {
            "streams": [
              "Microsoft-HealthStateChange"
            ],
            "destinations": [
              "Microsoft-HealthStateChange-Dest"
            ]
          }
        ]
      }
    }
  ]
}
```

### <a name="sample-parameter-file"></a>Örnek parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "healthDataCollectionRuleResourceId": {
        "value": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/my-resource-group/providers/microsoft.operationalinsights/workspaces/my-workspace"
      },
      "dataCollectionRuleLocation": {
        "value": "eastus"
      }
  }
}
```



## <a name="install-guest-health-extension-and-associate-with-data-collection-rule"></a>Konuk durum uzantısını yükler ve veri toplama kuralıyla ilişkilendir
Bir sanal makineyi Konuk sistem durumu için etkinleştirmek üzere aşağıdaki Kaynak Yöneticisi şablonunu kullanın. Bu, Konuk sistem durumu uzantısını yükleyerek, veri toplama kuralıyla ilişki oluşturur. Bu şablonu, [Kaynak Yöneticisi şablonları için herhangi bir dağıtım yöntemi](../../azure-resource-manager/templates/deploy-powershell.md)kullanarak dağıtabilirsiniz.


Örneğin, PowerShell veya Azure CLı kullanarak bir kaynak grubuna şablon ve parametre dosyasını dağıtmak için aşağıdaki komutları kullanın.


# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
New-AzResourceGroupDeployment -Name GuestHealthDeployment -ResourceGroupName my-resource-group -TemplateFile azure-monitor-deploy.json -TemplateParameterFile azure-monitor-deploy.parameters.json
```

# <a name="cli"></a>[CLI](#tab/cli)

```cli
az deployment group create --name GuestHealthDeployment --resource-group my-resource-group --template-file Health.VirtualMachine.template.json --parameters Health.VirtualMachine.template.parameters.json
```

---

### <a name="template-file"></a>Şablon dosyası

``` json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "virtualMachineName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the virtual machine."
      }
    },
    "virtualMachineLocation": {
      "type": "string",
      "metadata": {
        "description": "The location code of the virtual machine region (location). Examples: eastus, westeurope, etc"
      }
    },
    "virtualMachineOsType": {
      "type": "string",
      "metadata": {
        "description": "Specifies operating system type of the target virtual machine."
      },
      "allowedValues": ["windows", "linux"]
    },
    "dataCollectionRuleAssociationName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the data collection rule association to create."
      },
      "defaultValue": "VM-Health-Dcr-Association"
    },
    "healthDataCollectionRuleResourceId": {
      "type": "string",
      "metadata": {
        "description": "Specifies resource id of Azure Monitor Data Collection Rule for virtual machine health data."
      }
    }
  },
  "variables": {
    "healthExtensionProperties": {
      "windows": {
        "publisher": "Microsoft.Azure.Monitor.VirtualMachines.GuestHealth",
        "type": "GuestHealthWindowsAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true
      },
      "linux": {
        "publisher": "Microsoft.Azure.Monitor.VirtualMachines.GuestHealth",
        "type": "GuestHealthLinuxAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true
      }
    },
    "azureMonitorAgentExtensionProperties": {
      "windows": {
        "publisher": "Microsoft.Azure.Monitor", 
        "type": "AzureMonitorWindowsAgent", 
        "typeHandlerVersion": "1.0", 
        "autoUpgradeMinorVersion": false 
      },
      "linux": {
        "publisher": "Microsoft.Azure.Monitor", 
        "type": "AzureMonitorLinuxAgent", 
        "typeHandlerVersion": "1.5", 
        "autoUpgradeMinorVersion": false 
      }
    }
  },
  "resources": [
    {
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('virtualMachineName')]",
      "location": "[parameters('virtualMachineLocation')]",
      "apiVersion": "2017-03-30",
      "identity": {
        "type": "SystemAssigned"
      }
    },
    {
      "type": "Microsoft.Compute/virtualMachines/providers/dataCollectionRuleAssociations",
      "name": "[concat(parameters('virtualMachineName'), '/Microsoft.Insights/', parameters('dataCollectionRuleAssociationName'))]",
      "apiVersion": "2019-11-01-preview",
      "properties": {
        "description": "Association of data collection rule for VM Insights Health.",
        "dataCollectionRuleId": "[parameters('healthDataCollectionRuleResourceId')]"
      }
    },
    { 
      "type": "Microsoft.Compute/virtualMachines/extensions", 
      "apiVersion": "2019-12-01", 
      "name": "[concat(parameters('virtualMachineName'), '/', variables('azureMonitorAgentExtensionProperties')[parameters('virtualMachineOsType')].type)]",
      "location": "[parameters('virtualMachineLocation')]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines/providers/dataCollectionRuleAssociations', parameters('virtualMachineName'), 'Microsoft.Insights', parameters('dataCollectionRuleAssociationName'))]"
      ], 
      "properties": "[variables('azureMonitorAgentExtensionProperties')[parameters('virtualMachineOsType')]]"
    },
    {
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "name": "[concat(parameters('virtualMachineName'), '/', variables('healthExtensionProperties')[parameters('virtualMachineOsType')].type)]",
      "location": "[parameters('virtualMachineLocation')]",
      "apiVersion": "2018-06-01",
      "dependsOn": [ 
        "[resourceId('Microsoft.Compute/virtualMachines/extensions', parameters('virtualMachineName'), variables('azureMonitorAgentExtensionProperties')[parameters('virtualMachineOsType')].type)]",
        "[resourceId('Microsoft.Compute/virtualMachines/providers/dataCollectionRuleAssociations', parameters('virtualMachineName'), 'Microsoft.Insights', parameters('dataCollectionRuleAssociationName'))]"
      ],    
      "properties": "[variables('healthExtensionProperties')[parameters('virtualMachineOsType')]]"
    }               
  ]
}
```

### <a name="sample-parameter-file"></a>Örnek parametre dosyası

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualMachineName": {
        "value": "myvm"
      },
      "virtualMachineLocation": {
        "value": "eastus"
      },
      "virtualMachineOsType": {
        "value": "linux"
      },
      "healthDataCollectionRuleResourceId": {
        "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/my-resource-group/providers/Microsoft.Insights/dataCollectionRules/Microsoft-VMInsights-Health"
      },
      "healthExtensionVersion": {
        "value": "private-preview"
      }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [VM'ler için Azure İzleyici tarafından etkinleştirilen izleyicileri Özelleştir](vminsights-health-configure.md)