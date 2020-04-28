---
title: Azure DevTest Labs 'de Automation runbook 'ları kullanarak makineler başlatma
description: Azure Otomasyonu runbook 'ları kullanarak Azure DevTest Labs laboratuvarda sanal makineleri başlatmayı öğrenin.
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
ms.openlocfilehash: 9bb97a73b7ca570ca122323e8e9c5a70c9348b15
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76166303"
---
# <a name="start-virtual-machines-in-a-lab-in-order-by-using-azure-automation-runbooks"></a>Azure Otomasyonu runbook 'larını kullanarak bir laboratuvarda sanal makine başlatma
DevTest Labs 'in [autostart](devtest-lab-set-lab-policy.md#set-autostart) özelliği, VM 'leri belirli bir zamanda otomatik olarak başlayacak şekilde yapılandırmanıza olanak tanır. Ancak, bu özellik makineleri belirli bir sırada başlatılacak şekilde desteklemez. Bu tür bir Otomasyon yararlı olacağı birçok senaryo vardır.  Bir senaryo, diğer VM 'lere erişim noktası olarak, diğer VM 'lerden önce, bir laboratuar içindeki bir sıçrama kutusu VM 'sinin ilk başlatılmalıdır.  Bu makalede, bir betiği yürüten PowerShell runbook 'u ile bir Azure Otomasyonu hesabının nasıl ayarlanacağı gösterilmektedir. Betiği, betiği değiştirmek zorunda kalmadan başlangıç sırasını denetlemenize olanak tanımak için laboratuvardaki VM 'lerde Etiketler kullanır.

## <a name="setup"></a>Kurulum
Bu örnekte, laboratuvardaki VM 'Lerin, uygun değer (0, 1, 2, vb.) **ile eklenmesi gerekir** . 1 olarak başlatılması gerekmeyen herhangi bir makineyi belirleyin.

## <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma
[Bu makaledeki](../automation/automation-create-standalone-account.md)yönergeleri Izleyerek bir Azure Otomasyonu hesabı oluşturun. Hesabı oluştururken **Farklı Çalıştır hesapları** seçeneğini belirleyin. Otomasyon hesabı oluşturulduktan sonra, **modüller** sayfasını açın ve menü çubuğunda **Azure modüllerini Güncelleştir** ' i seçin. Varsayılan modüller, bazı sürümlerdir ve güncelleştirme olmadan betiği çalışmayabilir.

## <a name="add-a-runbook"></a>Runbook ekleme
Şimdi Otomasyon hesabına bir runbook eklemek için soldaki menüden **runbook 'lar** ' ı seçin. Menüdeki **runbook Ekle** ' yi seçin ve [PowerShell runbook 'u oluşturmak](../automation/automation-first-runbook-textual-powershell.md)için yönergeleri izleyin.

## <a name="powershell-script"></a>PowerShell betiği
Aşağıdaki betik, abonelik adını, laboratuvar adını parametreler olarak alır. Betiğin akışı, laboratuvardaki tüm VM 'Leri almak ve ardından, VM adlarının ve başlangıç sırasının bir listesini oluşturmak için etiket bilgilerini ayrıştırmaktır. Betik VM 'leri sırayla gösterir ve VM 'Leri başlatır. Belirli bir sıra numarasında birden fazla VM varsa, PowerShell işleri kullanılarak zaman uyumsuz olarak başlatılır. Etiketi olmayan bu VM 'Ler için, başlangıç değerini en son (10) olarak ayarlayın, varsayılan olarak en son başlatılır.  Laboratuvar, VM 'nin otomatik olarak başlatılmasını istemiyorsanız, etiket değerini 11 olarak ayarlayın ve yok sayılır.

```powershell
#Requires -Version 3.0
#Requires -Module AzureRM.Resources

param
(
    [Parameter(Mandatory=$false, HelpMessage="Name of the subscription that has the lab")]
    [string] $SubscriptionName,

    [Parameter(Mandatory=$false, HelpMessage="Lab name")]
    [string] $LabName
)

# Connect and add the appropriate subscription
$Conn = Get-AutomationConnection -Name AzureRunAsConnection

Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationId -Subscription $SubscriptionName -CertificateThumbprint $Conn.CertificateThumbprint

# Find the lab
$dtLab = Find-AzResource -ResourceType 'Microsoft.DevTestLab/labs' -ResourceNameEquals $LabName

# Get the VMs
$dtlAllVms = New-Object System.Collections.ArrayList
$AllVMs = Get-AzResource -ResourceId "$($dtLab.ResourceId)/virtualmachines" -ApiVersion 2016-05-15

# Get the StartupOrder tag, if missing set to be run last (10)
ForEach ($vm in $AllVMs) {
    if ($vm.Tags) {
        if ($vm.Tags['StartupOrder']) {
            $startupValue = $vm.Tags['StartupOrder']
        } else {
            $startupValue = 10
        }
        } else {
            $startupValue = 10
        }
        $dtlAllVms.Add(@{$vm.Name = $startupValue}) > $null
}

# Setup for the async multiple vm start

# Save profile
$profilePath = Join-Path $env:Temp "profile.json"
If (Test-Path $profilePath){
    Remove-Item $profilePath
}
Save-AzContext -Path $profilePath

# Job to start VMs asynch
$startVMBlock = {
    Param($devTestLab,$vmToStart,$profilePath)
    Import-AzContext -Path ($profilePath)
    Invoke-AzResourceAction `
        -ResourceId "$($devTestLab.ResourceId)/virtualmachines/$vmToStart" `
        -Action Start `
        -Force
    Write-Output "Started: $vmToStart"
}

$current = 0
# Start in order from 0 to 10

While ($current -le 10) {
# Get the VMs in the current stage
    $tobeStarted = $null
    $tobeStarted = $dtlAllVms | Where-Object { $_.Values -eq $current}
    if ($tobeStarted.Count -eq 1) {
        # Run sync – jobs not necessary for a single VM
        $returnStatus = Invoke-AzResourceAction `
                -ResourceId "$($dtLab.ResourceId)/virtualmachines/$($tobeStarted.Keys)" `
                -Action Start `
                -Force
        Write-Output "$($tobeStarted.Keys) status: $($returnStatus.status)"
    } elseif ($tobeStarted.Count -gt 1) {
        # Start multiple VMs async
        $jobs = @()
        Write-Output "Start Jobs start: $(Get-Date)"
        
        # Jobs
        $jobs += Start-Job -ScriptBlock $startVMBlock -ArgumentList $dtLab, $($singlevm.Keys), $profilePath
        Write-Output "Start Jobs end: $(Get-Date)"
    }

    # Get results from all jobs
    if($jobs.Count -ne 0) {
        Write-Output "Receive Jobs start: $(Get-Date)"
        foreach ($job in $jobs){
            $jobResult = Receive-Job -Job $job -Wait | Write-Output
        }
        Remove-Job -Job $jobs -Force
    }
    else
    {
        Write-Output "Information: No jobs available"
    }
}
```

## <a name="create-a-schedule"></a>Zamanlama Oluştur
Bu betiğin günlük olarak yürütülmesi için Otomasyon hesabında [bir zamanlama oluşturun](../automation/shared-resources/schedules.md#creating-a-schedule) . Zamanlama oluşturulduktan sonra [runbook 'a bağlayın](../automation/shared-resources/schedules.md#linking-a-schedule-to-a-runbook). 

Birden çok laboratuvarı olan birden çok aboneliğin olduğu büyük ölçekli bir durumda, parametre bilgilerini farklı laboratuvarlara yönelik bir dosyada depolayın ve tek tek parametreler yerine dosyayı betiğe geçirin. Betiğin değiştirilmesi gerekir, ancak çekirdek yürütme aynı olacaktır. Bu örnek, PowerShell betiğini yürütmek için Azure Otomasyonu 'nu kullanırken, bir yapı/yayın işlem hattında görev kullanma gibi başka seçenekler de vardır.

## <a name="next-steps"></a>Sonraki adımlar
Azure Otomasyonu hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın: [Azure Otomasyonu 'na giriş](../automation/automation-intro.md).
