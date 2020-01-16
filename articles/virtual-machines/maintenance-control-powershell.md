---
title: PowerShell kullanarak Azure sanal makineleri için bakım denetimi
description: Bakım denetimini ve PowerShell 'i kullanarak Azure VM 'lerinize bakım uygulandığını nasıl denetleyeceğinizi öğrenin.
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 12/06/2019
ms.author: cynthn
ms.openlocfilehash: 7ca98723511cc7297b462747d4e1e12ca9bd38c2
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75979011"
---
# <a name="preview-control-updates-with-maintenance-control-and-azure-powershell"></a>Önizleme: bakım denetimi ve Azure PowerShell güncelleştirmelerini denetleme

Bakım denetimini kullanarak yeniden başlatma gerektirmeyen platform güncelleştirmelerini yönetin. Azure, güvenilirliği, performansı, güvenliği geliştirmek veya yeni özellikleri başlatmak için altyapısını sıklıkla güncelleştirir. Çoğu güncelleştirme kullanıcılara saydamdır. Oyun, medya akışı ve finans işlemleri gibi bazı hassas iş yükleri, bir VM 'nin bakım için donuyor veya bağlantısı kesilmesinin birkaç saniyesini de kabul edemez. Bakım denetimi, platform güncelleştirmelerini bekleme ve bunları 35 günlük bir sıralı pencere içinde uygulama seçeneği sunar. 

Bakım denetimi, yalıtılmış sanal makinelerinize güncelleştirmelerin ne zaman uygulanacağına karar vermenizi sağlar.

Bakım denetimi ile şunları yapabilirsiniz:
- Toplu güncelleştirmeler tek bir güncelleştirme paketine sahiptir.
- Güncelleştirmelerin uygulanması için 35 güne kadar bekleyin. 
- Azure Işlevleri 'ni kullanarak bakım pencereniz için platform güncelleştirmelerini otomatikleştirin.
- Bakım yapılandırması abonelikler ve kaynak grupları arasında çalışır. 

> [!IMPORTANT]
> Bakım denetimi şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
> 

## <a name="limitations"></a>Sınırlamalar

- VM 'Lerin [ayrılmış bir konakta](./linux/dedicated-hosts.md)olması veya [yalıtılmış bir VM boyutu](./linux/isolation.md)kullanılarak oluşturulması gerekir.
- 35 gün sonra, bir güncelleştirme otomatik olarak uygulanır.
- Kullanıcının **kaynak sahibi** erişimi olmalıdır.


## <a name="enable-the-powershell-module"></a>PowerShell modülünü etkinleştir

`PowerShellGet` güncel olduğundan emin olun.

```azurepowershell-interactive
Install-Module -Name PowerShellGet -Repository PSGallery -Force
```

Az. Maintenance PowerShell cmdlet 'leri önizlemededir, bu nedenle modülü Cloud Shell veya yerel PowerShell yüklemenizde `AllowPrerelease` parametresiyle yüklemeniz gerekir.   

```azurepowershell-interactive
Install-Module -Name Az.Maintenance -AllowPrerelease
```

Yerel olarak yüklüyorsanız, PowerShell komut dosyanızı yönetici olarak açtığınızdan emin olun.

Ayrıca, *Güvenilmeyen bir depodan*yüklemek istediğinizi onaylamanız istenebilir. Modülü yüklemek için `Y` yazın veya **Tümüne Evet** ' i seçin.



## <a name="create-a-maintenance-configuration"></a>Bakım yapılandırması oluşturma

Yapılandırmanız için kapsayıcı olarak bir kaynak grubu oluşturun. Bu örnekte, *eastus*içinde *myMaintenanceRG* adlı bir kaynak grubu oluşturulur. Kullanmak istediğiniz bir kaynak grubunuz zaten varsa, bu bölümü atlayabilir ve örneklerin geri kalanında kaynak grubu adını sahipsiz olarak değiştirebilirsiniz.

```azurepowershell-interactive
New-AzResourceGroup `
   -Location eastus `
   -Name myMaintenanceRG
```

Bir bakım yapılandırması oluşturmak için [New-AzMaintenanceConfiguration](https://docs.microsoft.com/powershell/module/az.maintenance/new-azmaintenanceconfiguration) kullanın. Bu örnek, konakta *MyConfig* adlı bir bakım yapılandırması oluşturur. 

```azurepowershell-interactive
$config = New-AzMaintenanceConfiguration `
   -ResourceGroup myMaintenanceRG `
   -Name myConfig `
   -MaintenanceScope host `
   -Location  eastus
```

`-MaintenanceScope host` kullanmak, bakım yapılandırmasının konaktaki güncelleştirmeleri denetlemek için kullanılmasını sağlar.

Aynı ada sahip bir yapılandırma oluşturmaya çalışırsanız, ancak farklı bir konumda hata alırsınız. Yapılandırma adları, aboneliğiniz için benzersiz olmalıdır.

[Get-AzMaintenanceConfiguration](https://docs.microsoft.com/powershell/module/az.maintenance/get-azmaintenanceconfiguration)kullanarak kullanılabilir bakım yapılandırması için sorgulama yapabilirsiniz.

```azurepowershell-interactive
Get-AzMaintenanceConfiguration | Format-Table -Property Name,Id
```

## <a name="assign-the-configuration"></a>Yapılandırmayı ata

Yapılandırmayı yalıtılmış sanal makinenize veya Azure adanmış ana bilgisayarınıza atamak için [New-Azconfigurationatama](https://docs.microsoft.com/powershell/module/az.maintenance/new-azconfigurationassignment) ' yı kullanın.

### <a name="isolated-vm"></a>Yalıtılmış VM

Yapılandırma KIMLIĞINI kullanarak yapılandırmayı bir VM 'ye uygulayın. `-ResourceType VirtualMachines` belirtin ve `-ResourceName`sanal makinenin adını ve `-ResourceGroupName`için VM 'nin kaynak grubunu sağlayın. 

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

Bir yapılandırmayı adanmış bir konağa uygulamak için `-ResourceType hosts`, konak grubunun adı ile `-ResourceParentName` ve `-ResourceParentType hostGroups`de eklemeniz gerekir. 


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

## <a name="check-for-pending-updates"></a>Bekleyen güncelleştirmeleri denetle

Bekleyen güncelleştirmeler olup olmadığını görmek için [Get-AzMaintenanceUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/get-azmaintenanceupdate) kullanın. Oturum açmış olduğunuz sunucudan farklıysa VM 'nin Azure aboneliğini belirtmek için `-subscription` kullanın.

Güncelleştirme yoksa, komut şu hata iletisini döndürür: `Resource not found...StatusCode: 404`.

### <a name="isolated-vm"></a>Yalıtılmış VM

Yalıtılmış bir VM için bekleyen güncelleştirmeleri denetleyin. Bu örnekte, çıkış okunabilirlik için bir tablo olarak biçimlendirilir.

```azurepowershell-interactive
Get-AzMaintenanceUpdate `
  -ResourceGroupName myResourceGroup `
  -ResourceName myVM `
  -ResourceType VirtualMachines `
  -ProviderName Microsoft.Compute | Format-Table
```

### <a name="dedicated-host"></a>Ayrılmış konak

Adanmış bir ana bilgisayar için bekleyen güncelleştirmeleri denetlemek için. Bu örnekte, çıkış okunabilirlik için bir tablo olarak biçimlendirilir. Kaynakların değerlerini kendi değerlerinizle değiştirin.

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

Bekleyen güncelleştirmeleri uygulamak için [New-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/new-azapplyupdate) kullanın.

### <a name="isolated-vm"></a>Yalıtılmış VM

Yalıtılmış bir VM 'ye güncelleştirme uygulamak için bir istek oluşturun.

```azurepowershell-interactive
New-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute
```

### <a name="dedicated-host"></a>Ayrılmış konak

Güncelleştirmeleri adanmış bir konağa uygulayın.

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
Bir güncelleştirmenin durumunu denetlemek için [Get-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/get-azapplyupdate) kullanın. Aşağıda gösterilen komutlar, `-ApplyUpdateName` parametresi için `default` kullanarak en son güncelleştirmenin durumunu gösterir. Belirli bir güncelleştirmenin durumunu almak için, güncelleştirmenin adını ( [New-AzApplyUpdate](https://docs.microsoft.com/powershell/module/az.maintenance/new-azapplyupdate) komutu tarafından döndürülen) kullanabilirsiniz.

Gösterilecek güncelleştirme yoksa, komut şu hata iletisini döndürür: `Resource not found...StatusCode: 404`.

### <a name="isolated-vm"></a>Yalıtılmış VM

Belirli bir sanal makinede güncelleştirmeleri denetleyin.

```azurepowershell-interactive
Get-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myVM `
   -ResourceType VirtualMachines `
   -ProviderName Microsoft.Compute `
   -ApplyUpdateName default
```

### <a name="dedicated-host"></a>Ayrılmış konak

Adanmış bir konaktaki güncelleştirmeleri denetleyin.

```azurepowershell-interactive
Get-AzApplyUpdate `
   -ResourceGroupName myResourceGroup `
   -ResourceName myHost `
   -ResourceType hosts `
   -ResourceParentName myHostGroup `
   -ResourceParentType hostGroups `
   -ProviderName Microsoft.Compute `
   -ApplyUpdateName default
```

## <a name="remove-a-maintenance-configuration"></a>Bakım yapılandırmasını kaldırma

Bir bakım yapılandırmasını silmek için [Remove-AzMaintenanceConfiguration](https://docs.microsoft.com/powershell/module/az.maintenance/remove-azmaintenanceconfiguration) kullanın.

```azurecli-interactive
Remove-AzMaintenanceConfiguration `
   -ResourceGroupName myResourceGroup `
   -Name $config.Name
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz. [bakım ve güncelleştirmeler](maintenance-and-updates.md).
