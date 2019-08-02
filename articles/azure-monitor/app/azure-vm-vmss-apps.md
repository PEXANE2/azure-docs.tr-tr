---
title: Azure VM ve Azure sanal makine ölçek kümelerinde barındırılan uygulama performansını izleme | Microsoft Docs
description: Azure VM ve Azure sanal makine ölçek kümeleri için uygulama performansı izleme. Grafik yükleme ve yanıt süresi, bağımlılık bilgileri ve performans üzerinde Uyarılar ayarlama.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 06/27/2019
ms.author: mbullwin
ms.openlocfilehash: f2c6b98fd0be2061e9d8cab5c063cafadf71476a
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68597459"
---
# <a name="monitor-application-performance-hosted-on-azure-vm-and-azure-virtual-machine-scale-sets"></a>Azure VM ve Azure sanal makine ölçek kümelerinde barındırılan uygulama performansını izleme

[Azure sanal makinelerinde](https://azure.microsoft.com/services/virtual-machines/) ve [Azure sanal makine ölçek kümelerinde](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) çalışan .NET tabanlı Web uygulamalarınızda izlemenin etkinleştirilmesi artık hiç olmadığı kadar kolay. Kodunuzda değişiklik yapmadan Application Insights kullanmanın avantajlarından yararlanın.

Bu makale, ApplicationMonitoringWindows uzantısını kullanarak Application Insights izlemeyi etkinleştirme konusunda size kılavuzluk eder ve büyük ölçekli dağıtımlar için işlemi otomatikleştirmek üzere ön kılavuz sağlar.

> [!IMPORTANT]
> Azure ApplicationMonitoringWindows uzantısı Şu anda genel önizleme aşamasındadır.
> Bu önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için önermiyoruz. Bazı özellikler desteklenmeyebilir ve bazıları kısıtlı özelliklere sahip olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="enable-application-insights"></a>Application Insights'ı Etkinleştir

Azure VM ve Azure sanal makine ölçek kümesi barındırılan uygulamalar için uygulama izlemeyi etkinleştirmenin iki yolu vardır:

* **Aracı tabanlı uygulama izleme** (ApplicationMonitoringWindows uzantısı).
    * Bu yöntem etkinleştirilmesi en kolayıdır ve gelişmiş yapılandırma gerekmez. Genellikle "çalışma zamanı" izleme olarak adlandırılır. Azure VM 'Leri ve Azure sanal makine ölçek kümeleri için, bu izleme düzeyini en az etkinleştirmenizi öneririz. Bu tarihten sonra, özel senaryonuza bağlı olarak, el ile izleme gerekip gerekmediğini değerlendirebilirsiniz.
    * Şu anda yalnızca .NET IIS tarafından barındırılan uygulamalar desteklenir.

* Application Insights SDK 'Yı yükleyerek **uygulamayı kodla el ile işaretleme** .

    * Bu yaklaşım çok daha özelleştirilebilir, ancak [APPLICATION INSIGHTS SDK NuGet paketlerine bağımlılık eklemeyi](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)gerektirir. Bu yöntem, ayrıca paketlerin en son sürümüne yönelik güncelleştirmeleri yönetmeniz anlamına gelir.

    * Aracı tabanlı izleme ile varsayılan olarak yakalanmayan olayları/bağımlılıkları izlemek için özel API çağrıları yapmanız gerekiyorsa, bu yöntemi kullanmanız gerekir. Daha fazla bilgi edinmek için [özel olaylar ve ölçümler makalesine yönelik API](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) 'ye göz atın.

> [!NOTE]
> Hem aracı tabanlı izleme hem de el ile SDK tabanlı izleme algılanırsa yalnızca el ile izleme ayarları kabul edilir. Bu, yinelenen verilerin gönderilmesini önlemektir. Bu konuda daha fazla bilgi edinmek için aşağıdaki [sorun giderme bölümüne](#troubleshooting) bakın.

## <a name="manage-agent-based-monitoring-for-net-applications-on-vm-using-powershell"></a>PowerShell kullanarak VM 'de .NET uygulamaları için aracı tabanlı izlemeyi yönetme

VM için uygulama izleme uzantısını yükler veya güncelleştirir
```powershell
$publicCfgJsonString = '
{
  "RedfieldConfiguration": {
    "InstrumentationKeyMap": {
      "Filters": [
        {
          "AppFilter": ".*",
          "MachineFilter": ".*",
          "InstrumentationSettings" : {
            "InstrumentationKey": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          }
        }
      ]
    }
  }
}
';
$privateCfgJsonString = '{}';

Set-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Location "South Central US" -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -Version "2.8" -SettingString $publicCfgJsonString -ProtectedSettingString $privateCfgJsonString
```

Uygulama izleme uzantısını VM 'den kaldır
```powershell
Remove-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name "ApplicationMonitoring"
```

VM için sorgu uygulaması izleme uzantısı durumu
```powershell
Get-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name ApplicationMonitoring -Status
```

VM için yüklü uzantıların listesini al
```powershell
Get-AzResource -ResourceId "/subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions"

# Name              : ApplicationMonitoring
# ResourceGroupName : <myVmResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachines/extensions
# Location          : southcentralus
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions/ApplicationMonitoring
```

## <a name="manage-agent-based-monitoring-for-net-applications-on-azure-virtual-machine-scale-set-using-powershell"></a>PowerShell kullanarak Azure sanal makine ölçek kümesi 'nde .NET uygulamaları için aracı tabanlı izlemeyi yönetme

Azure sanal makine ölçek kümesi için uygulama izleme uzantısını yükler veya güncelleştirir
```powershell
$publicCfgHashtable =
@{
  "RedfieldConfiguration"= @{
    "InstrumentationKeyMap"= @{
      "Filters"= @(
        @{
          "AppFilter"= ".*";
          "MachineFilter"= ".*";
          "InstrumentationSettings"= @{
            "InstrumentationKey"= "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"; # Application Insights Instrumentation Key, create new Application Insights resource if you don't have one. https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents
          }
        }
      )
    }
  }
};
$privateCfgHashtable = @{};

$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Add-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -TypeHandlerVersion "2.8" -Setting $publicCfgHashtable -ProtectedSetting $privateCfgHashtable

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Azure sanal makine ölçek kümesinden uygulama izleme uzantısını kaldır
```powershell
$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Remove-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring"

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Azure sanal makine ölçek kümesi için sorgu uygulaması izleme uzantısı durumu
```powershell
# Not supported by extensions framework
```

Azure sanal makine ölçek kümesi için yüklü uzantıların listesini al
```powershell
Get-AzResource -ResourceId /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions

# Name              : ApplicationMonitoringWindows
# ResourceGroupName : <myResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachineScaleSets/extensions
# Location          :
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions/ApplicationMonitoringWindows
```

## <a name="troubleshooting"></a>Sorun giderme

Azure VM ve Azure sanal makine ölçek kümeleri üzerinde çalışan .NET uygulamalarına yönelik uzantı tabanlı izleme için adım adım sorun giderme kılavuzumuz aşağıda verilmiştir.

> [!NOTE]
> .NET Core, Java ve Node. js uygulamaları yalnızca Azure VM 'de ve Azure sanal makine ölçek kümelerinde, el ile SDK tabanlı araçlar aracılığıyla desteklenir ve bu nedenle aşağıdaki adımlar bu senaryolara uygulanmaz.

Uzantı yürütme çıktısı, aşağıdaki dizinlerde bulunan dosyalara kaydedilir:
```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.ApplicationMonitoringWindows\<version>\
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir [Azure sanal makine ölçek kümesine uygulama dağıtmayı](../../virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app.md)öğrenin.
* Uç noktanız kapalıysa, [kullanılabilirlik Web testlerini](monitor-web-app-availability.md) uyarılmak için ayarlayın.
