---
title: Azure Spot VM'lerini dağıtmak için PowerShell'i kullanma
description: Maliyetlerden tasarruf etmek için Spot VM'leri dağıtmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 03/25/2020
ms.author: cynthn
ms.openlocfilehash: 234cf3f51173c53ef8ca15af4ca6f24881be3109
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80547276"
---
# <a name="deploy-spot-vms-using-azure-powershell"></a>Azure PowerShell'i kullanarak Spot VM'leri dağıtma


[Spot VM'leri](spot-vms.md) kullanmak, önemli bir maliyet tasarrufu nda kullanılmayan kapasitemizden yararlanmanızı sağlar. Azure'un kapasiteye geri ihtiyacı olduğu herhangi bir zamanda, Azure altyapısı Spot VM'leri boşaltacaktır. Bu nedenle, Spot VM'ler toplu iş işleri, geliştirme/test ortamları, büyük bilgi işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilir iş yükleri için idealdir.

Spot VM'ler için fiyatlandırma, bölgeye ve SKU'ya göre değişkendir. Daha fazla bilgi için [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması için bkz. Maksimum fiyatı ayarlama hakkında daha fazla bilgi için [Spot VM'ler - Fiyatlandırma'ya](spot-vms.md#pricing)bakın.

VM için saat başına ödemek istediğiniz maksimum fiyatı belirleme seçeneğiniz vardır. Spot VM'nin maksimum fiyatı, 5 ondalık basamak kullanılarak ABD doları (USD) olarak ayarlanabilir. Örneğin, değeri `0.98765`saatte 0,98765 USD maksimum fiyat olacaktır. Maksimum fiyatı olarak `-1`ayarlarsanız, VM fiyata göre tahliye olmaz. VM için fiyat spot veya standart bir VM için fiyat olacak, hangi hiç daha az, sürece kapasite ve kota mevcuttur.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Yapılandırmayı oluşturmak için [New-AzVmConfig](/powershell/module/az.compute/new-azvmconfig) kullanarak bir spotVM oluşturun. Şunları `-Priority Spot` ekleyin `-MaxPrice` ve aşağıdakileri ayarlayın:
- `-1`böylece VM fiyata göre tahliye edilmez.
- bir dolar miktarı, en fazla 5 haneli. Örneğin, `-MaxPrice .98765` bir spotVM fiyatı saatte yaklaşık $.98765 gider kez VM tahsis edilecektir anlamına gelir.


Bu örnek, fiyatlandırmaya dayalı olarak tahsis edilmeyecek bir spotVM oluşturur (yalnızca Azure kapasiteye geri ihtiyaç duyduğunda).

```azurepowershell-interactive
$resourceGroup = "mySpotRG"
$location = "eastus"
$vmName = "mySpotVM"
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."
New-AzResourceGroup -Name $resourceGroup -Location $location
$subnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name mySubnet -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork -ResourceGroupName $resourceGroup `
   -Location $location -Name MYvNET -AddressPrefix 192.168.0.0/16 `
   -Subnet $subnetConfig
$pip = New-AzPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration and set this to be a Spot VM

$vmConfig = New-AzVMConfig -VMName $vmName -VMSize Standard_D1 -Priority "Spot" -MaxPrice -1| `
Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred | `
Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzVMNetworkInterface -Id $nic.Id

New-AzVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

VM oluşturulduktan sonra, kaynak grubundaki tüm VM'lerin maksimum fiyatını görmek için sorgu layabilirsiniz.

```azurepowershell-interactive
Get-AzVM -ResourceGroupName $resourceGroup | `
   Select-Object Name,@{Name="maxPrice"; Expression={$_.BillingProfile.MaxPrice}}
```

## <a name="next-steps"></a>Sonraki adımlar

[Ayrıca Azure CLI'yi](../linux/spot-cli.md) veya [şablonu](../linux/spot-template.md)kullanarak bir Spot VM oluşturabilirsiniz.

Bir hatayla karşılaşırsanız, [hata kodlarına](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.