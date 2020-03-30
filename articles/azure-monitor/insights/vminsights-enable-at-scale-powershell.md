---
title: PowerShell veya şablonlu VM'ler için Azure Monitörünü etkinleştirme
description: Bu makalede, Bir veya daha fazla Azure sanal makine veya sanal makine ölçek kümeleri için Azure PowerShell veya Azure Kaynak Yöneticisi şablonlarını kullanarak Azure Monitor'u nasıl etkinleştirdiğiniz açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 10/14/2019
ms.openlocfilehash: 75d5203e7c475a44b6a00dbf9286f43114b7b54f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79480853"
---
# <a name="enable-azure-monitor-for-vms-using-azure-powershell-or-resource-manager-templates"></a>Azure PowerShell veya Kaynak Yöneticisi şablonlarını kullanarak VM'ler için Azure Monitörünü etkinleştirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede, Azure PowerShell veya Azure Kaynak Yöneticisi şablonlarını kullanarak Azure sanal makineler veya sanal makine ölçek kümeleri için Sanal Makineler için Azure Monitor'un nasıl etkinleştirilen olduğu açıklanmaktadır. Bu işlemin sonunda, tüm sanal makinelerinizi başarıyla izlemeye başlamış ve performans veya kullanılabilirlik sorunları olup olmadığını öğrenebilirsiniz.

## <a name="set-up-a-log-analytics-workspace"></a>Log Analytics çalışma alanı ayarlama

Log Analytics çalışma alanınız yoksa, bir tane oluşturmanız gerekir. Yapılandırma adımlarını yapılandırmadan önce [Önkoşullar](vminsights-enable-overview.md#log-analytics) bölümünde önerilen yöntemleri gözden geçirin. Ardından, Azure Kaynak Yöneticisi şablon yöntemini kullanarak VM'ler için Azure Monitor dağıtımını tamamlayabilirsiniz.

### <a name="install-the-vminsights-solution"></a>VMInsights çözümlerini yükleyin

Bu yöntem, Log Analytics çalışma alanınızdaki çözüm bileşenlerini etkinleştirmek için yapılandırmayı belirten bir JSON şablonu içerir.

Bir şablon kullanarak kaynakları nasıl dağıtacağını bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)
* [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLI'yi kullanmak için öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.27 veya sonraki sürümde çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemek veya yükseltmek için [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

1. Aşağıdaki JSON söz dizimini kopyalayıp dosyanıza yapıştırın:

    ```json
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "WorkspaceName": {
                "type": "string"
            },
            "WorkspaceLocation": {
                "type": "string"
            }
        },
        "resources": [
            {
                "apiVersion": "2017-03-15-preview",
                "type": "Microsoft.OperationalInsights/workspaces",
                "name": "[parameters('WorkspaceName')]",
                "location": "[parameters('WorkspaceLocation')]",
                "resources": [
                    {
                        "apiVersion": "2015-11-01-preview",
                        "location": "[parameters('WorkspaceLocation')]",
                        "name": "[concat('VMInsights', '(', parameters('WorkspaceName'),')')]",
                        "type": "Microsoft.OperationsManagement/solutions",
                        "dependsOn": [
                            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        ],
                        "properties": {
                            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('WorkspaceName'))]"
                        },

                        "plan": {
                            "name": "[concat('VMInsights', '(', parameters('WorkspaceName'),')')]",
                            "publisher": "Microsoft",
                            "product": "[Concat('OMSGallery/', 'VMInsights')]",
                            "promotionCode": ""
                        }
                    }
                ]
            }
        ]
    }
    ```

1. Bu *dosyayı installsolutionsforvminsights.json* olarak yerel bir klasöre kaydedin.

1. *WorkspaceName,* *ResourceGroupName*ve *WorkspaceLocation*değerlerini yakalayın. *WorkspaceName'nin* değeri, Log Analytics çalışma alanınızın adıdır. *WorkspaceLocation* için değer, çalışma alanının tanımlandığı bölgedir.

1. Bu şablonu dağıtmaya hazırsınız.

    * Şablonu içeren klasörde aşağıdaki PowerShell komutlarını kullanın:

        ```powershell
        New-AzResourceGroupDeployment -Name DeploySolutions -TemplateFile InstallSolutionsForVMInsights.json -ResourceGroupName <ResourceGroupName> -WorkspaceName <WorkspaceName> -WorkspaceLocation <WorkspaceLocation - example: eastus>
        ```

        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Bittiğinde, aşağıdakilere benzer bir ileti görüntülenir ve sonucu içerir:

        ```output
        provisioningState       : Succeeded
        ```

    * Azure CLI'yi kullanarak aşağıdaki komutu çalıştırmak için:

        ```azurecli
        az login
        az account set --subscription "Subscription Name"
        az group deployment create --name DeploySolutions --resource-group <ResourceGroupName> --template-file InstallSolutionsForVMInsights.json --parameters WorkspaceName=<workspaceName> WorkspaceLocation=<WorkspaceLocation - example: eastus>
        ```

        Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Bittiğinde, aşağıdakilere benzer ve sonucu içeren bir ileti görüntülenir:

        ```output
        provisioningState       : Succeeded
        ```

## <a name="enable-with-azure-resource-manager-templates"></a>Azure Kaynak Yöneticisi şablonları ile etkinleştirme

Sanal makinelerinize ve sanal makine ölçek kümelerine binmek için örnek Azure Kaynak Yöneticisi şablonları oluşturduk. Bu şablonlar, varolan bir kaynakta izlemeyi etkinleştirmek ve izlemeyi etkinleştirmiş yeni bir kaynak oluşturmak için kullanabileceğiniz senaryoları içerir.

>[!NOTE]
>Şablonun, gemiye getirilecek kaynakla aynı kaynak grubunda dağıtılması gerekir.

Bir şablon kullanarak kaynakları nasıl dağıtacağını bilmiyorsanız, bkz:
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/templates/deploy-powershell.md)
* [Kaynak Yöneticisi şablonları ve Azure CLI ile kaynakları dağıtma](../../azure-resource-manager/templates/deploy-cli.md)

Azure CLI'yi kullanmak için öncelikle CLI'yi yerel olarak yüklemeniz ve kullanmanız gerekir. Azure CLI sürümünü 2.0.27 veya sonraki sürümde çalışıyor olmalısınız. Sürümünüzü tanımlamak için `az --version`çalıştırın. Azure CLI'yi yüklemek veya yükseltmek için [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

### <a name="download-templates"></a>Şablonları indirin

Azure Kaynak Yöneticisi şablonları, GitHub repomuzdan [indirebileceğiniz](https://aka.ms/VmInsightsARMTemplates) bir arşiv dosyasında (.zip) sağlanır. Dosyanın içeriği, her dağıtım senaryosunu bir şablon ve parametre dosyasıyla temsil eden klasörleri içerir. Çalıştırmadan önce, parametreler dosyasını değiştirin ve gerekli değerleri belirtin. Belirli gereksinimlerinizi destekleyecek şekilde özelleştirmeniz gerekmedikçe şablon dosyasını değiştirmeyin. Parametre dosyasını değiştirdikten sonra, bu makalede daha sonra açıklanan aşağıdaki yöntemleri kullanarak dosyayı dağıtabilirsiniz.

İndirme dosyası farklı senaryolar için aşağıdaki şablonları içerir:

- **ExistingVmOnboarding** şablonu, sanal makine zaten varsa VM'ler için Azure Monitor'a olanak tanır.
- **NewVmOnboarding** şablonu sanal bir makine oluşturur ve Sanal Makineler için Azure Monitor'un bunu izlemesini sağlar.
- **ExistingVmssOnboarding** şablonu, sanal makine ölçeği kümesi zaten varsa VM'ler için Azure Monitor'a olanak tanır.
- **NewVmssOnboarding** şablonu sanal makine ölçeği kümeleri oluşturur ve Sanal Ayarlar için Azure Monitor'un bunları izlemesini sağlar.
- **ConfigureWorkspace** şablonu, Linux ve Windows işletim sistemi performans sayaçlarının çözümlerini ve koleksiyonunu etkinleştirerek Log Analytics çalışma alanınızı VM'ler için Azure Monitor'u destekleyecek şekilde yapılandırır.

>[!NOTE]
>Sanal makine ölçeği kümeleri zaten mevcutsa ve yükseltme ilkesi **El Ile**ayarlanmışsa, VM'ler için Azure Monitörü, **ExistingVmssOnboarding** Azure Kaynak Yöneticisi şablonu çalıştırıldıktan sonra varsayılan olarak örneğin etkinleştirilmez. Örnekleri el ile yükseltmeniz gerekir.

### <a name="deploy-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak dağıtma

Aşağıdaki adım, Azure PowerShell'i kullanarak izlemeyi sağlar.

```powershell
New-AzResourceGroupDeployment -Name OnboardCluster -ResourceGroupName <ResourceGroupName> -TemplateFile <Template.json> -TemplateParameterFile <Parameters.json>
```
Yapılandırma değişikliğinin tamamlanması birkaç dakika sürebilir. Bittiğinde, aşağıdakilere benzer bir ileti görüntülenir ve sonucu içerir:

```output
provisioningState       : Succeeded
```

### <a name="deploy-by-using-the-azure-cli"></a>Azure CLI'yi kullanarak dağıtma

Aşağıdaki adım, Azure CLI'yi kullanarak izlemeyi sağlar.

```azurecli
az login
az account set --subscription "Subscription Name"
az group deployment create --resource-group <ResourceGroupName> --template-file <Template.json> --parameters <Parameters.json>
```

Çıktı aşağıdakilere benzer:

```output
provisioningState       : Succeeded
```

## <a name="enable-with-powershell"></a>PowerShell ile etkinleştirin

Birden çok VM veya sanal makine ölçeği kümesi için VM'ler için Azure Monitörünü etkinleştirmek için PowerShell komut dosyası [Install-VMInsights.ps1'i](https://www.powershellgallery.com/packages/Install-VMInsights/1.0)kullanın. Azure PowerShell Galerisi'nden edinilebilir. Bu komut dosyası ile yineler:

- Aboneliğinizde belirlenen her sanal makine ve sanal makine ölçeği.
- *Kaynak Grubu*tarafından belirtilen kapsamlı kaynak grubu.
- *Ad*tarafından belirtilen tek bir VM veya sanal makine ölçeği kümesi.

Her VM veya sanal makine ölçeği kümesi için komut dosyası, VM uzantısının zaten yüklü olup olmadığını doğrular. VM uzantısı yüklüyse, komut dosyası yeniden yüklemeye çalışır. VM uzantısı yüklü değilse, komut dosyası Log Analytics ve Bağımlılık aracısı VM uzantıları yükler.

Azure PowerShell modülü Az sürüm 1.0.0 veya `Enable-AzureRM` daha sonra uyumluluk takma adları etkinleştirilmiş olarak kullandığınızı doğrulayın. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, Azure `Connect-AzAccount` ile bağlantı oluşturmak için de çalışmanız gerekir.

Komut dosyasının bağımsız değişken ayrıntılarının ve örnek `Get-Help`kullanımının listesini almak için çalıştırın.

```powershell
Get-Help .\Install-VMInsights.ps1 -Detailed

SYNOPSIS
    This script installs VM extensions for Log Analytics and the Dependency agent as needed for VM Insights.


SYNTAX
    .\Install-VMInsights.ps1 [-WorkspaceId] <String> [-WorkspaceKey] <String> [-SubscriptionId] <String> [[-ResourceGroup]
    <String>] [[-Name] <String>] [[-PolicyAssignmentName] <String>] [-ReInstall] [-TriggerVmssManualVMUpdate] [-Approve] [-WorkspaceRegion] <String>
    [-WhatIf] [-Confirm] [<CommonParameters>]


DESCRIPTION
    This script installs or reconfigures the following on VMs and virtual machine scale sets:
    - Log Analytics VM extension configured to supplied Log Analytics workspace
    - Dependency agent VM extension

    Can be applied to:
    - Subscription
    - Resource group in a subscription
    - Specific VM or virtual machine scale set
    - Compliance results of a policy for a VM or VM extension

    Script will show you a list of VMs or virtual machine scale sets that will apply to and let you confirm to continue.
    Use -Approve switch to run without prompting, if all required parameters are provided.

    If the extensions are already installed, they will not install again.
    Use -ReInstall switch if you need to, for example, update the workspace.

    Use -WhatIf if you want to see what would happen in terms of installs, what workspace configured to, and status of the extension.


PARAMETERS
    -WorkspaceId <String>
        Log Analytics WorkspaceID (GUID) for the data to be sent to

    -WorkspaceKey <String>
        Log Analytics Workspace primary or secondary key

    -SubscriptionId <String>
        SubscriptionId for the VMs/VM Scale Sets
        If using PolicyAssignmentName parameter, subscription that VMs are in

    -ResourceGroup <String>
        <Optional> Resource Group to which the VMs or VM Scale Sets belong

    -Name <String>
        <Optional> To install to a single VM/VM Scale Set

    -PolicyAssignmentName <String>
        <Optional> Take the input VMs to operate on as the Compliance results from this Assignment
        If specified will only take from this source.

    -ReInstall [<SwitchParameter>]
        <Optional> If VM/VM Scale Set is already configured for a different workspace, set this to change to the new workspace

    -TriggerVmssManualVMUpdate [<SwitchParameter>]
        <Optional> Set this flag to trigger update of VM instances in a scale set whose upgrade policy is set to Manual

    -Approve [<SwitchParameter>]
        <Optional> Gives the approval for the installation to start with no confirmation prompt for the listed VMs/VM Scale Sets

    -WorkspaceRegion <String>
        Region the Log Analytics Workspace is in
        Supported values: "East US","eastus","Southeast Asia","southeastasia","West Central US","westcentralus","West Europe","westeurope"
        For Health supported is: "East US","eastus","West Central US","westcentralus"

    -WhatIf [<SwitchParameter>]
        <Optional> See what would happen in terms of installs.
        If extension is already installed will show what workspace is currently configured, and status of the VM extension

    -Confirm [<SwitchParameter>]
        <Optional> Confirm every action

    <CommonParameters>
        This cmdlet supports the common parameters: Verbose, Debug,
        ErrorAction, ErrorVariable, WarningAction, WarningVariable,
        OutBuffer, PipelineVariable, and OutVariable. For more information, see
        about_CommonParameters (https:/go.microsoft.com/fwlink/?LinkID=113216).

    -------------------------- EXAMPLE 1 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup>

    Install for all VMs in a resource group in a subscription

    -------------------------- EXAMPLE 2 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -ResourceGroup <ResourceGroup> -ReInstall

    Specify to reinstall extensions even if already installed, for example, to update to a different workspace

    -------------------------- EXAMPLE 3 --------------------------
    .\Install-VMInsights.ps1 -WorkspaceRegion eastus -WorkspaceId <WorkspaceId>-WorkspaceKey <WorkspaceKey> -SubscriptionId <SubscriptionId>
    -PolicyAssignmentName a4f79f8ce891455198c08736 -ReInstall

    Specify to use a PolicyAssignmentName for source and to reinstall (move to a new workspace)
```

Aşağıdaki örnek, VM'ler için Azure Monitor'u etkinleştirmek ve beklenen çıktıyı anlamak için klasördeki PowerShell komutlarının kullanılmasını gösterir:

```powershell
$WorkspaceId = "<GUID>"
$WorkspaceKey = "<Key>"
$SubscriptionId = "<GUID>"
.\Install-VMInsights.ps1 -WorkspaceId $WorkspaceId -WorkspaceKey $WorkspaceKey -SubscriptionId $SubscriptionId -WorkspaceRegion eastus

Getting list of VMs or virtual machine scale sets matching criteria specified

VMs or virtual machine scale sets matching criteria:

db-ws-1 VM running
db-ws2012 VM running

This operation will install the Log Analytics and Dependency agent extensions on the previous two VMs or virtual machine scale sets.
VMs in a non-running state will be skipped.
Extension will not be reinstalled if already installed. Use -ReInstall if desired, for example, to update workspace.

Confirm
Continue?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

db-ws-1 : Deploying DependencyAgentWindows with name DAExtension
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Deploying DependencyAgentWindows with name DAExtension
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Deploying MicrosoftMonitoringAgent with name MMAExtension
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Summary:

Already onboarded: (0)

Succeeded: (4)
db-ws-1 : Successfully deployed DependencyAgentWindows
db-ws-1 : Successfully deployed MicrosoftMonitoringAgent
db-ws2012 : Successfully deployed DependencyAgentWindows
db-ws2012 : Successfully deployed MicrosoftMonitoringAgent

Connected to different workspace: (0)

Not running - start VM to configure: (0)

Failed: (0)
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal makineleriniz için izleme etkinleştirildiğinden, bu bilgiler Sanal Makineler için Azure Monitor ile analiz edilebilir.

- Keşfedilen uygulama bağımlılıklarını görüntülemek [için, VM'ler Haritası için Azure Monitörünü Görüntüle'ye](vminsights-maps.md)bakın.

- VM'nizin performansıyla birlikte darboğazları ve genel kullanımı belirlemek için [bkz.](vminsights-performance.md)
