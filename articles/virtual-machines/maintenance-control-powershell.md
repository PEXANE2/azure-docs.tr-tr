---
title: PowerShell kullanan Azure sanal makineleri için bakım kontrolü
description: Bakım Denetimi ve PowerShell'i kullanarak Azure VM'lerinize ne zaman bakım uygulandığını nasıl kontrol edebilirsiniz.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/31/2020
ms.author: cynthn
ms.openlocfilehash: dc47afe9cb6eca1b10f8caca7b85087023c5eadf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060127"
---
# <a name="preview-control-updates-with-maintenance-control-and-azure-powershell"></a>Önizleme: Bakım Denetimi ve Azure PowerShell ile güncelleştirmeleri denetleme

Bakım denetimini kullanarak yeniden başlatma gerektirmeyen platform güncelleştirmelerini yönetin. Azure, güvenilirliği, performansı, güvenliği artırmak veya yeni özellikler başlatmak için altyapısını sık sık güncelleştirir. Güncelleştirmelerin çoğu kullanıcılar için saydamdır. Oyun, medya akışı ve finansal işlemler gibi bazı hassas iş yükleri, birkaç saniyelik VM donma veya bakım için bağlantının kesilmesine bile tahammül edemez. Bakım denetimi, platform güncelleştirmelerini bekleme ve bunları 35 günlük bir haddeleme süresi içinde uygulama seçeneği sunar. 

Bakım denetimi, güncelleştirmeleri yalıtılmış VM'lerinize ne zaman uygulayacağınıza karar vermenizi sağlar.

Bakım kontrolü ile şunları yapabilirsiniz:
- Toplu güncelleştirmeleri tek bir güncelleştirme paketinde tamamlar.
- Güncelleştirmeleri uygulamak için 35 güne kadar bekleyin. 
- Azure İşlevlerini kullanarak bakım pencereniz için platform güncelleştirmelerini otomatikleştirin.
- Bakım yapılandırmaları abonelikler ve kaynak grupları arasında çalışır. 

> [!IMPORTANT]
> Bakım Denetimi şu anda genel önizlemede.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
> 

## <a name="limitations"></a>Sınırlamalar

- VM'ler özel bir [ana bilgisayarda](./linux/dedicated-hosts.md)olmalı veya [yalıtılmış VM boyutu](./linux/isolation.md)kullanılarak oluşturulmalıdır.
- 35 gün sonra otomatik olarak bir güncelleştirme uygulanır.
- Kullanıcı **kaynak katkıda bulunan** erişimi ne olmalıdır.


## <a name="enable-the-powershell-module"></a>PowerShell modüllerini etkinleştirme

Güncel `PowerShellGet` olduğundan emin olun.

```azurepowershell-interactive
Install-Module -Name PowerShellGet -Repository PSGallery -Force
```

Az.Maintenance PowerShell cmdlets önizlemede olduğundan, modülü Cloud Shell'de veya yerel PowerShell kurulumunuzda `AllowPrerelease` parametreyle yüklemeniz gerekir.   

```azurepowershell-interactive
Install-Module -Name Az.Maintenance -AllowPrerelease
```

Yerel olarak yüklüyorsanız, PowerShell komut isteminizi yönetici olarak açtığınızdan emin olun.

Ayrıca, güvenilmeyen bir depodan yüklemek *istediğinizi*onaylamanız da istenebilir. Modülü `Y` yüklemek **için Tümüne Evet** yazın veya seçin.



## <a name="create-a-maintenance-configuration"></a>Bakım yapılandırması oluşturma

Yapılandırmanız için kapsayıcı olarak bir kaynak grubu oluşturun. Bu örnekte *eastus'ta* *myMaintenanceRG* adlı bir kaynak grubu oluşturulur. Kullanmak istediğiniz bir kaynak grubunuz zaten varsa, bu bölümü atlayabilir ve örneklerin geri kalanında kaynak grubu adını sizinkiyle değiştirebilirsiniz.

```azurepowershell-interactive
New-AzResourceGroup `
   -Location eastus `
   -Name myMaintenanceRG
```

Bakım yapılandırması oluşturmak için [Yeni-AzMaintenanceConfiguration'ı](https://docs.microsoft.com/powershell/module/az.maintenance/new-azmaintenanceconfiguration) kullanın. Bu örnek, ana bilgisayara kapsamı *myConfig* adlı bir bakım yapılandırması oluşturur. 

```azurepowershell-interactive
$config = New-AzMaintenanceConfiguration `
   -ResourceGroup myMaintenanceRG `
   -Name myConfig `
   -MaintenanceScope host `
   -Location  eastus
```

Bakım `-MaintenanceScope host` yapılandırmasının ana bilgisayara güncelleştirmeleri denetlemek için kullanılmasını sağlar.

Aynı ada sahip, ancak farklı bir konumda bir yapılandırma oluşturmaya çalışırsanız, bir hata alırsınız. Yapılandırma adları aboneliğinize özgü olmalıdır.

[Get-AzMaintenanceConfiguration'ı](https://docs.microsoft.com/powershell/module/az.maintenance/get-azmaintenanceconfiguration)kullanarak kullanılabilir bakım yapılandırmalarını sorgulayabilirsiniz.

```azurepowershell-interactive
Get-AzMaintenanceConfiguration | Format-Table -Property Name,Id
```

## <a name="assign-the-configuration"></a>Yapılandırmayı atama

Yapılandırmayı yalıtılmış VM'nize veya Azure Özel Ana Bilgisayarınıza atamak için [Yeni YapılandırmaAtama'yı](https://docs.microsoft.com/powershell/module/az.maintenance/new-azconfigurationassignment) kullanın.

### <a name="isolated-vm"></a>Yalıtılmış VM

Yapılandırmanın kimliğini kullanarak yapılandırmayı bir VM'ye uygulayın. VM'nin adını ve VM'nin `-ResourceType VirtualMachines` `-ResourceName`kaynak grubunu belirtin `-ResourceGroupName`ve tedarik edin. 

```azurepowershell-interactive
New-AzConfigurationAssignment `
   -ResourceGroupName myResourceGroup `
   -Location eastus `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute `
   -ConfigurationAssignmentName $config.Name `
   -MaintenanceConfigurationId $config.Id
```

### <a name="dedicated-host"></a>Ayrılmış konak

Özel bir ana bilgisayara yapılandırma uygulamak için, `-ResourceParentName` ana bilgisayar grubunun adını `-ResourceParentType hostGroups`ve `-ResourceType hosts`. 


```azurepowershell-interactive
New-AzConfigurationAssignment `
   -ResourceGroupName myResourceGroup `
   -Location eastus `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute `
   -ConfigurationAssignmentName $config.Name `
   -MaintenanceConfigurationId $config.Id
```

## <a name="check-for-pending-updates"></a>Bekleyen güncelleştirmeleri denetleme

Bekleyen güncelleştirmeler olup olmadığını görmek için [Get-AzMaintenanceUpdate'i](https://docs.microsoft.com/powershell/module/az.maintenance/get-azmaintenanceupdate) kullanın. VM'nin Azure aboneliğini, oturum açtığınız abonelikten farklıysa belirtmek için kullanın. `-subscription`

Gösterilecek güncelleştirme yoksa, bu komut hiçbir şey döndürmez. Aksi takdirde, bir PSApplyUpdate nesnedöndürecek:

```json
{
   "maintenanceScope": "Host",
   "impactType": "Freeze",
   "status": "Pending",
   "impactDurationInSec": 9,
   "notBefore": "2020-02-21T16:47:44.8728029Z",
   "properties": {
      "resourceId": "/subscriptions/39c6cced-4d6c-4dd5-af86-57499cd3f846/resourcegroups/Ignite2019/providers/Microsoft.Compute/virtualMachines/MCDemo3"
} 
```

### <a name="isolated-vm"></a>Yalıtılmış VM

Yalıtılmış bir VM için bekleyen güncelleştirmeleri denetleyin. Bu örnekte, çıktı okunabilirlik için bir tablo olarak biçimlendirilir.

```azurepowershell-interactive
Get-AzMaintenanceUpdate `
  -ResourceGroupName myResourceGroup `
  -ResourceName myVM `
  -ResourceType VirtualMachines `
  -ProviderName Microsoft.Compute | Format-Table
```


### <a name="dedicated-host"></a>Ayrılmış konak

Özel bir ana bilgisayar için bekleyen güncelleştirmeleri denetlemek için. Bu örnekte, çıktı okunabilirlik için bir tablo olarak biçimlendirilir. Kaynakların değerlerini kendi değerlerinizle değiştirin.

```azurepowershell-interactive
Get-AzMaintenanceUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute | Format-Table
```


## <a name="apply-updates"></a>Güncelleştirme uygulama

Bekleyen güncelleştirmeleri uygulamak için [Yeni-AzApplyUpdate'i](https://docs.microsoft.com/powershell/module/az.maintenance/new-azapplyupdate) kullanın.

### <a name="isolated-vm"></a>Yalıtılmış VM

Güncelleştirmeleri yalıtılmış bir VM'ye uygulamak için bir istek oluşturun.

```azurepowershell-interactive
New-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute
```

Başarı da, bu komut `PSApplyUpdate` bir nesneyi döndürecek. Güncelleştirme durumunu denetlemek için komuttaki `Get-AzApplyUpdate` Ad özniteliğini kullanabilirsiniz. Bkz. [Güncelleştirme durumunu denetle.](#check-update-status)

### <a name="dedicated-host"></a>Ayrılmış konak

Güncelleştirmeleri özel bir ana bilgisayara uygulayın.

```azurepowershell-interactive
New-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute
```

## <a name="check-update-status"></a>Güncelleştirme durumunu denetle
Bir güncelleştirmenin durumunu kontrol etmek için [Get-AzApplyUpdate'i](https://docs.microsoft.com/powershell/module/az.maintenance/get-azapplyupdate) kullanın. Aşağıda gösterilen komutlar `default` `-ApplyUpdateName` parametre için kullanarak en son güncelleştirmenin durumunu gösterir. Belirli bir güncelleştirmenin durumunu almak için güncelleştirmenin adını [(Yeni-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/new-azapplyupdate) komutu yla döndürülen) değiştirebilirsiniz.

```text
Status         : Completed
ResourceId     : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso
LastUpdateTime : 1/1/2020 12:00:00 AM
Id             : /subscriptions/12ae7457-4a34-465c-94c1-17c058c2bd25/resourcegroups/TestShantS/providers/Microsoft.Comp
ute/virtualMachines/DXT-test-04-iso/providers/Microsoft.Maintenance/applyUpdates/default
Name           : default
Type           : Microsoft.Maintenance/applyUpdates
```
LastUpdateTime, güncelleştirmenin tamamlandığını, sizin veya kendi bakım penceresinin kullanılmaması durumunda platform tarafından başlatıldığı zaman olacaktır. Bakım denetimi yoluyla hiç güncelleştirme uygulanmamışsa, varsayılan değeri gösterir.

### <a name="isolated-vm"></a>Yalıtılmış VM

Belirli bir sanal makinenin güncelleştirmelerini denetleyin.

```azurepowershell-interactive
Get-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute `
   -ApplyUpdateName default
```

### <a name="dedicated-host"></a>Ayrılmış konak

Özel bir ana bilgisayara güncelleştirmeleri denetleyin.

```azurepowershell-interactive
Get-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute `
   -ApplyUpdateName myUpdateName
```

## <a name="remove-a-maintenance-configuration"></a>Bakım yapılandırması kaldırma

Bakım yapılandırmasını silmek için [Kaldır-AzMaintenanceConfiguration'ı](https://docs.microsoft.com/powershell/module/az.maintenance/remove-azmaintenanceconfiguration) kullanın.

```azurepowershell-interactive
Remove-AzMaintenanceConfiguration `
   -ResourceGroupName myResourceGroup `
   -Name $config.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [Bkz. Bakım ve güncelleştirmeler.](maintenance-and-updates.md)
