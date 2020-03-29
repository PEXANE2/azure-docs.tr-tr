---
title: PowerShell'i kullanarak Azure VM'leri için bakım bildirimleri alın
description: Azure'da çalışan sanal makineler için bakım bildirimlerini görüntüleyin ve PowerShell'i kullanarak self servis bakıma başlayın.
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: b23c210d7c8a9f1d42e6e1b46e0f7f81bda857b2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77916091"
---
# <a name="handling-planned-maintenance-using-powershell"></a>PowerShell kullanarak planlı bakımı nişleme

**Bu makale, hem Linux hem de Windows çalıştıran sanal makineler için geçerlidir.**

VM'lerin [bakım](maintenance-notifications.md)için ne zaman zamanlandığını görmek için Azure PowerShell'i kullanabilirsiniz. Parametreyi kullandığınızda [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) cmdlet'ten `-status` planlı bakım bilgileri edinilebilir.
  
Bakım bilgileri yalnızca planlanmış bir bakım varsa döndürülür. VM'yi etkileyen bir bakım zamanlanmamışsa, cmdlet herhangi bir bakım bilgisini döndürmez. 


```powershell
Get-AzVM -ResourceGroupName myResourceGroup -Name myVM -Status
```

Aşağıdaki özellikler MaintenanceRedeployStatus altında döndürülür: 

| Değer | Açıklama   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | VM'de şu anda bakım başlatıp başlatamayacağınızı gösterir |
| Ön BakımPenceresiBaşlangıç Zamanı         | VM'nizde bakım başlatabileceğiniz bakım self servis penceresinin başlangıcı |
| Ön BakımPenceresiSon Saat           | VM'nizde bakım başlatabileceğiniz de bakım self servis penceresinin sonu |
| MaintenanceWindowStartTime            | Azure'un VM'nizde bakım başlattığı zamanlanan bakımın başlangıcı |
| MaintenanceWindowEndTime              | Azure'un VM'nizde bakım başlattığı bakım zamanlanan pencerenin sonu |
| LastOperationResultCode               | VM'de bakım başlatmak için yapılan son girişimin sonucu |



Ayrıca, [Get-AzVM](https://docs.microsoft.com/powershell/module/az.compute/get-azvm) kullanarak ve vm belirtmeden kaynak grubundaki tüm VM'ler için bakım durumunu da alabilirsiniz.
 
```powershell
Get-AzVM -ResourceGroupName myResourceGroup -Status
```

Aşağıdaki PowerShell örneği abonelik kimliğinizi alır ve bakım için zamanlanmış bir VM listesini döndürür.

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

### <a name="start-maintenance-on-your-vm-using-powershell"></a>PowerShell'i kullanarak VM'inizin bakımını başlatın

Önceki bölümdeki işlevdeki bilgileri kullanarak, **IsCustomerInitiatedMaintenanceAllowed** doğru ayarlanmışsa aşağıdaki vm'de bakım başlar.

```powershell
Restart-AzVM -PerformMaintenance -name $vm.Name -ResourceGroupName $rg.ResourceGroupName 
```

## <a name="classic-deployments"></a>Klasik dağıtımlar

[!INCLUDE [classic-vm-deprecation](../../includes/classic-vm-deprecation.md)]

Klasik dağıtım modelini kullanarak dağıtılan eski VM'leriniz hala varsa, PowerShell'i vm'leri sorgulamak ve bakımı başlatmak için kullanabilirsiniz.

VM'nin bakım durumunu almak için şunları yazın:

```
Get-AzureVM -ServiceName <Service name> -Name <VM name>
```

Klasik VM'nizde bakıma başlamak için şunları yazın:

```
Restart-AzureVM -InitiateMaintenance -ServiceName <service name> -Name <VM name>
```

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, [Azure CLI'yi](maintenance-notifications-cli.md) veya [portalı](maintenance-notifications-portal.md)kullanarak planlı bakımı da işleyebilirsiniz.
