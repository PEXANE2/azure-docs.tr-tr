---
title: VM 'Leri başlatmak ve durdurmak için komut satırı araçlarını kullanın Azure DevTest Labs
description: Azure DevTest Labs ' de sanal makineleri başlatmak ve durdurmak için komut satırı araçlarını kullanmayı öğrenin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: e5a42658f2b83f101271f158c9af70833601b56d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85476425"
---
# <a name="use-command-line-tools-to-start-and-stop-azure-devtest-labs-virtual-machines"></a>Azure DevTest Labs sanal makineleri başlatmak ve durdurmak için komut satırı araçlarını kullanma
Bu makalede, Azure DevTest Labs bir laboratuvardaki sanal makineleri başlatmak veya durdurmak için Azure PowerShell veya Azure CLı kullanarak nasıl kullanılacağı gösterilmektedir. Bu işlemleri otomatikleştirmek için PowerShell/CLı betikleri oluşturabilirsiniz. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Azure DevTest Labs hızlı, kolay ve yalın geliştirme ve test ortamları oluşturmanın bir yoludur. Maliyeti yönetmenize, hızlı bir şekilde VM sağlamanıza ve atık durumuna küçültmenize olanak tanır.  Azure portal, bir laboratuvardaki VM 'Leri otomatik olarak başlatıp durdurmak üzere yapılandırmanıza olanak sağlayan yerleşik özellikler vardır. 

Ancak bazı senaryolarda, PowerShell/CLı betiklerinden VM 'Leri başlatma ve durdurma işlemlerini otomatik hale getirmek isteyebilirsiniz. Belirli zamanlarda değil, bireysel makineleri dilediğiniz zaman başlatıp durdurmakla ilgili bir esneklik sunar. Komut dosyalarını kullanarak bu görevleri çalıştırmanın yararlı olacağı durumlardan bazıları şunlardır.

- Bir test ortamının parçası olarak 3 katmanlı bir uygulama kullanırken, katmanların bir sırayla başlatılması gerekir. 
- Para tasarrufu için özel bir ölçüt karşılandığında bir VM 'yi kapatın. 
- Akışın başlangıcında başlamak için bir CI/CD iş akışında bir görev olarak kullanın, VM 'Leri derleme makineleri, test makineleri veya altyapı olarak kullanın, sonra işlem tamamlandığında VM 'Leri durdurun. Bunun bir örneği, Azure DevTest Labs ile özel görüntü fabrikası olacaktır.  

## <a name="azure-powershell"></a>Azure PowerShell

> [!NOTE]
> Aşağıdaki betik Azure PowerShell az Module kullanır. 

Aşağıdaki PowerShell betiği laboratuvara bir sanal makine başlatır. [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction?view=azps-1.7.0) , bu betiğin birincil odadır. **RESOURCEID** parametresi, LABORATUVARA sanal makine için tam kaynak kimliğidir. **Eylem** parametresi, gerekli olan seçeneğe göre **Başlangıç** veya **durdurma** seçeneklerinin ayarlandığı yerdir.

```powershell
# The id of the subscription
$subscriptionId = "111111-11111-11111-1111111"

# The name of the lab
$devTestLabName = "yourlabname"

# The name of the virtual machine to be started
$vMToStart = "vmname"

# The action on the virtual machine (Start or Stop)
$vmAction = "Start"

# Select the Azure subscription
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab information
$devTestLab = Get-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceName $devTestLabName

# Start the VM and return a succeeded or failed status
$returnStatus = Invoke-AzResourceAction `
                    -ResourceId "$($devTestLab.ResourceId)/virtualmachines/$vMToStart" `
                    -Action $vmAction `
                    -Force

if ($returnStatus.Status -eq 'Succeeded') {
    Write-Output "##[section] Successfully updated DTL machine: $vMToStart, Action: $vmAction"
}
else {
    Write-Error "##[error]Failed to update DTL machine: $vMToStart, Action: $vmAction"
}
```


## <a name="azure-cli"></a>Azure CLI
[Azure CLI](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) , DevTest Labs VM 'lerinin başlatılmasına ve durdurulmasına otomatik hale getirmenin bir başka yoludur. Azure CLı, farklı işletim sistemlerine [yüklenebilir](/cli/azure/install-azure-cli?view=azure-cli-latest) . Aşağıdaki betik, laboratuvara bir sanal makineyi başlatma ve durdurma komutları sağlar. 

```azurecli
# Sign in to Azure
az login 

## Get the name of the resource group that contains the lab
az resource list --resource-type "Microsoft.DevTestLab/labs" --name "yourlabname" --query "[0].resourceGroup" 

## Start the VM
az lab vm start --lab-name yourlabname --name vmname --resource-group labResourceGroupName

## Stop the VM
az lab vm stop --lab-name yourlabname --name vmname --resource-group labResourceGroupName
```


## <a name="next-steps"></a>Sonraki adımlar
Bu işlemleri yapmak için Azure portal kullanmak için aşağıdaki makaleye bakın: [bir VM 'Yi yeniden başlatın](devtest-lab-restart-vm.md).
