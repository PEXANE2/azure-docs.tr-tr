---
title: Azure spot VM 'Leri dağıtmak için PowerShell 'i kullanma
description: Maliyetlerden tasarruf etmek için spot VM 'Leri dağıtmak üzere Azure PowerShell nasıl kullanacağınızı öğrenin.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 10/14/2019
ms.author: cynthn
ms.openlocfilehash: 8752522e4b5a7b91778d6eb2cd8e4ba3bac95da0
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74782131"
---
# <a name="preview-deploy-spot-vms-using-azure-powershell"></a>Önizleme: Azure PowerShell kullanarak spot VM 'Leri dağıtma


[Spot VM 'lerin](spot-vms.md) kullanılması, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi her zaman yapması gerektiğinde, Azure altyapısı spot VM 'Leri çıkarır. Bu nedenle, spot VM 'Ler toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için mükemmeldir.

Nokta VM 'Leri için fiyatlandırma, bölge ve SKU temel alınarak değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması. En yüksek fiyatı ayarlama hakkında daha fazla bilgi için bkz. [spot VM 'ler-fiyatlandırma](spot-vms.md#pricing).

VM için saat başına ödeme yapmak istediğiniz maksimum fiyatı ayarlama seçeneğiniz vardır. Bir spot VM 'nin en yüksek fiyatı, en fazla 5 ondalık basamak kullanılarak ABD Doları (USD) olarak ayarlanabilir. Örneğin değer `0.98765`, saat başına $0,98765 ABD Doları olan en yüksek fiyat olacaktır. En büyük fiyatı `-1`olarak ayarlarsanız, VM fiyata göre çıkarılmaz. Kapasite ve kota kullanılabilir olduğu sürece, sanal makine fiyatı, nokta için geçerli fiyat veya standart bir sanal makine fiyatı olacaktır.

> [!IMPORTANT]
> Nokta örnekleri şu anda genel önizlemededir.
> Bu önizleme sürümü üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Genel önizlemenin erken bölümü için, spot örnekleri sabit bir fiyata sahip olur, bu nedenle fiyat tabanlı çıkarmalar olmayacaktır.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Yapılandırmayı oluşturmak için [New-AzVmConfig](/powershell/module/az.compute/new-azvmconfig) kullanarak bir spotvm oluşturun. `-Priority Spot` ekleyin ve `-MaxPrice` birine ayarlayın:
- `-1`, VM fiyata çıkarılmaz.
- en fazla 5 basamaklı bir dolar miktarı. Örneğin, `-MaxPrice .98765` bir spotVM 'nin fiyatı yaklaşık olarak $. 98765 ' e geçtiğinde VM 'nin serbest bırakılacak olması anlamına gelir.

> [!IMPORTANT]
> Genel önizlemenin erken bölümü için, en yüksek fiyat ayarlayabilirsiniz ancak yok sayılır. Spot VM 'Ler sabit bir fiyata sahip olacak, bu nedenle fiyat tabanlı çıkarmalar olmayacaktır.


Bu örnek, fiyatlandırma temelinde serbest bırakılmayacak bir spotVM oluşturur (yalnızca Azure 'un kapasiteyi geri sağlaması gerektiğinde).

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

VM oluşturulduktan sonra, kaynak grubundaki tüm VM 'Lerin en yüksek fiyatını görmek için sorgulama yapabilirsiniz.

```azurepowershell-interactive
Get-AzVM -ResourceGroupName $resourceGroup | `
   Select-Object Name,@{Name="maxPrice"; Expression={$_.BillingProfile.MaxPrice}}
```

## <a name="next-steps"></a>Sonraki adımlar

[Azure CLI](../linux/spot-cli.md) veya [şablon](../linux/spot-template.md)kullanarak bir spot VM de oluşturabilirsiniz.

Bir hatayla karşılaşırsanız bkz. [hata kodları](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).