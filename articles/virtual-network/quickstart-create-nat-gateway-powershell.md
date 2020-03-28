---
title: 'Hızlı başlangıç: NAT ağ geçidi oluşturma - Azure PowerShell'
titlesuffix: Azure Virtual Network NAT
description: Bu hızlı başlangıç, Azure PowerShell kullanarak bir NAT ağ geçidinin nasıl oluşturulurolduğunu gösterir
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: 1d5f8d6e0b2499bbecd32e7cb3fda2cd2cad4d19
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79202255"
---
# <a name="quickstart-create-a-nat-gateway-using-azure-powershell"></a>Quickstart: Azure PowerShell'i kullanarak bir NAT ağ geçidi oluşturma

Bu hızlı başlangıç, Azure Sanal Ağ NAT hizmetini nasıl kullanacağınızı gösterir. Azure'daki sanal bir makine için giden bağlantı sağlamak için bir NAT ağ geçidi oluşturursunuz. 


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu öğreticiyi Azure Cloud Shell'i kullanarak tamamlayabilir veya komutları yerel olarak çalıştırabilirsiniz.  Azure Bulut Su Şurası'nı [kullanmadıysanız, şimdi oturum açın.](https://shell.azure.com)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=latest)ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki **örnekeastus2** konumunda **myResourceGroupNAT** adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
  
New-AzResourceGroup -Name $rsg -Location $loc
```

## <a name="create-the-nat-gateway"></a>NAT ağ geçidioluşturma

NAT ağ geçidi için genel IP seçenekleri şunlardır:

* **Genel IP adresleri**
* **Genel IP önekleri**

Her ikisi de NAT ağ geçidi ile kullanılabilir.

Bu senaryoya göstermek için herkese açık bir IP adresi ve genel bir IP öneki ekleriz.

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Internet'e erişmek için NAT ağ geçidi için bir veya daha fazla genel IP adresine ihtiyacınız vardır. **MyResourceGroupNAT'ta** **myPublicIP** adlı genel bir IP adresi kaynağı oluşturmak için [Yeni-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) kullanın. Bu komutun sonucu daha sonra kullanılmak üzere bir değişken **$publicIP** depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$pbnm = 'myPublicIP'
  
$publicIP = 
New-AzPublicIpAddress -Name $pbnm -ResourceGroupName $rsg -AllocationMethod Static -Location $loc -Sku $sku
```

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

**myResourceGroupNAT'ta** **myPublicIPprefix** adlı genel bir IP öneki kaynağı oluşturmak için [Yeni-AzPublicIpPrefix'i](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipprefix?view=latest) kullanın.  Bu komutun sonucu, daha sonra kullanmak üzere **$publicIPPrefix** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$pxnm = 'myPublicIPprefix'

$publicIPPrefix = 
New-AzPublicIpPrefix -Name $pxnm -ResourceGroupName $rsg -Location $loc -PrefixLength 31
```

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz ayrıntılı olarak açıklanmaktadır:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel BIR IP havuzu ve genel IP öneki.
  - Boşta kalma süresini varsayılan 4 dakikadan 10 dakikaya değiştirin.

[New-AzNatGateway](https://docs.microsoft.com/powershell/module/az.network/new-aznatgateway)ile küresel bir Azure NAT ağ geçidi oluşturun. Bu komutun sonucu, **myNATağ geçidi** adlı bir ağ geçidi kaynağı oluşturarak public IP adresi **myPublicIP'i** ve genel IP öneki **myPublicIPprefix'i**kullanır. Boşta zaman dilimi 10 dakika olarak ayarlanır.  Bu komutun sonucu daha sonra kullanmak üzere **$natGateway** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$gnm = 'myNATgateway'

$natGateway = 
New-AzNatGateway -Name $gnm -ResourceGroupName $rsg -PublicIpAddress $publicIP -PublicIpPrefix $publicIPPrefix -Location $loc -Sku $sku -IdleTimeoutInMinutes 10
  ```

Bu noktada, NAT ağ geçidi işlevseldir ve eksik olan tek şey, sanal bir ağın hangi alt ağlarını kullanması gerektiğini yapılandırmaktır.

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

Sanal ağı oluşturun ve alt ağı ağ geçidine ilişkilendirin.

**MyResourceGroup'ta** [New-AzVirtualNetworkConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) kullanarak **mySubnet** adlı bir alt ağı olan **myVnet** adında bir sanal ağ oluşturun. [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest) Sanal ağ için IP adresi alanı **192.168.0.0/16'dır.** Sanal ağ içindeki alt ağ **192.168.0.0/24'dür.**  Komutların sonucu, daha sonra kullanılmak üzere **$subnet** ve **$vnet** adlı değişkenlerde depolanır.

```azurepowershell-interactive
$sbnm = 'mySubnet'
$vnnm = 'myVnet'
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$pfxsub = '192.168.0.0/24'
$pfxvn = '192.168.0.0/16'

$subnet = 
New-AzVirtualNetworkSubnetConfig -Name $sbnm -AddressPrefix $pfxsub -NatGateway $natGateway

$vnet = 
New-AzVirtualNetwork -Name $vnnm -ResourceGroupName $rsg -Location $loc -AddressPrefix $pfxvn -Subnet $subnet
```

İnternet hedeflerine giden tüm giden trafik artık NAT hizmetini kullanıyor.  Bir UDR yapılandırmak için gerekli değildir.

## <a name="create-a-vm-to-use-the-nat-service"></a>NAT hizmetini kullanmak için bir VM oluşturma

Şimdi NAT hizmetini kullanmak için bir VM oluşturacağız.  Bu VM, VM'ye erişmenizi sağlamak için örnek düzeyinde public IP olarak kullanabileceği ortak bir IP'ye sahiptir.  NAT hizmeti akış yönü farkındadır ve alt anızdaki varsayılan Internet hedefinin yerini alır. VM'nin genel IP adresi giden bağlantılar için kullanılmaz.

### <a name="create-public-ip-for-source-vm"></a>Kaynak VM için ortak IP oluşturma

VM'ye erişmek için kullanılacak genel bir IP oluştururuz.  **MyResourceGroupNAT'ta** **myPublicIPVM** adlı genel bir IP adresi kaynağı oluşturmak için [Yeni-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) kullanın.  Bu komutun sonucu, daha sonra kullanılmak üzere **$publicIpVM** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$ipnm = 'myPublicIPVM'
$sku = 'Standard'

$publicIpVM = 
New-AzPublicIpAddress -Name $ipnm -ResourceGroupName $rsg -AllocationMethod Static -Location $loc -Sku $sku
```

### <a name="create-an-nsg-and-expose-ssh-endpoint-for-vm"></a>Bir NSG oluşturun ve VM için SSH uç noktasını ortaya çıkar

Standart genel IP adresleri 'varsayılan olarak güvenli', biz ssh için gelen erişim sağlamak için bir NSG oluşturmanız gerekir. **myNSG**adında bir NSG kaynağı oluşturmak için [Yeni-AzNetworkSecurityGroup'u](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup?view=latest) kullanın. **MyResourceGroupNAT'ta** **ssh** adı verilen SSH erişimi için Bir NSG kuralı oluşturmak için [Yeni-AzNetworkSecurityRuleConfig'i](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest) kullanın.  Bu komutun sonucu, daha sonra kullanmak üzere **$nsg** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rnm = 'ssh'
$rdesc = 'SSH access'
$acc = 'Allow'
$pro = 'Tcp'
$dir = 'Inbound'
$pri = '100'
$prt = '22'
$rsg = 'myResourceGroupNAT'
$rnm = 'myNSG'
$loc = 'eastus2'

$sshrule = 
New-AzNetworkSecurityRuleConfig -Name $rnm -Description $rdesc -Access $acc -Protocol $pro -Direction $dir -Priority $pri -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange $prt

$nsg = 
New-AzNetworkSecurityGroup -ResourceGroupName $rsg -Name $rnm -Location $loc -SecurityRules $sshrule 
```

### <a name="create-nic-for-vm"></a>VM için NIC oluşturma

**MyNic**adlı [Yeni-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface?view=azps-2.8.0) ile bir ağ arabirimi oluşturun. Bu komut, Ortak IP adresini ve ağ güvenlik grubunu ilişkilendirer. Bu komutun sonucu, daha sonra kullanılmak üzere **$nic** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$nmn = 'myNic'
$loc = 'eastus2'

$nic = 
New-AzNetworkInterface -ResourceGroupName $rsg -Name $nmn -NetworkSecurityGroupID $nsg.Id -PublicIPAddressID $publicIPVM.Id -SubnetID $vnet.Subnets[0].Id -Location $loc
```

### <a name="create-vm"></a>VM oluşturma

#### <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

SSH anahtar çifti oluşturmak için ssh-keygen kullanın.

```azurepowershell-interactive
ssh-keygen -t rsa -b 2048
```
PuTTy kullanımı dahil olmak üzere SSH anahtar çiftlerinin oluşturulması konusunda daha ayrıntılı bilgi edinmek için bkz. [Windows ile SSH anahtarları kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

Bulut Kabuğu'nu kullanarak SSH anahtar çiftini oluşturursanız, anahtar çifti bir kapsayıcı görüntüsünde depolanır. Bu [depolama hesabı otomatik olarak oluşturulur.](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage) Anahtarlarınızı alana kadar depolama hesabını veya içindeki dosya paylaşımını silmeyin.

#### <a name="create-vm-configuration"></a>VM Yapılandırması Oluşturma

PowerShell'de VM oluşturmak için, görüntünün kullanımı, boyutu ve kimlik doğrulama seçenekleri için ayarlara sahip bir yapılandırma oluşturursunuz. Yapılandırma VM oluşturmak için kullanılır.

SSH kimlik bilgilerini, işletim sistemi bilgilerini ve VM boyutunu tanımlayın. Bu örnekte, SSH anahtarı ~/.ssh/id_rsa.pub'da depolanır.

```azurepowershell-interactive
#Define a credential object

$securePassword = 
ConvertTo-SecureString ' ' -AsPlainText -Force

$cred = 
New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration

$vnm = 'myVM'
$vsz = 'Standard_D1'
$pub = 'Canonical'
$off = 'UbuntuServer'
$sku = '18.04-LTS'
$ver = 'latest'

$vmConfig = 
New-AzVMConfig -VMName $vnm -VMSize $vsz

Set-AzVMOperatingSystem -VM $vmConfig -Linux -ComputerName $vnm -Credential $cred -DisablePasswordAuthentication

Set-AzVMSourceImage -VM $vmConfig -PublisherName $pub -Offer $off -Skus $sku -Version $ver

Add-AzVMNetworkInterface -VM $vmConfig -Id $nic.Id

# Configure the SSH key

$sshPublicKey = cat ~/.ssh/id_rsa.pub

Add-AzVMSshPublicKey -VM $vmconfig -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

```
**MyResourceGroupNAT'ta** **MyVM adlı myVM** ile [Yeni-AzVM](/powershell/module/az.compute/new-azvm?view=azps-2.8.0) adında bir VM oluşturmak için yapılandırma tanımlarını birleştirin.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzVM -ResourceGroupName $rsg -Location $loc -VM $vmconfig
```

VM'nin dağıtıma hazırlanmasını bekleyin ve diğer adımlarla devam edin.

## <a name="discover-the-ip-address-of-the-vm"></a>VM'nin IP adresini keşfedin

Öncelikle oluşturduğunuz VM'nin IP adresini keşfetmemiz gerekir. VM'nin genel IP adresini almak için [Get-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=latest)kullanın. 

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$nmn = 'myPublicIPVM'

Get-AzPublicIpAddress -ResourceGroupName $rsg -Name $nmn | select IpAddress
``` 

>[!IMPORTANT]
>Ortak IP adresini kopyalayın ve VM'ye erişmek için kullanabilmeniz için not defterine yapıştırın.

### <a name="sign-in-to-vm"></a>VM'de oturum açın

SSH kimlik bilgileri, önceki işlemdeki Bulut Kabuğunuzda depolanmalıdır.  Tarayıcınızda bir [Azure Bulut Kabuğu](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın.

```bash
ssh azureuser@<ip-address-destination>
```

Artık NAT hizmetini kullanmaya hazırsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) komutunu kullanabilirsiniz.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupNAT
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kullanmak için bir NAT ağ geçidi ve bir VM oluşturdunuz. 

NAT hizmetinizin çalıştığını görmek için Azure Monitor'da ölçümleri inceleyin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanı.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi de eklenerek giderilir.


- Azure [Sanal Ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI'yi kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmak için hızlı başlatın.
- [Azure PowerShell'i kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmak için hızlı başlatın.
- [Azure portalını kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmak için hızlı başlatın.
> [!div class="nextstepaction"]


