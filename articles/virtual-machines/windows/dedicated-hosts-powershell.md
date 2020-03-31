---
title: Azure PowerShell'i kullanarak Azure'a özel ana bilgisayarları dağıtma
description: Azure PowerShell'i kullanarak özel ana bilgisayarlara VM dağıtın.
author: cynthn
ms.service: virtual-machines-windows
ms.topic: article
ms.workload: infrastructure
ms.date: 08/01/2019
ms.author: cynthn
ms.openlocfilehash: a228a83d711c84d2aa994e6de7d90af48cca7f28
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79530946"
---
# <a name="deploy-vms-to-dedicated-hosts-using-the-azure-powershell"></a>Azure PowerShell'i kullanarak özel ana bilgisayarlara VM dağıtma

Bu makalede, sanal makinelerinizi (VM) barındırmak için [Azure'a adanmış](dedicated-hosts.md) bir ana bilgisayar oluşturma konusunda size rehberlik eder. 

Azure PowerShell sürüm 2.8.0 veya sonraki sürümlerini yüklediğinizden ve bir Azure `Connect-AzAccount`hesabında oturum açarak oturum açabildiğinizden emin olun. 

## <a name="limitations"></a>Sınırlamalar

- Sanal makine ölçek kümeleri şu anda özel ana bilgisayarlarda desteklenmez.
- Özel ana bilgisayarlar için kullanılabilen boyutlar ve donanım türleri bölgeye göre değişir. Daha fazla bilgi edinmek için ana bilgisayar [fiyatlandırma sayfasına](https://aka.ms/ADHPricing) bakın.

## <a name="create-a-host-group"></a>Ana bilgisayar grubu oluşturma

**Ana bilgisayar grubu,** özel ana bilgisayarkoleksiyonunu temsil eden bir kaynaktır. Bir bölgede ve kullanılabilirlik bölgesinde bir ana bilgisayar grubu oluşturur ve ona ana bilgisayarlar eklersiniz. Yüksek kullanılabilirlik için planlama yaparken, ek seçenekler vardır. İlgili ana bilgisayarlarınızla aşağıdaki seçeneklerden birini veya her ikisini de kullanabilirsiniz: 
- Birden çok kullanılabilirlik bölgesine yayılma. Bu durumda, kullanmak istediğiniz bölgelerin her birinde bir ana bilgisayar grubuna sahip olması gerekir.
- Fiziksel raflara eşlenen birden çok hata etki alanına yayılma alanı. 
 
Her iki durumda da, ana bilgisayar grubunuz için hata etki alanı sayısını sağlamanız gerekir. Grubunuzdaki hata etki alanlarını yayılımını istemiyorsanız, 1 hata etki alanı sayısı kullanın. 

Ayrıca, hem kullanılabilirlik bölgelerini hem de hata etki alanlarını kullanmaya karar verebilirsiniz. Bu örnek, bölge 1'de 2 hata etki alanı içeren bir ana bilgisayar grubu oluşturur. 


```azurepowershell-interactive
$rgName = "myDHResourceGroup"
$location = "EastUS"

New-AzResourceGroup -Location $location -Name $rgName
$hostGroup = New-AzHostGroup `
   -Location $location `
   -Name myHostGroup `
   -PlatformFaultDomain 2 `
   -ResourceGroupName $rgName `
   -Zone 1
```

## <a name="create-a-host"></a>Ana bilgisayar oluşturma

Şimdi ev sahibi grubunda özel bir ana bilgisayar oluşturalım. Ev sahibi nin adına ek olarak, ev sahibi için SKU sağlamanız gerekir. Ana bilgisayar SKU, desteklenen VM serisinin yanı sıra özel ana bilgisayarınız için donanım üretimini de yakalar.

Ana bilgisayar SK'leri ve fiyatlandırması hakkında daha fazla bilgi için [Azure Özel Ana Bilgisayar fiyatlandırması'na](https://aka.ms/ADHPricing)bakın.

Ana bilgisayar grubunuz için bir hata etki alanı sayısı ayarlarsanız, ev sahibiniz için hata etki alanını belirtmeniz istenir. Bu örnekte, ana bilgisayar için hata etki alanını 1 olarak ayarlıyoruz.


```azurepowershell-interactive
$dHost = New-AzHost `
   -HostGroupName $hostGroup.Name `
   -Location $location -Name myHost `
   -ResourceGroupName $rgName `
   -Sku DSv3-Type1 `
   -AutoReplaceOnFailure 1 `
   -PlatformFaultDomain 1
```

## <a name="create-a-vm"></a>VM oluşturma

Özel ana bilgisayarda sanal bir makine oluşturun. 

Ana bilgisayar grubunuzu oluştururken bir kullanılabilirlik bölgesi belirttiyseniz, sanal makineoluştururken aynı bölgeyi kullanmanız gerekir. Bu örnekte, ana bilgisayar grubumuz bölge 1'de olduğundan, bölge 1'de VM oluşturmamız gerekir.  


```azurepowershell-interactive
$cred = Get-Credential
New-AzVM `
   -Credential $cred `
   -ResourceGroupName $rgName `
   -Location $location `
   -Name myVM `
   -HostId $dhost.Id `
   -Image Win2016Datacenter `
   -Zone 1 `
   -Size Standard_D4s_v3
```

> [!WARNING]
> Yeterli kaynağa sahip olmayan bir ana bilgisayarda sanal bir makine oluşturursanız, sanal makine BAŞARıSıZ durumda oluşturulur. 

## <a name="check-the-status-of-the-host"></a>Ev sahibinin durumunu denetleme

Ana bilgisayar durumu durumunu ve parametreile [GetAzHost'u](/powershell/module/az.compute/get-azhost) kullanarak ana bilgisayara `-InstanceView` kaç sanal makine dağıtabileceğinizi kontrol edebilirsiniz.

```azurepowershell-interactive
Get-AzHost `
   -ResourceGroupName $rgName `
   -Name myHost `
   -HostGroupName $hostGroup.Name `
   -InstanceView
```

Çıktı şuna benzer:

```
ResourceGroupName      : myDHResourceGroup
PlatformFaultDomain    : 1
AutoReplaceOnFailure   : True
HostId                 : 12345678-1234-1234-abcd-abc123456789
ProvisioningTime       : 7/28/2019 5:31:01 PM
ProvisioningState      : Succeeded
InstanceView           : 
  AssetId              : abc45678-abcd-1234-abcd-123456789abc
  AvailableCapacity    : 
    AllocatableVMs[0]  : 
      VmSize           : Standard_D2s_v3
      Count            : 32
    AllocatableVMs[1]  : 
      VmSize           : Standard_D4s_v3
      Count            : 16
    AllocatableVMs[2]  : 
      VmSize           : Standard_D8s_v3
      Count            : 8
    AllocatableVMs[3]  : 
      VmSize           : Standard_D16s_v3
      Count            : 4
    AllocatableVMs[4]  : 
      VmSize           : Standard_D32-8s_v3
      Count            : 2
    AllocatableVMs[5]  : 
      VmSize           : Standard_D32-16s_v3
      Count            : 2
    AllocatableVMs[6]  : 
      VmSize           : Standard_D32s_v3
      Count            : 2
    AllocatableVMs[7]  : 
      VmSize           : Standard_D64-16s_v3
      Count            : 1
    AllocatableVMs[8]  : 
      VmSize           : Standard_D64-32s_v3
      Count            : 1
    AllocatableVMs[9]  : 
      VmSize           : Standard_D64s_v3
      Count            : 1
  Statuses[0]          : 
    Code               : ProvisioningState/succeeded
    Level              : Info
    DisplayStatus      : Provisioning succeeded
    Time               : 7/28/2019 5:31:01 PM
  Statuses[1]          : 
    Code               : HealthState/available
    Level              : Info
    DisplayStatus      : Host available
Sku                    : 
  Name                 : DSv3-Type1
Id                     : /subscriptions/10101010-1010-1010-1010-101010101010/re
sourceGroups/myDHResourceGroup/providers/Microsoft.Compute/hostGroups/myHostGroup/hosts
/myHost
Name                   : myHost
Location               : eastus
Tags                   : {}
```

## <a name="add-an-existing-vm"></a>Varolan bir VM ekleme 

Özel bir ana bilgisayara varolan bir VM ekleyebilirsiniz, ancak VM'nin önce Stop\Deallocated olması gerekir. Bir VM'yi özel bir ana bilgisayara taşımadan önce, VM yapılandırmasının desteklendirildiğinden emin olun:

- VM boyutu, özel ana bilgisayarla aynı boyutta olmalıdır. Örneğin, özel ana bilgisayarınız DSv3 ise, VM boyutu Standard_D4s_v3 olabilir, ancak Standard_A4_v2 olamaz. 
- VM'nin özel ana bilgisayarla aynı bölgede bulunması gerekir.
- VM yakınlık yerleştirme grubunun bir parçası olamaz. VM'yi özel bir ana bilgisayara taşımadan önce yakınlık yerleşim grubundan çıkarın. Daha fazla bilgi için bkz: [VM'yi yakınlık yerleşim grubundan taşıma](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups#move-an-existing-vm-out-of-a-proximity-placement-group)
- VM kullanılabilirlik kümesinde olamaz.
- VM bir kullanılabilirlik bölgesindeyse, ana bilgisayar grubuyla aynı kullanılabilirlik bölgesi olmalıdır. VM ve ana bilgisayar grubu için kullanılabilirlik bölgesi ayarları eşleşmelidir.

Değişkenlerin değerlerini kendi bilgilerinizle değiştirin.

```azurepowershell-interactive
$vmRGName = "movetohost"
$vmName = "myVMtoHost"
$dhRGName = "myDHResourceGroup"
$dhGroupName = "myHostGroup"
$dhName = "myHost"

$myDH = Get-AzHost `
   -HostGroupName $dhGroupName `
   -ResourceGroupName $dhRGName `
   -Name $dhName
   
$myVM = Get-AzVM `
   -ResourceGroupName $vmRGName `
   -Name $vmName
   
$myVM.Host = New-Object Microsoft.Azure.Management.Compute.Models.SubResource

$myVM.Host.Id = "$myDH.Id"

Stop-AzVM `
   -ResourceGroupName $vmRGName `
   -Name $vmName -Force
   
Update-AzVM `
   -ResourceGroupName $vmRGName `
   -VM $myVM -Debug
   
Start-AzVM `
   -ResourceGroupName $vmRGName `
   -Name $vmName
```


## <a name="clean-up"></a>Temizleme

Sanal makineler dağıtılmasa bile, özel ana bilgisayarlarınız için ücretlendirilirsiniz. Maliyetleri kaydetmek için şu anda kullanmadığınız ana bilgisayarları silmeniz gerekir.  

Bir ana bilgisayarı yalnızca artık sanal makineler olmadığında silebilirsiniz. [Remove-AzVM](/powershell/module/az.compute/remove-azvm)kullanarak VM'leri silin.

```azurepowershell-interactive
Remove-AzVM -ResourceGroupName $rgName -Name myVM
```

VM'leri sildikten sonra [Remove-AzHost'u](/powershell/module/az.compute/remove-azhost)kullanarak ana bilgisayarı silebilirsiniz.

```azurepowershell-interactive
Remove-AzHost -ResourceGroupName $rgName -Name myHost
```

Tüm ana bilgisayarlarınızı sildikten [sonra, Remove-AzHostGroup'u](/powershell/module/az.compute/remove-azhostgroup)kullanarak ana bilgisayar grubunu silebilirsiniz. 

```azurepowershell-interactive
Remove-AzHost -ResourceGroupName $rgName -Name myHost
```

[Ayrıca Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)kullanarak tek bir komut tüm kaynak grubu silebilirsiniz. Bu, tüm VM'ler, ana bilgisayarlar ve ana bilgisayar grupları da dahil olmak üzere grupta oluşturulan tüm kaynakları siler.
 
```azurepowershell-interactive
Remove-AzResourceGroup -Name $rgName
```


## <a name="next-steps"></a>Sonraki adımlar

- Bir bölgede maksimum esneklik için hem bölgeleri hem de fay etki alanlarını kullanan örnek şablon [burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)bulunur.

- Azure [portalını](dedicated-hosts-portal.md)kullanarak özel ana bilgisayarları da dağıtabilirsiniz.
