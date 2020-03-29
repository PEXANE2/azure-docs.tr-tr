---
title: Filtre ağ trafiği - Azure PowerShell | Microsoft Dokümanlar
description: Bu makalede, PowerShell'i kullanarak ağ trafiğini bir ağ güvenlik grubuyla bir alt ağa nasıl filtreuygulayabileceğinizi öğrenirsiniz.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/30/2018
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 08031bc2ac29ea77374e21c4ce6f7bcf6151bcad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66730029"
---
# <a name="filter-network-traffic-with-a-network-security-group-using-powershell"></a>PowerShell'i kullanan bir ağ güvenlik grubuyla ağ trafiğini filtreleme

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir sanal ağ alt ağına gelen ve sanal ağ alt ağından giden ağ trafiğini, bir ağ güvenlik grubu ile filtreleyebilirsiniz. Ağ güvenlik grupları, ağ trafiğini IP adresi, bağlantı noktası ve protokole göre filtreleyen güvenlik kuralları içerir. Güvenlik kuralları bir alt ağda dağıtılmış kaynaklara uygulanır. Bu makalede şunları öğreneceksiniz:

* Ağ güvenlik grubu ve güvenlik kuralları oluşturma
* Bir sanal ağ oluşturma ve ağ güvenlik grubunu alt ağ ile ilişkilendirme
* Sanal makineleri (VM) bir alt ağa dağıtma
* Trafik filtrelerini test etme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure PowerShell modülü sürümü 1.0.0 veya daha sonra gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.

## <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

Bir ağ güvenlik grubu, güvenlik kuralları içerir. Güvenlik kuralları, bir kaynak ve hedefi belirtir. Kaynaklar ve hedefler, uygulama güvenlik grupları olabilir.

### <a name="create-application-security-groups"></a>Uygulama güvenlik grupları oluşturma

Önce [Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bu makalede oluşturulan tüm kaynaklar için bir kaynak grubu oluşturun. Aşağıdaki örnekte *eastus* konumunda bir kaynak grubu oluşturulmaktadır:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

[New-AzApplicationSecurityGroup](/powershell/module/az.network/new-azapplicationsecuritygroup)ile bir uygulama güvenlik grubu oluşturun. Uygulama güvenlik grubu, benzer bağlantı noktası filtreleme gereksinimlerine sahip sunucuları gruplandırmanızı sağlar. Aşağıdaki örnek iki uygulama güvenlik grubu oluşturur.

```azurepowershell-interactive
$webAsg = New-AzApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgWebServers `
  -Location eastus

$mgmtAsg = New-AzApplicationSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Name myAsgMgmtServers `
  -Location eastus
```

### <a name="create-security-rules"></a>Güvenlik kuralları oluşturma

[New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig)ile bir güvenlik kuralı oluşturun. Aşağıdaki örnek, internetten gelen trafiğin 80 ve 443 numaralı bağlantı noktaları üzerinden *myWebServers* uygulama güvenlik grubuna gitmesine izin veren bir kural oluşturur:

```azurepowershell-interactive
$webRule = New-AzNetworkSecurityRuleConfig `
  -Name "Allow-Web-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 100 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $webAsg.id `
  -DestinationPortRange 80,443

The following example creates a rule that allows traffic inbound from the internet to the *myMgmtServers* application security group over port 3389:

$mgmtRule = New-AzNetworkSecurityRuleConfig `
  -Name "Allow-RDP-All" `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 110 `
  -SourceAddressPrefix Internet `
  -SourcePortRange * `
  -DestinationApplicationSecurityGroupId $mgmtAsg.id `
  -DestinationPortRange 3389
```

Bu makalede, RDP (port 3389) *myAsgMgmtServers* VM için internete maruz kalmaktadır. Üretim ortamları için, 3389 bağlantı noktasını internete maruz bırakmak yerine, [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [özel](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ağ bağlantısı kullanarak yönetmek istediğiniz Azure kaynaklarına bağlanmanız önerilir.

### <a name="create-a-network-security-group"></a>Ağ güvenlik grubu oluşturma

[New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup)ile bir ağ güvenlik grubu oluşturun. Aşağıdaki örnek *myNsg* adlı bir ağ güvenlik grubu oluşturur:

```powershell-interactive
$nsg = New-AzNetworkSecurityGroup `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myNsg `
  -SecurityRules $webRule,$mgmtRule
```

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile sanal ağ oluşturun. Aşağıdaki örnek *myVirtualNetwork* adlı bir sanal ağ oluşturur:

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

[New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)ile bir alt net yapılandırması oluşturun ve ardından [Set-AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork)ile sanal ağa subnet yapılandırmasını yazın. Aşağıdaki örnek, sanal ağa *mySubnet* adlı bir alt ağ ekler ve *myNsg* ağ güvenlik grubunu onunla ilişkilendirir:

```azurepowershell-interactive
Add-AzVirtualNetworkSubnetConfig `
  -Name mySubnet `
  -VirtualNetwork $virtualNetwork `
  -AddressPrefix "10.0.2.0/24" `
  -NetworkSecurityGroup $nsg
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

VM'leri oluşturmadan [önce, Get-AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork)ile alt ağ ile sanal ağ nesnesini alın:

```powershell-interactive
$virtualNetwork = Get-AzVirtualNetwork `
 -Name myVirtualNetwork `
 -Resourcegroupname myResourceGroup
```

[New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress)ile her VM için ortak bir IP adresi oluşturun:

```powershell-interactive
$publicIpWeb = New-AzPublicIpAddress `
  -AllocationMethod Dynamic `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myVmWeb

$publicIpMgmt = New-AzPublicIpAddress `
  -AllocationMethod Dynamic `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -Name myVmMgmt
```

[New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface)ile iki ağ arabirimi oluşturun ve ağ arabirimine ortak bir IP adresi atayın. Aşağıdaki örnek bir ağ arabirimi oluşturur, *myVmWeb* genel IP adresini onunla ilişkilendirir ve *myAsgWebServers* uygulama güvenlik grubunun bir üyesi yapar:

```powershell-interactive
$webNic = New-AzNetworkInterface `
  -Location eastus `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $webAsg.Id `
  -PublicIpAddressId $publicIpWeb.Id
```

Aşağıdaki örnek bir ağ arabirimi oluşturur, *myVmMgmt* genel IP adresini onunla ilişkilendirir ve *myAsgMgmtServers* uygulama güvenlik grubunun bir üyesi yapar:

```powershell-interactive
$mgmtNic = New-AzNetworkInterface `
  -Location eastus `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  -SubnetId $virtualNetwork.Subnets[0].Id `
  -ApplicationSecurityGroupId $mgmtAsg.Id `
  -PublicIpAddressId $publicIpMgmt.Id
```

Daha sonraki bir adımda trafik filtrelemesini doğrulayabilmek için sanal ağda iki VM oluşturun.

[New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig)ile bir VM yapılandırması oluşturun, sonra [New-AzVM](/powershell/module/az.compute/new-azvm)ile VM oluşturun. Aşağıdaki örnek, web sunucusu olarak görev yapacak bir VM oluşturur. `-AsJob` seçeneği, sonraki adıma devam edebilmeniz için arka planda sanal makineyi oluşturur:

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

$webVmConfig = New-AzVMConfig `
  -VMName myVmWeb `
  -VMSize Standard_DS1_V2 | `
Set-AzVMOperatingSystem -Windows `
  -ComputerName myVmWeb `
  -Credential $cred | `
Set-AzVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzVMNetworkInterface `
  -Id $webNic.Id
New-AzVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $webVmConfig `
  -AsJob
```

Yönetim sunucusu olarak görev yapacak bir VM oluşturun:

```azurepowershell-interactive
# Create user object
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."

# Create the web server virtual machine configuration and virtual machine.
$mgmtVmConfig = New-AzVMConfig `
  -VMName myVmMgmt `
  -VMSize Standard_DS1_V2 | `
Set-AzVMOperatingSystem -Windows `
  -ComputerName myVmMgmt `
  -Credential $cred | `
Set-AzVMSourceImage `
  -PublisherName MicrosoftWindowsServer `
  -Offer WindowsServer `
  -Skus 2016-Datacenter `
  -Version latest | `
Add-AzVMNetworkInterface `
  -Id $mgmtNic.Id
New-AzVM `
  -ResourceGroupName myResourceGroup `
  -Location eastus `
  -VM $mgmtVmConfig
```

Sanal makinenin oluşturulması birkaç dakika sürer. Azure VM oluşturma işlemini tamamlayana kadar sonraki adıma geçmeyin.

## <a name="test-traffic-filters"></a>Trafik filtrelerini test etme

Bir VM'nin genel IP adresini döndürmek için [Get-AzPublicIpAddress'i](/powershell/module/az.network/get-azpublicipaddress) kullanın. Aşağıdaki örnek,*myVmMgmt* sanal makinesinin genel IP adresini döndürür:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmMgmt `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Yerel bilgisayarınızdan *myVmMgmt* ile bir uzak masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. `<publicIpAddress>` değerini önceki komutta döndürülen IP adresi ile değiştirin.

```
mstsc /v:<publicIpAddress>
```

İndirilen RDP dosyasını açın. İstendiğinde **Bağlan**’ı seçin.

Sanal makineyi oluştururken belirttiğiniz kullanıcı adını ve parolayı girin (sanal makineyi oluştururken girdiğiniz kimlik bilgilerini belirtmek için **Diğer seçenekler**’i ve sonra **Farklı bir hesap kullan**’ı seçmeniz gerekebilir), ardından **Tamam**’ı seçin. Oturum açma işlemi sırasında bir sertifika uyarısı alabilirsiniz. Bağlantıya devam etmek için **Evet**’i seçin.

*myVmMgmt* sanal makinesine bağlı ağ arabiriminin içinde bulunduğu *myAsgMgmtServers* uygulama güvenlik grubuna 3389 numaralı bağlantı noktasının internetten gelmesine izin verildiği için bağlantı başarılı olur.

PowerShell üzerinden *myVmWeb* sanal makinesi ile *myVmMgmt* sanal makinesi arasında bir uzak masaüstü bağlantısı oluşturmak için aşağıdaki komutu kullanın:

``` 
mstsc /v:myvmWeb
```

Her bir ağ güvenlik grubu içindeki varsayılan güvenlik kuralı bir sanal ağ içindeki tüm IP adresleri arasında tüm bağlantı noktaları üzerinden trafiğe izin verdiği için bağlantı başarılı olur. *myAsgWebServers* güvenlik kuralı internetten 3389 numaralı gelen bağlantı noktasına izin vermediği için *myVmWeb* sanal makinesi ile bir uzak masaüstü bağlantısı oluşturamazsınız.

PowerShell’den *myVmWeb* sanal makinesine Microsoft IIS yüklemek için aşağıdaki komutu kullanın:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

IIS yüklemesi tamamlandıktan sonra *myVmWeb* sanal makinesinin bağlantısını kesmeniz durumunda *myVmMgmt* VM uzak masaüstü bağlantısında kalırsınız. IIS karşılama ekranını görüntülemek için bir internet tarayıcısı\/açın ve http: /myVmWeb adresine göz atın.

*myVmMgmt* sanal makinesiyle bağlantıyı kesin.

Bilgisayarınızda *myVmWeb* sunucusundan genel IP adresini almak için Powershell'den aşağıdaki komutu girin:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmWeb `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

*myVmWeb* web sunucusuna Azure dışından erişebildiğinizi onaylamak için bilgisayarınızda bir internet tarayıcısı açın ve `http://<public-ip-address-from-previous-step>` sayfasına göz atın. *myVmWeb* sanal makinesine bağlı ağ arabiriminin içinde bulunduğu *myAsgWebServers* uygulama güvenlik grubuna 80 numaralı bağlantı noktasının internetten gelmesine izin verildiği için bağlantı başarılı olur.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup) kullanabilirsiniz:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir ağ güvenlik grubu oluşturdunuz ve sanal ağ alt ağıyla ilişkilendirdin. Ağ güvenlik grupları hakkında daha fazla bilgi edinmek bkz. [Ağ güvenlik grubuna genel bakış](security-overview.md) ve [Ağ güvenlik grubunu yönetme](manage-network-security-group.md).

Azure, varsayılan olarak trafiği alt ağlar arasında yönlendirir. Bunun yerine, alt ağlar arasındaki trafiği, örneğin, güvenlik duvarı olarak görev yapan bir VM aracılığıyla yönlendirmeyi seçebilirsiniz. Nasıl yapılacağını öğrenmek için [bkz.](tutorial-create-route-table-powershell.md)
