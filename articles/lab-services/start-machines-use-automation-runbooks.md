---
title: Azure DevTest Labs'da Otomasyon runbook'larını kullanarak makineleri başlatın
description: Azure Otomasyon runbook'larını kullanarak Azure DevTest Labs'daki bir laboratuvarda sanal makineleri nasıl başlatlayacağınızı öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76166303"
---
# <a name="start-virtual-machines-in-a-lab-in-order-by-using-azure-automation-runbooks"></a>Azure Otomasyon runbook'larını kullanarak sanal makineleri laboratuvarda başlatın
DevTest Labs'ın [otomatik başlatma](devtest-lab-set-lab-policy.md#set-autostart) özelliği, VM'leri belirli bir zamanda otomatik olarak başlayacak şekilde yapılandırmanıza olanak tanır. Ancak, bu özellik makineleri belirli bir sırada başlatmak için desteklemez. Bu tür otomasyonun yararlı olacağı birkaç senaryo vardır.  Bir senaryo, atlama kutusu diğer VM'lere erişim noktası olarak kullanıldığından, bir laboratuardaki Jumpbox VM'nin önce diğer VM'lerden önce başlatılması gerektiğidir.  Bu makalede, komut dosyası çalıştıran bir PowerShell runbook ile bir Azure Otomasyon hesabı nasıl ayarlayabileceğinizi gösterilmektedir. Komut dosyası, komut dosyasını değiştirmek zorunda kalmadan başlangıç sırasını denetlemenize olanak sağlamak için laboratuardaki VM'lerde etiketler kullanır.

## <a name="setup"></a>Kurulum
Bu örnekte, laboratuardaki VM'lerin **Başlangıç Sipariş** etiketinin uygun değerle (0,1,2, vb.) eklenmesi gerekir. -1 olarak başlatılması gerekmeyen herhangi bir makineyi belirleyin.

## <a name="create-an-azure-automation-account"></a>Azure Otomasyonu hesabı oluşturma
[Bu makaledeki](../automation/automation-create-standalone-account.md)yönergeleri izleyerek bir Azure Otomasyon hesabı oluşturun. Hesabı oluştururken **Hesap Olarak Çalıştır** seçeneğini belirleyin. Otomasyon hesabı oluşturulduktan sonra **Modüller** sayfasını açın ve menü çubuğundaki **Azure Modüllerini Güncelleştir'i** seçin. Varsayılan modüller eski birkaç sürümve güncelleştirme olmadan komut dosyası çalışmayabilir.

## <a name="add-a-runbook"></a>Runbook ekleme
Şimdi, otomasyon hesabına bir runbook eklemek için sol menüde **Runbook'ları** seçin. Menüde **runbook ekle'yi** seçin ve [PowerShell runbook oluşturmak](../automation/automation-first-runbook-textual-powershell.md)için yönergeleri izleyin.

## <a name="powershell-script"></a>PowerShell betiği
Aşağıdaki komut dosyası abonelik adını, laboratuar adını parametreler olarak alır. Komut dosyasının akışı, tüm VM'leri laboratuara almak ve ardından VM adlarının ve başlangıç siparişlerinin bir listesini oluşturmak için etiket bilgilerini ayrıştırmaktır. Komut dosyası, VM'ler arasında sırayla yürür ve VM'leri başlatır. Belirli bir sipariş numarasında birden çok VM varsa, PowerShell işleri kullanılarak eş senkronize olarak başlatılırlar. Etiketi olmayan, başlangıç değerini son (10) olarak ayarlayan VM'ler için varsayılan olarak en son başlatılır.  Laboratuvar VM'nin otomatik olarak başlatılmasını istemiyorsa, etiket değerini 11 olarak ayarlayın ve yoksayıldığında.

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

## <a name="create-a-schedule"></a>Zamanlama oluşturma
Bu komut dosyasının her gün çalıştırılması için otomasyon hesabında [bir zamanlama oluşturun.](../automation/shared-resources/schedules.md#creating-a-schedule) Zamanlama oluşturulduktan sonra, [runbook'a bağlayın.](../automation/shared-resources/schedules.md#linking-a-schedule-to-a-runbook) 

Birden çok laboratuvariçeren birden çok abonelik olan büyük ölçekli bir durumda, parametre bilgilerini farklı laboratuarlar için bir dosyada saklayın ve dosyayı tek tek parametreler yerine komut dosyasına geçirin. Komut dosyasının değiştirilmesi gerekir, ancak temel yürütme aynı olacaktır. Bu örnek PowerShell komut dosyasını yürütmek için Azure Otomasyonu'nu kullanırken, Bir Görevin Yap/Bırak ardışık yapısında kullanılması gibi başka seçenekler de vardır.

## <a name="next-steps"></a>Sonraki adımlar
Azure Otomasyonu hakkında daha fazla bilgi edinmek için aşağıdaki makaleye bakın: [Azure Otomasyonuna giriş.](../automation/automation-intro.md)
