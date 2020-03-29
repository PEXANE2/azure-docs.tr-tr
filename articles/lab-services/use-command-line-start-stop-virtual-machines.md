---
title: VM Azure DevTest Laboratuvarlarını başlatmak ve durdurmak için komut satırı araçlarını kullanma
description: Azure DevTest Labs'da sanal makineleri başlatmak ve durdurmak için komut satırı araçlarını nasıl kullanacağınızı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: fd643559a09d5c75aad9be5f35c653994c8488cf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169244"
---
# <a name="use-command-line-tools-to-start-and-stop-azure-devtest-labs-virtual-machines"></a>Azure DevTest Labs sanal makinelerini başlatmak ve durdurmak için komut satırı araçlarını kullanın
Bu makalede, Azure DevTest Labs'daki bir laboratuvarda sanal makineleri başlatmak veya durdurmak için Azure PowerShell veya Azure CLI'yi nasıl kullanacağınızı gösterilmektedir. Bu işlemleri otomatikleştirmek için PowerShell/CLI komut dosyaları oluşturabilirsiniz. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Azure DevTest Labs, hızlı, kolay ve yalın geliştirme/test ortamları oluşturmanın bir yoludur. Maliyeti yönetmeniz, VM'leri hızlı bir şekilde sağlamanızı ve israfı en aza indirmenizi sağlar.  Azure portalında, belirli zamanlarda otomatik olarak başlayıp durmak üzere laboratuvardaki VM'leri yapılandırmanıza olanak tanıyan yerleşik özellikler vardır. 

Ancak, bazı senaryolarda PowerShell/CLI komut dosyalarından VM'lerin başlatılmasını ve durdurulmasını otomatikleştirmek isteyebilirsiniz. Belirli zamanlarda değil, istediğiniz zaman tek tek makineleri başlatma ve durdurma konusunda size biraz esneklik sağlar. Bu görevleri komut dosyalarını kullanarak çalıştırmanın yararlı olacağı durumlardan bazıları şunlardır.

- 3 katmanlı bir uygulamayı bir test ortamının parçası olarak kullanırken, katmanların bir sıra halinde başlatılması gerekir. 
- Para kazanmak için özel bir ölçüt karşılandığında VM'yi kapatın. 
- Akışın başında başlamak için BIR CI/CD iş akışı içinde bir görev olarak kullanın, makine, test makineleri veya altyapı oluşturmak için VM'leri kullanın ve işlem tamamlandığında VM'leri durdurun. Buna bir örnek, Azure DevTest Labs ile özel görüntü fabrikası olacaktır.  

## <a name="azure-powershell"></a>Azure PowerShell

> [!NOTE]
> Aşağıdaki komut dosyasında Azure PowerShell Az modülü kullanılır. 

Aşağıdaki PowerShell komut dosyası bir laboratuvarda bir VM başlatır. [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction?view=azps-1.7.0) bu komut dosyası için birincil odak noktasıdır. **ResourceId** parametresi, laboratuvardaki VM için tam nitelikli kaynak kimliğidir. **Eylem** parametresi, gerekenlere bağlı olarak **Başlat** veya **Durdur** seçeneklerinin ayarlandığı yerdir.

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
[Azure CLI,](/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) DevTest Labs VM'lerinin başlatılmasını ve durdurulmasını otomatikleştirmenin başka bir yoludur. Azure CLI farklı işletim sistemlerine [yüklenebilir.](/cli/azure/install-azure-cli?view=azure-cli-latest) Aşağıdaki komut dosyası, bir vm'yi laboratuvarda başlatma ve durdurma komutları verir. 

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
Bu işlemleri yapmak için Azure portalını kullanmak için aşağıdaki makaleye bakın: [VM'yi yeniden başlatın.](devtest-lab-restart-vm.md)
