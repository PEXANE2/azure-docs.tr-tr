---
title: Güncelleştirme Yönetimi eklemek için Azure Resource Manager şablonları kullanın | Microsoft Docs
description: Azure Resource Manager şablonu kullanarak Azure Otomasyonu Güncelleştirme Yönetimi çözümünü ekleyebilirsiniz.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 04/24/2020
ms.openlocfilehash: 45045cb1360658d394e5469d022ac03033d11aff
ms.sourcegitcommit: fad3aaac5af8c1b3f2ec26f75a8f06e8692c94ed
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "82165799"
---
# <a name="onboard-update-management-solution-using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak Güncelleştirme Yönetimi çözümü ekleme

Kaynak grubunuzda Azure Otomasyonu Güncelleştirme Yönetimi çözümünü etkinleştirmek için [Azure Resource Manager şablonlarını](../azure-resource-manager/templates/template-syntax.md) kullanabilirsiniz. Bu makale, aşağıdakileri otomatikleştiren örnek bir şablon sağlar:

* Azure Izleyici Log Analytics çalışma alanı oluşturma.
* Azure Otomasyonu hesabı oluşturma.
* Zaten bağlı değilse, Otomasyon hesabını Log Analytics çalışma alanına bağlama.
* Azure Otomasyonu Güncelleştirme Yönetimi çözümünü ekleme.

Şablon bir veya daha fazla Azure veya Azure dışı VM 'yi eklemeyi otomatik hale getirir.

Aboneliğinizde desteklenen bir bölgede dağıtılmış bir Log Analytics çalışma alanı ve Otomasyon hesabı zaten varsa, bunlar bağlanmaz. Çalışma alanında Güncelleştirme Yönetimi çözümü zaten dağıtılmadı. Bu şablonu kullanmak bağlantıyı başarıyla oluşturur ve Güncelleştirme Yönetimi çözümünü dağıtır. 

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="api-versions"></a>API sürümleri

Aşağıdaki tabloda, bu şablonda kullanılan kaynakların API sürümleri listelenmektedir.

| Kaynak | Kaynak türü | API sürümü |
|:---|:---|:---|
| Çalışma alanı | çalışma alanı | 2017-03-15-Önizleme |
| Otomasyon hesabı | automation | 2015-10-31 | 
| Çözüm | çözümler | 2015-11-01-Önizleme |

## <a name="before-using-the-template"></a>Şablonu kullanmadan önce

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale Azure PowerShell az modülünü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız, Azure ile bağlantı oluşturmak için [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-3.7.0) komutunu da çalıştırmanız gerekir. Azure PowerShell, dağıtım [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment)kullanır.

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.1.0 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Azure CLı ile bu dağıtım [az Group Deployment Create](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)kullanır. 

JSON şablonu şunları isteyecek şekilde yapılandırılır:

* Çalışma alanının adı
* Çalışma alanının oluşturulacağı bölge
* Otomasyon hesabının adı
* Hesabın oluşturulacağı bölge

JSON şablonu, ortamınızda standart bir yapılandırma için kullanılabilecek olabilecek diğer parametreler için varsayılan bir değer belirtir. Şablonu kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında saklayabilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynak dağıtma](../azure-resource-manager/templates/deploy-cli.md).

Şablondaki aşağıdaki parametreler, Log Analytics çalışma alanı için varsayılan bir değerle ayarlanır:

* SKU-Nisan 2018 fiyatlandırma modelinde yayınlanan yeni GB başına fiyatlandırma katmanına varsayılan olarak sahiptir
* veri saklama-varsayılan olarak otuz gün
* Kapasite ayırma-varsayılan olarak 100 GB olur

>[!WARNING]
>Yeni Nisan 2018 fiyatlandırma modelini kabul eden bir abonelikte Log Analytics çalışma alanı oluşturuyor veya yapılandırıyorsanız, geçerli Log Analytics fiyatlandırma katmanı yalnızca **PerGB2018**olur.
>

JSON şablonu, ortamınızda standart bir yapılandırma olarak kullanılacak diğer parametreler için varsayılan bir değer belirtir. Şablonu kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında saklayabilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynak dağıtma](../azure-resource-manager/templates/deploy-cli.md).

Yeni otomasyon hesabınıza bağlı bir Log Analytics çalışma alanı oluşturmaya, yapılandırmaya ve kullanmaya çalışırken hatalardan kaçınmak için, Azure Otomasyonu ve Azure Izleyici 'de yeni olan aşağıdaki yapılandırma ayrıntılarını anlamak önemlidir.

* Erişim denetimi modu, fiyatlandırma katmanı, bekletme ve kapasite ayırma düzeyi gibi çalışma alanı yapılandırma seçeneklerini tam olarak anlamak için [ek ayrıntıları](../azure-monitor/platform/template-workspace-configuration.md#create-a-log-analytics-workspace) gözden geçirin.

* Log Analytics çalışma alanını ve aboneliğinizdeki Otomasyon hesabını bağlamak için yalnızca belirli bölgeler desteklendiğinden, desteklenen bölgeleri satır içi olarak veya bir Parametreler dosyasında belirtmek için [çalışma alanı eşlemelerini](how-to/region-mappings.md) gözden geçirin.

* Azure Izleyici günlükleri ile yeni bir çalışma alanı dağıtmadıysanız, erişim denetimi hakkında bilgi edinmek ve kuruluşunuz için önerdiğimiz tasarım uygulama stratejilerini anlamak için [çalışma alanı tasarım](../azure-monitor/platform/design-logs-deployment.md) kılavuzunu gözden geçirmeniz gerekir.

## <a name="deploy-template"></a>Şablon dağıtma

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
                "description": "Pricing tier: perGB2018 or legacy tiers (Free, Standalone, PerNode, Standard or Premium) which are not available to all customers."
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
        }
    },
    "variables": {
        "Updates": {
            "name": "[concat('Updates', '(', parameters('workspaceName'), ')')]",
            "galleryName": "Updates"
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
                    "apiVersion": "2015-11-01-preview",
                    "location": "[resourceGroup().location]",
                    "name": "[variables('Updates').name]",
                    "type": "Microsoft.OperationsManagement/solutions",
                    "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').name)]",
                    "dependsOn": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    ],
                    "properties": {
                        "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
                    },
                    "plan": {
                        "name": "[variables('Updates').name]",
                        "publisher": "Microsoft",
                        "promotionCode": "",
                        "product": "[concat('OMSGallery/', variables('Updates').galleryName)]"
                    }
                }
            ]
        },
        {
            "type": "Microsoft.Automation/automationAccounts",
            "apiVersion": "2015-01-01-preview",
            "name": "[parameters('automationAccountName')]",
            "location": "[parameters('automationAccountLocation')]",
            "dependsOn": [],
            "tags": {},
            "properties": {
                "sku": {
                    "name": "Basic"
                }
            },
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
    ```

2. Gereksinimlerinizi karşılayacak şekilde şablonu düzenleyin. Parametreleri satır içi değerler olarak geçirmek yerine bir [Kaynak Yöneticisi Parameters dosyası](../azure-resource-manager/templates/parameter-files.md) oluşturmayı düşünün.

3. Bu dosyayı **Deployumsolutiontemplate. JSON**olarak yerel bir klasöre kaydedin.

4. Bu şablonu dağıtmaya hazırsınız. PowerShell veya Azure CLı kullanabilirsiniz. Bir çalışma alanı ve Otomasyon hesabı adı istendiğinde, tüm Azure abonelikleri genelinde genel olarak benzersiz bir ad sağlayın.

    **PowerShell**

    ```powershell
    New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deployUMSolutiontemplate.json
    ```

    **Azure CLI**

    ```cli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deployUMSolutiontemplate.json
    ```

    Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonucu içeren aşağıdakine benzer bir ileti görürsünüz:

    ![Dağıtım tamamlandığında örnek sonuç](media/automation-update-management-deploy-template/template-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Güncelleştirme Yönetimi çözümü dağıttığınıza göre, VM 'Leri yönetim için etkinleştirebilir, güncelleştirme değerlendirmelerini gözden geçirebilir ve güncelleştirmeleri uyumluluğa getirecek şekilde dağıtabilirsiniz.

- Azure [Otomasyonu hesabınızdan](automation-onboard-solutions-from-automation-account.md) bir veya daha fazla Azure makinesi Için ve Azure dışı makineler için el ile.

- Tek bir Azure VM için Azure portal sanal makine sayfasından. Bu senaryo, [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) ve [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management) VM 'leri için kullanılabilir.

- [Birden çok Azure VM](manage-update-multi.md) Için Azure Portal **sanal makineler** sayfasında bunları seçerek. 