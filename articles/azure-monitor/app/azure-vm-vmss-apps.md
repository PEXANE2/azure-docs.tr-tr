---
title: Azure VM'lerde performansı izleme - Azure Uygulama Öngörüleri
description: Azure VM ve Azure sanal makine ölçek kümeleri için uygulama performansı izleme. Yükleme ve yanıt süresini, bağımlılık bilgilerini grafik ve performans uyarıları ayarlayın.
ms.topic: conceptual
ms.date: 08/26/2019
ms.openlocfilehash: d75e14dccef565f0029d06583e74d5693726dd99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77661337"
---
# <a name="deploy-the-azure-monitor-application-insights-agent-on-azure-virtual-machines-and-azure-virtual-machine-scale-sets"></a>Azure Sanal makinelerde ve Azure sanal makine ölçek kümelerinde Azure Monitörü Uygulama Öngörüleri Aracısını dağıtma

[Azure sanal makinelerde](https://azure.microsoft.com/services/virtual-machines/) ve [Azure sanal makine ölçek setlerinde](https://docs.microsoft.com/azure/virtual-machine-scale-sets/) çalışan .NET tabanlı web uygulamalarınızda izlemeyi etkinleştirmek artık her zamankinden daha kolay. Kodunuzu değiştirmeden Uygulama Öngörüleri'ni kullanmanın tüm avantajlarından yararlanın.

Bu makale, Uygulama Öngörüleri Aracısı'nı kullanarak Uygulama Öngörüleri izlemeyi etkinleştirmenize olanak sağlar ve büyük ölçekli dağıtımlar için işlemi otomatikleştirmek için ön yönerge sağlar.

> [!IMPORTANT]
> .NET için Azure Application Insights Aracısı şu anda genel önizlemede.
> Bu önizleme sürümü hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önermiyoruz. Bazı özellikler desteklenmeyebilir ve bazıları kısıtlı özelliklere sahip olabilir.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

Azure sanal makineleri ve Azure sanal makine ölçek kümeleri için uygulamaları etkinleştirmenin iki yolu vardır:

* Uygulama Öngörüleri Aracısı ile **kodsuz**
    * Bu yöntem etkinleştirilmesi en kolay yöntemdir ve gelişmiş yapılandırma gerekmez. Genellikle "çalışma zamanı" izleme olarak adlandırılır.

    * Azure sanal makineleri ve Azure sanal makine ölçek kümeleri için en azından bu izleme düzeyini etkinleştirmenizi öneririz. Bundan sonra, özel senaryonuza bağlı olarak, manuel enstrümantasyon gerekli olup olmadığını değerlendirebilirsiniz.

    * Application Insights Agent, .NET SDK ile aynı bağımlılık sinyallerini otomatik olarak toplar. Daha fazla bilgi edinmek için [Bağımlılık otomatik koleksiyonuna](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies#net) bakın.
        > [!NOTE]
        > Şu anda yalnızca .Net IIS tarafından barındırılan uygulamalar desteklenmektedir. Azure sanal makinelerinde ve sanal makine ölçeği kümelerinde barındırılan Core, Java ve Node.js uygulamalarını ASP.NET bir SDK'yı kullanın.

* Kod **tabanlı** SDK üzerinden

    * Bu yaklaşım çok daha özelleştirilebilir, ancak [Uygulama Öngörüleri SDK NuGet paketleri bir bağımlılık eklemeyi](https://docs.microsoft.com/azure/azure-monitor/app/asp-net)gerektirir. Bu yöntem, aynı zamanda paketlerin en son sürümüne güncellemeleri kendiniz yönetmek zorunda anlamına gelir.

    * Aracı tabanlı izleme yle varsayılan olarak yakalanan olayları/bağımlılıkları izlemek için özel API çağrıları yapmanız gerekiyorsa, bu yöntemi kullanmanız gerekir. Daha fazla bilgi [edinmek için özel olaylar ve ölçümler makalesi için API'ye](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics) göz atın.

> [!NOTE]
> Hem aracı tabanlı izleme hem de manuel SDK tabanlı enstrümantasyon tespit edilirse, yalnızca manuel enstrümantasyon ayarları nasibini alacaktır. Bu, yinelenen verilerin gönderilmesini önlemek içindir. Bu konuda daha fazla bilgi edinmek için aşağıdaki [sorun giderme bölümüne](#troubleshooting) göz atın.

## <a name="manage-application-insights-agent-for-net-applications-on-azure-virtual-machines-using-powershell"></a>PowerShell'i kullanarak Azure sanal makinelerinde .NET uygulamaları için Uygulama Öngörüleri Aracısını Yönetin

> [!NOTE]
> Application Insights Aracısı'nı yüklemeden önce bir bağlantı dizeniz gerekir. [Yeni bir Uygulama Öngörüleri Kaynağı oluşturun](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) veya varolan bir uygulama öngörüleri kaynağından bağlantı dizesini kopyalayın.

> [!NOTE]
> Powershell'de yeni misiniz? [Başlangıç Kılavuzu'na](https://docs.microsoft.com/powershell/azure/get-started-azureps?view=azps-2.5.0)göz atın.

Azure sanal makineleri için uzantı olarak Uygulama Öngörüleri Aracısını yükleme veya güncelleştirme
```powershell
$publicCfgJsonString = '
{
  "redfieldConfiguration": {
    "instrumentationKeyMap": {
      "filters": [
        {
          "appFilter": ".*",
          "machineFilter": ".*",
          "virtualPathFilter": ".*",
          "instrumentationSettings" : {
            "connectionString": "InstrumentationKey=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
          }
        }
      ]
    }
  }
}
';
$privateCfgJsonString = '{}';

Set-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Location "<myVmLocation>" -Name "ApplicationMonitoring" -Publisher "Microsoft.Azure.Diagnostics" -Type "ApplicationMonitoringWindows" -Version "2.8" -SettingString $publicCfgJsonString -ProtectedSettingString $privateCfgJsonString
```

> [!NOTE]
> Powershell döngüsü kullanarak, Uygulama Öngörüleri Aracısını birden çok Sanal Makine ölçeğinde uzantı olarak yükleyebilir veya güncelleştirebilirsiniz.

Azure sanal makineden Uygulama Öngörüleri Aracı uzantısını kaldırma
```powershell
Remove-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name "ApplicationMonitoring"
```

Azure sanal makine için Uygulama Öngörülerini Sorgula Aracı uzantısı durumu
```powershell
Get-AzVMExtension -ResourceGroupName "<myVmResourceGroup>" -VMName "<myVmName>" -Name ApplicationMonitoring -Status
```

Azure sanal makinesi için yüklü uzantıların listesini alma
```powershell
Get-AzResource -ResourceId "/subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions"

# Name              : ApplicationMonitoring
# ResourceGroupName : <myVmResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachines/extensions
# Location          : southcentralus
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myVmResourceGroup>/providers/Microsoft.Compute/virtualMachines/<myVmName>/extensions/ApplicationMonitoring
```
Yüklü uzantıları Portal'daki Azure [sanal makine bıçağında](https://docs.microsoft.com/azure/virtual-machines/extensions/overview) da görüntüleyebilirsiniz.

> [!NOTE]
> Uygulama Öngörüleri Aracı Uzantısı'nı dağıtmak için kullandığınız bağlantı dizesiyle ilişkili Application Insights Kaynağı'ndaki Canlı Ölçümler Akışı'nı tıklatarak yüklemeyi doğrulayın. Birden çok Sanal Makineden veri gönderiyorsanız, Sunucu Adı altında hedef Azure sanal makinelerini seçin. Verilerin akmaya başlaması bir dakika kadar sürebilir.

## <a name="manage-application-insights-agent-for-net-applications-on-azure-virtual-machine-scale-sets-using-powershell"></a>Powershell kullanarak Azure sanal makine ölçeği kümelerinde .NET uygulamaları için Uygulama Öngörüleri Aracısını Yönetin

Azure sanal makine ölçeği kümesi için uzantı olarak Uygulama Öngörüleri Aracısını yükleme veya güncelleştirme
```powershell
$publicCfgHashtable =
@{
  "redfieldConfiguration"= @{
    "instrumentationKeyMap"= @{
      "filters"= @(
        @{
          "appFilter"= ".*";
          "machineFilter"= ".*";
          "virtualPathFilter": ".*",
          "instrumentationSettings" : {
            "connectionString": "InstrumentationKey=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" # Application Insights connection string, create new Application Insights resource if you don't have one. https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/microsoft.insights%2Fcomponents
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

Uygulama izleme uzantısını Azure sanal makine ölçek kümelerinden kaldırma
```powershell
$vmss = Get-AzVmss -ResourceGroupName "<myResourceGroup>" -VMScaleSetName "<myVmssName>"

Remove-AzVmssExtension -VirtualMachineScaleSet $vmss -Name "ApplicationMonitoring"

Update-AzVmss -ResourceGroupName $vmss.ResourceGroupName -Name $vmss.Name -VirtualMachineScaleSet $vmss

# Note: depending on your update policy, you might need to run Update-AzVmssInstance for each instance
```

Azure sanal makine ölçek kümeleri için uygulama izleme uzantısı durumunu sorgula
```powershell
# Not supported by extensions framework
```

Azure sanal makine ölçek kümeleri için yüklü uzantıların listesini alma
```powershell
Get-AzResource -ResourceId /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions

# Name              : ApplicationMonitoringWindows
# ResourceGroupName : <myResourceGroup>
# ResourceType      : Microsoft.Compute/virtualMachineScaleSets/extensions
# Location          :
# ResourceId        : /subscriptions/<mySubscriptionId>/resourceGroups/<myResourceGroup>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVmssName>/extensions/ApplicationMonitoringWindows
```

## <a name="troubleshooting"></a>Sorun giderme

Azure sanal makinelerde ve sanal makine ölçek kümelerinde çalışan .NET uygulamaları için Application Insights Monitoring Agent Extension için sorun giderme ipuçları nı bulun.

> [!NOTE]
> .NET Core, Java ve Node.js uygulamaları yalnızca Azure sanal makinelerinde ve Azure sanal makine ölçek setlerinde manuel SDK tabanlı enstrümantasyon aracılığıyla desteklenir ve bu nedenle aşağıdaki adımlar bu senaryolar için geçerli değildir.

Uzantı yürütme çıktısı aşağıdaki dizinlerde bulunan dosyalara kaydedilir:
```Windows
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.ApplicationMonitoringWindows\<version>\
```

## <a name="next-steps"></a>Sonraki adımlar
* [Bir uygulamayı Azure sanal makine ölçeği kümesine](../../virtual-machine-scale-sets/virtual-machine-scale-sets-deploy-app.md)nasıl dağıtılaymayı öğrenin.
* Bitiş noktanız aşağıdaysa uyarılacak [Kullanılabilirlik web testlerini ayarlayın.](monitor-web-app-availability.md)
