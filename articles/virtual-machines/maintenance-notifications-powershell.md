---
title: PowerShell kullanarak Azure VM 'Leri için bakım bildirimleri alın
description: Azure 'da çalışan sanal makineler için bakım bildirimlerini görüntüleyin ve PowerShell kullanarak Self servis bakımı başlatın.
services: virtual-machines
documentationcenter: ''
author: shants123
editor: ''
tags: azure-service-management,azure-resource-manager
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: f136fc3001e6ae224e264a59ceba66ed61ead865
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/26/2019
ms.locfileid: "74535840"
---
# <a name="handling-planned-maintenance-using-powershell"></a>PowerShell kullanarak planlı Bakımı işleme

**Bu makale hem Linux hem de Windows çalıştıran sanal makineler için geçerlidir.**

VM 'Lerin [bakım](maintenance-notifications.md)için ne zaman zamanlandığını görmek Için Azure PowerShell 'i kullanabilirsiniz. Planlı bakım bilgileri, `-status` parametresini kullandığınızda [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) cmdlet 'inden kullanılabilir.
  
Bakım bilgileri yalnızca bakım planlandı durumunda döndürülür. VM 'yi etkileyen bakım zamanlanmamışsa, cmdlet herhangi bir bakım bilgisi döndürmez. 


```powershell
Get-AzVM -ResourceGroupName myResourceGroup -Name myVM -Status
```

Aşağıdaki özellikler MaintenanceRedeployStatus altında döndürülür: 

| Value | Açıklama   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Şu anda VM 'de bakım başlatılıp başlatılmayacağını belirtir |
| PreMaintenanceWindowStartTime         | VM 'niz üzerinde bakım başlatabilmeniz için bakım self servis penceresinin başlangıcı |
| PreMaintenanceWindowEndTime           | VM 'niz üzerinde bakım başlatabilmeniz için bakım self servis penceresinin sonu |
| MaintenanceWindowStartTime            | Azure 'un VM 'niz üzerinde bakım başlattığı zamanlanan bakım başlangıcı |
| MaintenanceWindowEndTime              | Azure 'un VM 'niz üzerinde Bakımı başlattığı bakım zamanlandı penceresinin sonu |
| LastOperationResultCode               | VM 'de son bakım başlatma denemesinin sonucu |



Ayrıca, [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) ' i kullanarak bir kaynak grubundaki tüm VM 'lerin bakım durumunu alabılır ve VM belirtmemelidir.
 
```powershell
Get-AzVM -ResourceGroupName myResourceGroup -Status
```

Aşağıdaki PowerShell örneği, abonelik KIMLIĞINIZI alır ve bakım için zamanlanmış VM 'lerin bir listesini döndürür.

```powershell

function MaintenanceIterator
{
    Select-AzSubscription -SubscriptionId $args[0]

    $rgList= Get-AzResourceGroup 

    for ($rgIdx=0; $rgIdx -lt $rgList.Length ; $rgIdx++)
    {
        $rg = $rgList[$rgIdx]        
    $vmList = Get-AzVM -ResourceGroupName $rg.ResourceGroupName 
        for ($vmIdx=0; $vmIdx -lt $vmList.Length ; $vmIdx++)
        {
            $vm = $vmList[$vmIdx]
            $vmDetails = Get-AzVM -ResourceGroupName $rg.ResourceGroupName -Name $vm.Name -Status
              if ($vmDetails.MaintenanceRedeployStatus )
            {
                Write-Output "VM: $($vmDetails.Name)  IsCustomerInitiatedMaintenanceAllowed: $($vmDetails.MaintenanceRedeployStatus.IsCustomerInitiatedMaintenanceAllowed) $($vmDetails.MaintenanceRedeployStatus.LastOperationMessage)"               
            }
          }
    }
}

```

### <a name="start-maintenance-on-your-vm-using-powershell"></a>PowerShell kullanarak VM 'niz üzerinde bakım başlatma

Önceki bölümde bulunan işlevin bilgilerini kullanarak, **IsCustomerInitiatedMaintenanceAllowed** true olarak ayarlanırsa, AŞAĞıDAKI bir VM üzerinde bakım başlatır.

```powershell
Restart-AzVM -PerformMaintenance -name $vm.Name -ResourceGroupName $rg.ResourceGroupName 
```

## <a name="classic-deployments"></a>Klasik dağıtımlar

Klasik dağıtım modeli kullanılarak dağıtılmış eski VM 'Ler varsa, PowerShell kullanarak VM 'Leri sorgulayabilir ve bakım başlatabilirsiniz.

Bir sanal makinenin bakım durumunu almak için şunu yazın:

```
Get-AzureVM -ServiceName <Service name> -Name <VM name>
```

Klasik sanal makinenizde bakım başlatmak için şunu yazın:

```
Restart-AzureVM -InitiateMaintenance -ServiceName <service name> -Name <VM name>
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure CLI](maintenance-notifications-cli.md) veya [portalını](maintenance-notifications-portal.md)kullanarak planlı bakım de işleyebilirsiniz.
