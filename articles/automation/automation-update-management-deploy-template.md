---
title: Azure Resource Manager şablonu kullanarak Güncelleştirme Yönetimi etkinleştirme | Microsoft Docs
description: Bu makalede, Güncelleştirme Yönetimi etkinleştirmek için bir Azure Resource Manager şablonunun nasıl kullanılacağı açıklanır.
ms.service: automation
ms.subservice: update-management
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 06/10/2020
ms.openlocfilehash: ad9029b44ffb0c98bad58bbf012eb19d084d5446
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86185764"
---
# <a name="enable-update-management-using-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak Güncelleştirme Yönetimi’ni etkinleştirme

Kaynak grubunuzda Azure Otomasyonu Güncelleştirme Yönetimi özelliğini etkinleştirmek için bir [Azure Resource Manager şablonu](../azure-resource-manager/templates/template-syntax.md) kullanabilirsiniz. Bu makale, aşağıdakileri otomatikleştiren örnek bir şablon sağlar:

* Azure Izleyici Log Analytics çalışma alanı oluşturma.
* Azure Otomasyonu hesabı oluşturma.
* Zaten bağlı değilse, Otomasyon hesabını Log Analytics çalışma alanına bağlama.
* Güncelleştirme Yönetimi etkinleştiriliyor.

Şablon, bir veya daha fazla Azure veya Azure dışı VM üzerinde Güncelleştirme Yönetimi etkinleştirmeyi otomatik hale getirmiyor.

Aboneliğinizde desteklenen bir bölgede dağıtılmış bir Log Analytics çalışma alanı ve Otomasyon hesabı zaten varsa, bunlar bağlanmaz. Bu şablonu kullanmak bağlantıyı başarıyla oluşturur ve Güncelleştirme Yönetimi dağıtır.

## <a name="api-versions"></a>API sürümleri

Aşağıdaki tabloda, bu şablonda kullanılan kaynakların API sürümleri listelenmektedir.

| Resource | Kaynak türü | API sürümü |
|:---|:---|:---|
| Çalışma alanı | çalışma alanı | 2020-03-01-Önizleme |
| Otomasyon hesabı | otomasyon | 2018-06-30 | 
| Çözüm | çözümler | 2015-11-01-Önizleme |

## <a name="before-using-the-template"></a>Şablonu kullanmadan önce

PowerShell 'i yerel olarak yükleyip kullanmayı tercih ederseniz bu makale Azure PowerShell az modülünü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell 'i yerel olarak çalıştırıyorsanız, Azure ile bağlantı oluşturmak için [Connect-AzAccount](/powershell/module/az.accounts/connect-azaccount?view=azps-3.7.0) komutunu da çalıştırmanız gerekir. Azure PowerShell, dağıtım [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment)kullanır.

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.1.0 veya üstünü çalıştırıyor olmanızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli?view=azure-cli-latest). Azure CLı ile bu dağıtım [az Group Deployment Create](/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)kullanır. 

JSON şablonu şunları isteyecek şekilde yapılandırılır:

* Çalışma alanının adı.
* Çalışma alanının oluşturulacağı bölge.
* Kaynak veya çalışma alanı izinlerini etkinleştirmek için.
* Otomasyon hesabının adı.
* Hesabın oluşturulacağı bölge.

JSON şablonu, ortamınızda standart bir yapılandırma için kullanılabilecek olabilecek diğer parametreler için varsayılan bir değer belirtir. Şablonu kuruluşunuzda paylaşılan erişim için bir Azure depolama hesabında saklayabilirsiniz. Şablonlarla çalışma hakkında daha fazla bilgi için bkz. [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynak dağıtma](../azure-resource-manager/templates/deploy-cli.md).

Şablondaki aşağıdaki parametreler, Log Analytics çalışma alanı için varsayılan bir değerle ayarlanır:

* SKU-Nisan 2018 fiyatlandırma modelinde yayınlanan yeni GB başına fiyatlandırma katmanına varsayılan olarak sahiptir
* veri saklama-varsayılan olarak otuz gün

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
        "location": {
            "type": "string",
            "metadata": {
                "description": "Specifies the location in which to create the workspace."
            }
        },
        "resourcePermissions": {
              "type": "bool",
              "metadata": {
                "description": "true to use resource or workspace permissions. false to require workspace permissions."
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
            "apiVersion": "2020-03-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "sku": {
                    "name": "[parameters('sku')]",
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
                    "location": "[parameters('location')]",
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
            "apiVersion": "2018-06-30",
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
            "apiVersion": "2020-03-01-preview",
            "type": "Microsoft.OperationalInsights/workspaces/linkedServices",
            "name": "[concat(parameters('workspaceName'), '/' , 'Automation')]",
            "location": "[parameters('location')]",
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

3. Bu dosyayı **deployUMSolutiontemplate.js**olarak yerel bir klasöre kaydedin.

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

* VM 'Ler için Güncelleştirme Yönetimi kullanmak için bkz. [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md).

* Artık Log Analytics çalışma alanına ihtiyacınız yoksa, [güncelleştirme yönetimi için çalışma alanının Otomasyon hesabından bağlantısını kaldır](automation-unlink-workspace-update-management.md)' daki yönergelere bakın.

* VM 'Leri Güncelleştirme Yönetimi silmek için bkz. [güncelleştirme yönetimi VM 'Leri kaldırma](automation-remove-vms-from-update-management.md).
