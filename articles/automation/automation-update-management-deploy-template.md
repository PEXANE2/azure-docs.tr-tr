---
title: Yerleşik Güncelleştirme Yönetimi için Azure Kaynak Yöneticisi şablonlarını kullanma | Microsoft Dokümanlar
description: Azure Otomasyon Güncelleştirme Yönetimi çözümünde bir Azure Kaynak Yöneticisi şablonu kullanabilirsiniz.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 02/27/2020
ms.openlocfilehash: a8b382663b56d7481da876979e33194fb0ac533d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77925806"
---
# <a name="onboard-update-management-solution-using-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak Yerleşik Güncelleştirme Yönetimi çözümü

Kaynak grubunuzdaki Azure Otomasyon Güncelleştirme Yönetimi çözümünüzü etkinleştirmek için [Azure Kaynak Yöneticisi şablonlarını](../azure-resource-manager/templates/template-syntax.md) kullanabilirsiniz. Bu makalede, aşağıdakileri otomatikleştiren bir örnek şablon uymaktadır:

* Azure Monitörü Log Analytics çalışma alanı oluşturulması.
* Azure Otomasyon hesabı oluşturma.
* Otomasyon hesabını, zaten bağlı değilse, Log Analytics çalışma alanına bağlar.
* Azure Otomasyon Güncelleme Yönetimi çözümünde yerleşik

Şablon, bir veya daha fazla Azure veya Azure olmayan VM'lerin biniş kısmını otomatikleştirmez.

Aboneliğinizde desteklenen bir bölgede zaten bir Log Analytics çalışma alanı ve Otomasyon hesabınız varsa, bunlar bağlı değildir ve çalışma alanı güncelleştirme yönetimi çözümünüzü zaten dağıtmamışsa, bu şablonu kullanarak başarılı bir şekilde oluşturulur bağlantı ve Güncelleştirme Yönetimi çözümdağıdağı. 

## <a name="api-versions"></a>API sürümleri

Aşağıdaki tabloda, bu örnekte kullanılan kaynakların API sürümü listeleneb.r.a.

| Kaynak | Kaynak türü | API sürümü |
|:---|:---|:---|
| Çalışma alanı | çalışma alanı | 2017-03-15-önizleme |
| Otomasyon hesabı | automation | 2015-10-31 | 
| Çözüm | çözümler | 2015-11-01-önizleme |

## <a name="before-using-the-template"></a>Şablonu kullanmadan önce

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell Az modülü gerekir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir. Azure PowerShell ile dağıtım, [Yeni-AzResourceGroupDeployment'ı](/powershell/module/az.resources/new-azresourcegroupdeployment)kullanır.

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürüm 2.1.0 veya sonraki sürümlerini çalıştırdığınız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Azure CLI ile bu dağıtım [az grup dağıtımı oluşturma yı](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)kullanır. 

JSON şablonu, aşağıdakiler için sizden istenmesi için yapılandırılmıştır:

* Çalışma alanının adı
* Çalışma alanını oluşturacak bölge
* Otomasyon hesabının adı
* Hesap oluşturmak için bölge

JSON şablonu, ortamınızda standart yapılandırma olarak kullanılabilecek diğer parametreler için varsayılan bir değer belirtir. Kuruluşunuzdaki paylaşılan erişim için şablonu bir Azure depolama hesabında depolayabilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıt'a](../azure-resource-manager/templates/deploy-cli.md)bakın.

Şablondaki aşağıdaki parametreler, Log Analytics çalışma alanı için varsayılan bir değerle ayarlanır:

* sku - Nisan 2018 fiyatlandırma modelinde yayımlanan yeni GB Başına fiyatlandırma katmanı için varsayılan
* veri saklama - varsayılan otuz gün

>[!WARNING]
>Yeni Nisan 2018 fiyatlandırma modeline dahil olan bir abonelikte Log Analytics çalışma alanı oluşturmak veya yapılandırmak ise, tek geçerli Log Analytics fiyatlandırma katmanı **PerGB2018'dir.**
>

>[!NOTE]
>Bu şablonu kullanmadan önce, erişim denetim modu, fiyatlandırma katmanı, bekletme ve kapasite rezervasyon düzeyi gibi çalışma alanı yapılandırma seçeneklerini tam olarak anlamak için [ek ayrıntıları](../azure-monitor/platform/template-workspace-configuration.md#create-a-log-analytics-workspace) gözden geçirin. Azure Monitor günlüklerinde yeniyseniz ve daha önce bir çalışma alanı dağıtmadıysanız, erişim denetimi hakkında bilgi edinmek için [çalışma alanı tasarım](../azure-monitor/platform/design-logs-deployment.md) kılavuzunu ve kuruluşunuz için önerdiğimiz tasarım uygulama stratejilerinin anlaşılmasını gözden geçirmelisiniz.

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
        "pricingTier": {
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

2. Gereksinimlerinizi karşılamak için şablonu edin.

3. Bu dosyayı deployUMSolutiontemplate.json olarak yerel bir klasöre kaydedin.

4. Bu şablonu dağıtmaya hazırsınız. PowerShell veya Azure CLI'yi kullanabilirsiniz. Bir çalışma alanı ve Otomasyon hesap adı istendiğinde, tüm Azure aboneliklerinde genel olarak benzersiz bir ad sağlayın.

    **Powershell**

    ```powershell
    New-AzResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile deployUMSolutiontemplate.json
    ```

    **Azure CLI**

    ```cli
    az group deployment create --resource-group <my-resource-group> --name <my-deployment-name> --template-file deployUMSolutiontemplate.json
    ```

    Dağıtımın tamamlanması birkaç dakika sürebilir. Bittiğinde, sonucu içeren aşağıdakine benzer bir ileti görürsünüz:

    ![Dağıtım tamamlandığında örnek sonuç](media/automation-update-management-deploy-template/template-output.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık Güncelleştirme Yönetimi çözümlerini dağıttığınız için, yönetim için VM'leri etkinleştirebilir, güncelleştirme değerlendirmelerini gözden geçirebilir ve güncelleştirmeleri uyumlu hale getirmek için güncelleştirmeleri dağıtabilirsiniz.

- Azure [Otomasyon uyupunuzdan](automation-onboard-solutions-from-automation-account.md) bir veya daha fazla Azure makinesi ve Azure olmayan makineler için el ile.

- Azure portalındaki sanal makine sayfasından tek bir Azure VM için. Bu senaryo [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) ve [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management) VM'leri için kullanılabilir.

- Azure portalındaki **Sanal makineler** sayfasından seçerek birden çok [Azure VM'si](manage-update-multi.md) için. 