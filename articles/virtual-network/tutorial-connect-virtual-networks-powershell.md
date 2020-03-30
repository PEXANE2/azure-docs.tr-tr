---
title: VNet eşlemeile sanal ağları bağlayın - Azure PowerShell
description: Bu makalede, Azure PowerShell'i kullanarak sanal ağları sanal ağ eşlemesiyle nasıl bağlayabileceğinizi öğreneceksiniz.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: caa620c2389bb6e2387636bc262ceb2de99d8e34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77201314"
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-powershell"></a>PowerShell'i kullanarak sanal ağ eşlemesiyle sanal ağları birbirine bağlayın

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Sanal ağ eşlemesi ile sanal ağları birbirine bağlayabilirsiniz. Sanal ağlar eşlendikten sonra, kaynaklar aynı sanal ağ üzerindeymiş gibi, aynı gecikme süresi ve bant genişliği ile her iki sanal ağdaki kaynaklar birbiriyle iletişim kurabilir. Bu makalede şunları öğreneceksiniz:

* İki sanal ağ oluşturma
* Sanal ağ eşlemesi iki sanal ağı bağlama
* Her sanal ağa sanal makine (VM) dağıtma
* Sanal makineler arasında iletişim

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

Sanal ağ oluşturmadan önce, sanal ağ ve bu makalede oluşturulan diğer tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile sanal ağ oluşturun. Aşağıdaki örnek, adres öneki *10.0.0.0/16*ile *myVirtualNetwork1* adlı bir sanal ağ oluşturur.

```azurepowershell-interactive
$virtualNetwork1 = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork1 `
  -AddressPrefix 10.0.0.0/16
```

[New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)ile bir alt net yapılandırması oluşturun. Aşağıdaki örnek, 10.0.0.0/24 adres öneki içeren bir alt net yapılandırması oluşturur:

```azurepowershell-interactive
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork1
```

Alt ağı oluşturan [Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork)ile sanal ağa alt ağ yapılandırmasını yazın:

```azurepowershell-interactive
$virtualNetwork1 | Set-AzVirtualNetwork
```

10.1.0.0/16 adres öneki ve bir alt net içeren bir sanal ağ oluşturun:

```azurepowershell-interactive
# Create the virtual network.
$virtualNetwork2 = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork2 `
  -AddressPrefix 10.1.0.0/16

# Create the subnet configuration.
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name Subnet1 `
  -AddressPrefix 10.1.0.0/24 `
  -VirtualNetwork $virtualNetwork2

# Write the subnet configuration to the virtual network.
$virtualNetwork2 | Set-AzVirtualNetwork
```

## <a name="peer-virtual-networks"></a>Sanal ağları eşleme

[Add-AzVirtualNetworkPeering](/powershell/module/az.network/add-azvirtualnetworkpeering)ile bir eşleme oluşturun. Aşağıdaki örnek *myVirtualNetwork1* *myVirtualNetwork2*eşler.

```azurepowershell-interactive
Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork1-myVirtualNetwork2 `
  -VirtualNetwork $virtualNetwork1 `
  -RemoteVirtualNetworkId $virtualNetwork2.Id
```

Önceki komut yürütüldünden sonra döndürülen çıktıda, **PeeringState'in** *başlatıldığını*görürsünüz. *MyVirtualNetwork2'den myVirtualNetwork1'e* kadar eşleme oluşturana *myVirtualNetwork1*kadar eşleme *Başlatılan* durumda kalır. *myVirtualNetwork2'den* *myVirtualNetwork1'e*bir eşleme oluşturun.

```azurepowershell-interactive
Add-AzVirtualNetworkPeering `
  -Name myVirtualNetwork2-myVirtualNetwork1 `
  -VirtualNetwork $virtualNetwork2 `
  -RemoteVirtualNetworkId $virtualNetwork1.Id
```

Önceki komut yürütüldünden sonra döndürülen çıktıda, **PeeringState'in** *Bağlı*olduğunu görürsünüz. Azure ayrıca *myVirtualNetwork1-myVirtualNetwork2'nin* *Connected'e*bakarak bakış durumunu da değiştirdi. *MyVirtualNetwork1-myVirtualNetwork2'nin* eşleme durumunun [Get-AzVirtualNetworkPeering](/powershell/module/az.network/get-azvirtualnetworkpeering)ile *Bağlantılı* olarak değiştirilmesini onaylayın.

```azurepowershell-interactive
Get-AzVirtualNetworkPeering `
  -ResourceGroupName myResourceGroup `
  -VirtualNetworkName myVirtualNetwork1 `
  | Select PeeringState
```

Bir sanal ağdaki kaynaklar, her iki sanal ağdaki **eşlemeler için PeeringState Bağlanıncaya** kadar diğer sanal ağdaki kaynaklarla iletişim kuramaz. *Connected*

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sonraki bir adımda aralarında iletişim kurabilmeniz için her sanal ağ üzerinde bir sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

[New-AzVM](/powershell/module/az.compute/new-azvm)ile VM oluşturun. Aşağıdaki örnek, *myVirtualNetwork1* sanal ağında *myVm1* adında bir VM oluşturur. Seçenek `-AsJob` arka planda VM oluşturur, böylece bir sonraki adıma devam edebilirsiniz. İstendiğinde, VM'de oturum açmak istediğiniz kullanıcı adını ve parolayı girin.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork1" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm1" `
  -AsJob
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork2" `
  -SubnetName "Subnet1" `
  -ImageName "Win2016Datacenter" `
  -Name "myVm2"
```

Sanal makinenin oluşturulması birkaç dakika sürer. Azure VM'yi oluşturup çıktıyı PowerShell'e döndürene kadar sonraki adımlarla devam etmeyin.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

Bir VM'nin genel IP adresine internetten bağlanabilirsiniz. Bir VM'nin genel IP adresini döndürmek için [Get-AzPublicIpAddress'i](/powershell/module/az.network/get-azpublicipaddress) kullanın. Aşağıdaki örnek,*myVm1* sanal makinesinin genel IP adresini döndürür:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup | Select IpAddress
```

Yerel bilgisayarınızdan *myVm1* VM ile uzak bir masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. `<publicIpAddress>` değerini önceki komutta döndürülen IP adresi ile değiştirin.

```
mstsc /v:<publicIpAddress>
```

Uzak Masaüstü Protokolü (.rdp) dosyası oluşturulur, bilgisayarınıza indirilir ve açılır. Kullanıcı adı ve parolagirin **(Daha fazla seçenek**seçmeniz gerekebilir, ardından VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için farklı bir hesap **kullanın)** ve **ardından Tamam'ı**tıklatın. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet** veya **Devam**’a tıklayın.

*myVm1* VM'de, PowerShell'i kullanarak bu *VM'yi myVm2'den* daha sonraki bir adımda pingleyebilmeniz için Windows güvenlik duvarından Internet Control Message Protocol'ı (ICMP) etkinleştirin:

```powershell
New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
```

Ping bu makalede VM'ler arasında iletişim kurmak için kullanılsa da, üretim dağıtımları için ICMP'nin Windows Güvenlik Duvarı'ndan geçmesine izin vermek önerilmez.

*myVm2* sanal makinesine bağlanmak için, *myVm1* sanal makinesinde bir komut isteminden aşağıdaki komutu girin:

```
mstsc /v:10.1.0.4
```

*myVm1'de*ping'i etkinleştirdiğiniz için, şimdi *myVm2* VM'deki bir komut isteminden IP adresine göre ping atabilirsiniz:

```
ping 10.0.0.4
```

Dört yanıt alırsınız. Hem *myVm1* hem de *myVm2* sanal makinesine yönelik RDP oturumlarınızın bağlantısını kesin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzKaynak Grubu'nu](/powershell/module/az.resources/remove-azresourcegroup) kullanın.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, aynı Azure bölgesindeki iki ağın sanal ağ eşlemesiyle nasıl bağlandığınızı öğrendiniz. Farklı [desteklenen bölgelerde](virtual-network-manage-peering.md#cross-region) ve [farklı Azure aboneliklerinde](create-peering-different-subscriptions.md#powershell) sanal ağları eşleyebilir ve eşleme ile [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke#virtual-network-peering) oluşturabilirsiniz. Sanal ağ eşlemesi hakkında daha fazla bilgi için bkz. [Sanal ağ eşlemesine genel bakış](virtual-network-peering-overview.md) ve [Sanal ağ eşlemelerini yönetme](virtual-network-manage-peering.md).

VPN üzerinden [kendi bilgisayarınızı sanal ağa bağlayabilir](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve sanal ağdaki veya eşlenen sanal ağlardaki kaynaklarla etkileşimkurabilirsiniz. Yeniden kullanılabilir komut dosyaları için sanal ağ makalelerinde kapsanan görevlerin çoğunu tamamlamak için [komut dosyası örneklerine](powershell-samples.md)bakın.