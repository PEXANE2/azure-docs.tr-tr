---
title: Rota ağ trafiği Azure PowerShell | Microsoft Dokümanlar
description: Bu makalede, PowerShell'i kullanarak bir rota tablosuyla ağ trafiğini nasıl yönlendireceklerini öğrenin.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 986371e479f7718fff2e1699401987cb0ca8f623
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73163997"
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>PowerShell'i kullanarak bir rota tablosuyla ağ trafiğini yönlendirme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Azure varsayılan olarak bir sanal ağ içindeki tüm alt ağlar arasında gerçekleşen trafiği otomatik olarak yönlendirir. Azure’ın varsayılan yönlendirmesini geçersiz kılmak için kendi yönlendirmelerinizi oluşturabilirsiniz. Örneğin, bir ağ sanal gereci üzerinden alt ağlar arasındaki trafiği yönlendirmek isteyebilirsiniz. Bu makalede şunları öğreneceksiniz:

* Yönlendirme tablosu oluşturma
* Yönlendirme oluşturma
* Birden fazla alt ağa sahip bir sanal ağ oluşturma
* Yönlendirme tablosunu bir alt ağ ile ilişkilendirme
* Trafiği yönlendiren bir NVA oluşturma
* Sanal makineleri (VM) farklı alt ağlara dağıtma
* NVA aracılığıyla trafiği bir alt ağdan başka birine yönlendirme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-route-table"></a>Yönlendirme tablosu oluşturma

Bir rota tablosu oluşturmadan önce, [Yeni Kaynak Grubu](/powershell/module/az.resources/new-azresourcegroup)ile bir kaynak grubu oluşturun. Aşağıdaki örnek, bu makalede oluşturulan tüm kaynaklar için *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

[New-AzRouteTable](/powershell/module/az.network/new-azroutetable)ile bir rota tablosu oluşturun. Aşağıdaki örnek, *myRouteTablePublic*adlı bir rota tablosu oluşturur.

```azurepowershell-interactive
$routeTablePublic = New-AzRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Yönlendirme oluşturma

[Get-AzRouteTable](/powershell/module/az.network/get-azroutetable)ile rota tablosu nesnesini alarak bir rota oluşturun, [Add-AzRouteConfig](/powershell/module/az.network/add-azrouteconfig)ile bir rota oluşturun, ardından Rota yapılandırmasını [Set-AzRouteTable](/powershell/module/az.network/set-azroutetable)ile rota tablosuna yazın.

```azurepowershell-interactive
Get-AzRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzRouteTable
```

## <a name="associate-a-route-table-to-a-subnet"></a>Yönlendirme tablosunu bir alt ağ ile ilişkilendirme

Bir rota tablosunu bir alt ağla ilişkilendirebilmek için sanal bir ağ ve alt ağ oluşturmanız gerekir. [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile sanal ağ oluşturun. Aşağıdaki örnek, adres öneki *10.0.0.0/16*ile *myVirtualNetwork* adlı bir sanal ağ oluşturur.

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

[New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)ile üç alt net yapılandırmaları oluşturarak üç alt ağ oluşturun. Aşağıdaki örnek, *Genel*, *Özel*ve *DMZ* alt ağları için üç alt net yapılandırması oluşturur:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

Sanal ağdaki alt ağları oluşturan [Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork)ile alt ağ yapılandırmalarını sanal ağa yazın:

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

*MyRouteTablePublic* rota tablosunu [Set-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) ile *Ortak* alt ağa bağlayın ve alt ağ yapılandırmasını [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork)ile sanal ağa yazın.

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzVirtualNetwork
```

## <a name="create-an-nva"></a>NVA oluşturma

NVA; yönlendirme, güvenlik duvarı oluşturma veya WAN iyileştirmesi gibi ağ işlevlerini gerçekleştiren bir VM'dir.

VM oluşturmadan önce bir ağ arabirimi oluşturun.

### <a name="create-a-network-interface"></a>Ağ arabirimi oluşturma

Bir ağ arayüzü oluşturmadan önce, [Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork)ile sanal ağ Id almak zorunda , sonra [Get-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig)ile subnet Id . IP iletme etkin *dmz* alt ağında [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) ile bir ağ arabirimi oluşturun:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

### <a name="create-a-vm"></a>VM oluşturma

Bir VM oluşturmak ve ona varolan bir ağ arabirimi eklemek için, öncelikle [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig)ile bir VM yapılandırması oluşturmanız gerekir. Yapılandırma, önceki adımda oluşturulan ağ arabirimini içerir. Kullanıcı adı ve parola istendiğinde, VM'de oturum açmak istediğiniz kullanıcı adını ve parolayı seçin.

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
$vmConfig = New-AzVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzVMNetworkInterface -Id $nic.Id
```

[New-AzVM](/powershell/module/az.compute/new-azvm)ile VM yapılandırmasını kullanarak VM'yi oluşturun. Aşağıdaki *örnekmyVmNva*adlı bir VM oluşturur.

```azurepowershell-interactive
$vmNva = New-AzVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

Seçenek `-AsJob` arka planda VM oluşturur, böylece bir sonraki adıma devam edebilirsiniz.

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Daha sonraki bir *adımda, Genel* alt ağdan gelen trafiğin ağ sanal cihazı üzerinden *Özel* alt ağa yönlendirildiğini doğrulamak için sanal ağda iki VM oluşturun.

[New-AzVM](/powershell/module/az.compute/new-azvm)ile *Ortak* alt ağda bir VM oluşturun. Aşağıdaki örnek, *myVirtualNetwork* sanal ağının *Ortak* alt ağında *myVmPublic* adında bir VM oluşturur.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPublic" `
  -AsJob
```

*Özel* alt ağda bir VM oluşturun.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulana ve Azure çıktıyı PowerShell'e döndürene kadar bir sonraki adımla devam etmeyin.

## <a name="route-traffic-through-an-nva"></a>Trafiği NVA üzerinden yönlendirme

*myVmPrivate* VM'nin genel IP adresini döndürmek için [Get-AzPublicIpAddress'i](/powershell/module/az.network/get-azpublicipaddress) kullanın. Aşağıdaki örnek, *myVmPrivate* VM'nin genel IP adresini döndürür:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Yerel bilgisayarınızdan *myVmPrivate* VM ile uzak bir masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. `<publicIpAddress>` değerini önceki komutta döndürülen IP adresi ile değiştirin.

```
mstsc /v:<publicIpAddress>
```

İndirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.

Sanal makineyi oluştururken belirttiğiniz kullanıcı adını ve parolayı girin (sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan**’ı seçmeniz gerekebilir), ardından **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet**’i seçin.

Daha sonraki bir `tracert.exe` adımda, komut yönlendirmeyi sınamak için kullanılır. Tracert, Windows Güvenlik Duvarı aracılığıyla reddedilen Internet Denetim İleti Protokolü'nü (ICMP) kullanır. PowerShell’den *myVmPrivate* sanal makinesinde aşağıdaki komutu girerek Windows güvenlik duvarı üzerinden ICMP’yi etkinleştirin:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

İzleme rotası bu makalede yönlendirmeyi sınamak için kullanılsa da, üretim dağıtımları için ICMP'nin Windows Güvenlik Duvarı'ndan geçmesine izin vermek önerilmez.

IP iletmeyi etkinleştirme bölümünde, VM'nin ağ arabirimi için Azure’da IP iletimini etkinleştirdiniz. VM içindeki işletim sistemi veya VM içinde çalışan bir uygulama da ağ trafiğini iletebilmelidir. *myVmNva*işletim sistemi içinde IP iletme etkinleştirin.

*myVmPrivate* VM'deki komut isteminden, uzak masaüstünden *myVmNva'ya:*

``` 
mstsc /v:myvmnva
```

İşletim sistemi içinde IP iletmeyi etkinleştirmek için *myVmNva* sanal makinesinden PowerShell’de aşağıdaki komutu girin:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```

*myVmNva* sanal makinesini yeniden başlatarak uzak masaüstü oturumunun bağlantısını da kesin.

*myVmPrivate* VM bağlantısı devam ederken, *myVmNva* VM yeniden başlatıldıktan sonra *myVmPublic* VM ile bir uzak masaüstü oturumu oluşturun:

``` 
mstsc /v:myVmPublic
```

PowerShell’den *myVmPublic* sanal makinesinde aşağıdaki komutu girerek Windows güvenlik duvarı üzerinden ICMP’yi etkinleştirin:

```powershell
New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
```

Ağ trafiğinin *myVmPrivate* sanal makinesinden *myVmPublic* sanal makinesine yönlendirilmesini test etmek için *myVmPublic* sanal makinesinde PowerShell’den aşağıdaki komutu girin:

```
tracert myVmPrivate
```

Yanıt aşağıdaki örneğe benzer:

```
Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:

1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4

Trace complete.
```

İlk atlamanın, NVA özel IP adresi olan 10.0.2.4 olduğunu görebilirsiniz. İkinci atlama ise *myVmPrivate* sanal makinesinin özel IP adresi olan 10.0.1.4’tür. *myRouteTablePublic* yönlendirme tablosuna eklenip *Genel* alt ağ ile ilişkilendirilen yönlendirme, Azure’un trafiği doğrudan *Özel* alt ağına yönlendirmek yerine NVA aracılığıyla yönlendirmesine neden olmuştur.

*myVmPublic* VM ile uzak masaüstü oturumunu kapatın. *myVmPrivate* VM bağlantınız hala açıktır.

Ağ trafiğinin *myVmPublic* sanal makinesinden *myVmPrivate* sanal makinesine yönlendirilmesini test etmek için *myVmPrivate* sanal makinesinde bir komut isteminden aşağıdaki komutu girin:

```
tracert myVmPublic
```

Yanıt aşağıdaki örneğe benzer:

```
Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:

1     1 ms     1 ms     1 ms  10.0.0.4

Trace complete.
```

Trafiğin *myVmPrivate* sanal makinesinden *myVmPublic* sanal makinesine doğrudan yönlendirildiğini görebilirsiniz. Varsayılan olarak Azure, trafiği doğrudan alt ağlar arasında yönlendirir.

*myVmPrivate* VM ile uzak masaüstü oturumunu kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzKaynak Grubu'nu](/powershell/module/az.resources/remove-azresourcegroup) kullanın.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir rota tablosu oluşturdunuz ve bir alt ağla ilişkilendirdin. Trafiği ortak bir alt ağdan özel bir alt ağa yönlendiren basit bir ağ sanal cihazı oluşturdunuz. [Azure](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking)Marketi'nden güvenlik duvarı ve WAN optimizasyonu gibi ağ işlevlerini gerçekleştiren önceden yapılandırılmış ağ sanal aygıtları dağıtın. Yönlendirme hakkında daha fazla bilgi için bkz. [Yönlendirmeye genel bakış](virtual-networks-udr-overview.md) ve [Yönlendirme tablosunu yönetme](manage-route-table.md).

Bir sanal ağ içinde çok sayıda Azure kaynağına dağıtabilmenize karşın, bazı Azure PaaS hizmetlerinin kaynakları bir sanal ağa dağıtılamaz. Yine de, bazı Azure PaaS hizmetlerinin kaynaklarına erişimi yalnızca bir sanal ağ alt ağından gelecek trafikle kısıtlayabilirsiniz. Nasıl yapılacağını öğrenmek [için](tutorial-restrict-network-access-to-resources-powershell.md)bkz.