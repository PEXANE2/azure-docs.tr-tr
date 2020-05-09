---
title: Otomasyon hesabı oluşturmak için Azure Resource Manager şablonları kullanma | Microsoft Docs
description: Azure Otomasyonu hesabı oluşturmak için bir Azure Resource Manager şablonu kullanabilirsiniz.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 04/24/2020
ms.openlocfilehash: 19aee9d5fdf3f4a3d74484bb7cb2e609bc2807b4
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82927879"
---
# <a name="create-an-automation-account-by-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak Otomasyon hesabı oluşturma

Kaynak grubunuzda Azure Otomasyonu hesabı oluşturmak için [Azure Resource Manager şablonlarını](../azure-resource-manager/templates/template-syntax.md) kullanabilirsiniz. Bu makalede şu örnek bir şablon verilmiştir:

* Bir Azure Izleyici Log Analytics çalışma alanı oluşturulmasını otomatikleştirir.
* Bir Azure Otomasyonu hesabının oluşturulmasını otomatikleştirir.
* Otomasyon hesabını Log Analytics çalışma alanına bağlar.

Şablon, Azure veya Azure olmayan sanal makinelerin veya çözümlerin eklenmesi için otomatikleştirmez. 

>[!NOTE]
>Azure Resource Manager şablonu kullanırken Otomasyon farklı çalıştır hesabının oluşturulması desteklenmez. Portaldan veya PowerShell ile el ile bir farklı çalıştır hesabı oluşturmak için bkz. [Farklı Çalıştır hesaplarını yönetme](manage-runas-account.md).

## <a name="api-versions"></a>API sürümleri

Aşağıdaki tabloda, bu örnekte kullanılan kaynakların API sürümü listelenmektedir.

| Kaynak | Kaynak türü | API sürümü |
|:---|:---|:---|
| Çalışma alanı | çalışma alanı | 2017-03-15-Önizleme |
| Otomasyon hesabı | automation | 2015-10-31 | 

## <a name="before-you-use-the-template"></a>Şablonu kullanmadan önce

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale Azure PowerShell az modülünü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız Azure ile bir bağlantı oluşturmak için öğesini `Connect-AzAccount` de çalıştırmanız gerekir. PowerShell ile, dağıtım [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment)kullanır.

Azure CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, sürüm 2.1.0 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Azure CLı ile bu dağıtım [az Group Deployment Create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)kullanır. 

JSON şablonu şunları isteyecek şekilde yapılandırılır:

* Çalışma alanının adı.
* Çalışma alanının oluşturulacağı bölge.
* Otomasyon hesabının adı.
* Hesabın oluşturulacağı bölge.

Şablondaki aşağıdaki parametreler, Log Analytics çalışma alanı için varsayılan bir değerle ayarlanır:

* *SKU* , Nisan 2018 fiyatlandırma MODELINDE yayınlanan GB başına fiyatlandırma katmanını varsayılan olarak alır.
* *Dataretention* varsayılan değer 30 gündür.
* *capacityReservationLevel* varsayılan olarak 100 GB olur.

>[!WARNING]
>Nisan 2018 fiyatlandırma modelini kabul eden bir abonelikte Log Analytics çalışma alanı oluşturmak veya yapılandırmak istiyorsanız, geçerli Log Analytics fiyatlandırma katmanı yalnızca *PerGB2018*olur.
>

JSON şablonu, ortamınızda standart bir yapılandırma olarak kullanılacak diğer parametreler için varsayılan bir değer belirtir. Şablonu kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında saklayabilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynak dağıtma](../azure-resource-manager/templates/deploy-cli.md).

Azure Otomasyonu ve Azure Izleyici 'de yeni başladıysanız aşağıdaki yapılandırma ayrıntılarını anlamanız önemlidir. Yeni otomasyon hesabınıza bağlı bir Log Analytics çalışma alanı oluşturmaya, yapılandırmaya ve kullanmaya çalıştığınızda hatalardan kaçınmanıza yardımcı olabilirler. 

* Erişim denetimi modu, fiyatlandırma katmanı, bekletme ve kapasite ayırma düzeyi gibi çalışma alanı yapılandırma seçeneklerini tam olarak anlamak için [ek ayrıntıları](../azure-monitor/platform/template-workspace-configuration.md#create-a-log-analytics-workspace) gözden geçirin.

* Desteklenen bölgeleri satır içi veya bir parametre dosyasında belirtmek için [çalışma alanı eşlemelerini](how-to/region-mappings.md) gözden geçirin. Log Analytics çalışma alanını ve aboneliğinizdeki Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklenir.

* Azure Izleyici günlüklerine yeni başladıysanız ve bir çalışma alanını henüz dağıtmadıysanız, [çalışma alanı tasarım kılavuzunu](../azure-monitor/platform/design-logs-deployment.md)gözden geçirmeniz gerekir. Erişim denetimi hakkında bilgi edinmenize ve kuruluşunuz için önerdiğimiz tasarım uygulama stratejilerini anlamanıza yardımcı olur.

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

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
                "pergb2018",
                "Free",
                "Standalone",
                "PerNode",
                "Standard",
                "Premium"
            ],
            "defaultValue": "pergb2018",
            "metadata": {
                "description": "Pricing tier: perGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium), which are not available to all customers."
            }
        },
        "dataRetention": {
            "type": "int",
            "defaultValue": 30,
            "minValue": 7,
            "maxValue": 730,
            "metadata": {
                "description": "Number of days of retention. Workspaces in the legacy Free pricing tier can have only 7 days."
            }
        },
        "immediatePurgeDataOn30Days": {
            "type": "bool",
            "defaultValue": "[bool('false')]",
            "metadata": {
                "description": "If set to true when changing retention to 30 days, older data will be immediately deleted. Use this with extreme caution. This applies only when retention is being set to 30 days."
            }
        },
        "location": {
            "type": "string",
            "metadata": {
                "description": "Specifies the location in which to create the workspace."
            }
        },
        "automationAccountName": {
            "type": "string",
            "metadata": {
                "description": "Automation account name"
            }
        },
        "automationAccountLocation": {
            "type": "string",
            "metadata": {
                "description": "Specify the location in which to create the Automation account."
            }
        },
        "sampleGraphicalRunbookName": {
                "type": "String",
                "defaultValue": "AzureAutomationTutorial"
            },
            "sampleGraphicalRunbookDescription": {
                "type": "String",
                "defaultValue": " An example runbook that gets all the Resource Manager resources by using the Run As account (service principal)."
            },
            "sampleGraphicalRunbookContentUri": {
                "type": "String",
                "defaultValue": "https://eus2oaasibizamarketprod1.blob.core.windows.net/marketplace-runbooks/AzureAutomationTutorial.graphrunbook"
            },
            "samplePowerShellRunbookName": {
                "type": "String",
                "defaultValue": "AzureAutomationTutorialScript"
            },
            "samplePowerShellRunbookDescription": {
                "type": "String",
                "defaultValue": " An example runbook that gets all the Resource Manager resources by using the Run As account (service principal)."
            },
            "samplePowerShellRunbookContentUri": {
                "type": "String",
                "defaultValue": "https://eus2oaasibizamarketprod1.blob.core.windows.net/marketplace-runbooks/AzureAutomationTutorial.ps1"
            },
            "samplePython2RunbookName": {
                "type": "String",
                "defaultValue": "AzureAutomationTutorialPython2"
            },
            "samplePython2RunbookDescription": {
                "type": "String",
                "defaultValue": " An example runbook that gets all the Resource Manager resources by using the Run As account (service principal)."
            },
            "samplePython2RunbookContentUri": {
                "type": "String",
                "defaultValue": "https://eus2oaasibizamarketprod1.blob.core.windows.net/marketplace-runbooks/AzureAutomationTutorialPython2.py"
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
                    "Name": "[parameters('sku')]",
                    "name": "CapacityReservation",
                    "capacityReservationLevel": 100
                },
                "retentionInDays": "[parameters('dataRetention')]",
                "features": {
                    "searchVersion": 1,
                    "legacy": 0,
                    "enableLogAccessUsingOnlyResourcePermissions": true
                }
            },
        "resources": [
        {
            "type": "Microsoft.Automation/automationAccounts",
            "apiVersion": "2015-01-01-preview",
            "name": "[parameters('automationAccountName')]",
            "location": "[parameters('automationAccountLocation')]",
            "dependsOn": [
                 "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
            ],
            "tags": {},
            "properties": {
                "sku": {
                    "name": "Basic"
                }
            },
            "resources": [
                    {
                        "type": "runbooks",
                        "apiVersion": "2015-01-01-preview",
                        "name": "[parameters('sampleGraphicalRunbookName')]",
                        "location": "[parameters('automationAccountLocation')]",
                        "dependsOn": [
                            "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
                        ],
                        "tags": {},
                        "properties": {
                            "runbookType": "GraphPowerShell",
                            "logProgress": "false",
                            "logVerbose": "false",
                            "description": "[parameters('sampleGraphicalRunbookDescription')]",
                            "publishContentLink": {
                                "uri": "[parameters('sampleGraphicalRunbookContentUri')]",
                                "version": "1.0.0.0"
                            }
                        }
                    },
                    {
                        "type": "runbooks",
                        "apiVersion": "2015-01-01-preview",
                        "name": "[parameters('samplePowerShellRunbookName')]",
                        "location": "[parameters('automationAccountLocation')]",
                        "dependsOn": [
                            "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
                        ],
                        "tags": {},
                        "properties": {
                            "runbookType": "PowerShell",
                            "logProgress": "false",
                            "logVerbose": "false",
                            "description": "[parameters('samplePowerShellRunbookDescription')]",
                            "publishContentLink": {
                                "uri": "[parameters('samplePowerShellRunbookContentUri')]",
                                "version": "1.0.0.0"
                            }
                        }
                    },
                    {
                        "type": "runbooks",
                        "apiVersion": "2015-01-01-preview",
                        "name": "[parameters('samplePython2RunbookName')]",
                        "location": "[parameters('automationAccountLocation')]",
                        "dependsOn": [
                            "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
                        ],
                        "tags": {},
                        "properties": {
                            "runbookType": "Python2",
                            "logProgress": "false",
                            "logVerbose": "false",
                            "description": "[parameters('samplePython2RunbookDescription')]",
                            "publishContentLink": {
                                "uri": "[parameters('samplePython2RunbookContentUri')]",
                                "version": "1.0.0.0"
                            }
                        }
                    }
                ]
        },
        {
            "apiVersion": "2015-11-01-preview",
            "type": "Microsoft.OperationalInsights/workspaces/linkedServices",
            "name": "[concat(parameters('workspaceName'), '/' , 'Automation')]",
            "location": "[resourceGroup().location]",
            "dependsOn": [
                "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]",
                "[concat('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            ],
            "properties": {
                "resourceId": "[resourceId('Microsoft.Automation/automationAccounts/', parameters('automationAccountName'))]"
            }
        }
       ]
      }
     ]
    }
    ```

2. Gereksinimlerinizi karşılayacak şekilde şablonu düzenleyin. Parametreleri satır içi değerler olarak geçirmek yerine bir [Kaynak Yöneticisi parametre dosyası](../azure-resource-manager/templates/parameter-files.md) oluşturmayı düşünün.

3. Bu dosyayı yerel bir klasöre deployAzAutomationAccttemplate. JSON olarak kaydedin.

4. Bu şablonu dağıtmaya hazırsınız. PowerShell veya Azure CLı kullanabilirsiniz. Bir çalışma alanı ve Otomasyon hesabı adı istendiğinde, tüm Azure aboneliklerinizde genel olarak benzersiz bir ad sağlayın.

    **PowerShell**

    ```powershell
    New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deployAzAutomationAccttemplate.json
    ```

    **Azure CLI**

    ```cli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deployAzAutomationAccttemplate.json
    ```

    Dağıtımın tamamlanması birkaç dakika sürebilir. Bunu yaparken, sonucu içeren aşağıdakine benzer bir ileti görürsünüz.

    ![Dağıtım tamamlandığında örnek sonuç](media/automation-create-account-template/template-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık bir Otomasyon hesabınız olduğuna göre, runbook 'lar oluşturabilir ve el ile işlemleri otomatikleştirebilirsiniz.

* PowerShell runbook 'larını kullanmaya başlamak için bkz. [PowerShell runbook 'U oluşturma](automation-first-runbook-textual-powershell.md).
* PowerShell iş akışı runbook 'larını kullanmaya başlamak için bkz. [PowerShell Iş akışı runbook 'U oluşturma](automation-first-runbook-textual.md).
* Python 2 runbook 'ları kullanmaya başlamak için bkz. [Python runbook oluşturma](automation-first-runbook-textual-python2.md).