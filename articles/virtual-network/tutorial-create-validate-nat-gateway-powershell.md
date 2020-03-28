---
title: 'Öğretici: Bir NAT ağ geçidi oluşturma ve test edin - Azure PowerShell'
titlesuffix: Azure Virtual Network NAT
description: Bu öğretici, Azure PowerShell'i kullanarak bir NAT ağ geçidioluşturmayı ve NAT hizmetini nasıl test ediletini gösterir
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to test a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2020
ms.author: allensu
ms.openlocfilehash: 61cda5e61d14c4eeaf2d88483603707598b1c911
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79202251"
---
# <a name="tutorial-create-a-nat-gateway-using-azure-powershell-and-test-the-nat-service"></a>Öğretici: Azure PowerShell'i kullanarak bir NAT ağ geçidi oluşturun ve NAT hizmetini test edin

Bu eğitimde, Azure'daki sanal makineler için giden bağlantı sağlamak için bir NAT ağ geçidi oluşturursunuz. NAT ağ geçidini sınamak için bir kaynak ve hedef sanal makine dağıtabilirsiniz. Genel bir IP adresine giden bağlantılar oluşturarak NAT ağ geçidini sınayacaksınız. Bu bağlantılar kaynaktan hedef sanal makineye gelecektir. Bu öğretici, basitlik için aynı kaynak grubunda ki iki farklı sanal ağda kaynak ve hedef dağıtır.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu öğreticiyi Azure Cloud Shell'i kullanarak tamamlayabilir veya ilgili komutları yerel olarak çalıştırabilirsiniz.  Azure Bulut Su Şurası'nı hiç kullanmadıysanız, [şimdi oturum açmanız](https://shell.azure.com)gerekir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](https://docs.microsoft.com/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki **örnekeastus2** konumunda **myResourceGroupNAT** adlı bir kaynak grubu oluşturur:


```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzResourceGroup -Name $rsg -Location $loc
```

## <a name="create-the-nat-gateway"></a>NAT ağ geçidioluşturma

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Internet'e erişmek için NAT ağ geçidi için bir veya daha fazla genel IP adresine ihtiyacınız vardır. **MyResourceGroupNAT'ta** **myPublicIPsource** adlı genel bir IP adresi kaynağı oluşturmak için [Yeni-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) kullanın. Bu komutun sonucu, daha sonra kullanmak üzere **$publicIPsource** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$pips = 'myPublicIPsource'
$alm = 'Static'
$sku = 'Standard'

$publicIPsource = 
New-AzPublicIpAddress -Name $pips -ResourceGroupName $rsg -AllocationMethod $alm -Location $loc -Sku $sku
```

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

 **myResourceGroupNAT'ta** **myPublicIPprefixsource** adlı genel bir IP öneki kaynağı oluşturmak için [Yeni-AzPublicIpPrefix'i](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipprefix?view=latest) kullanın.  Bu komutun sonucu, daha sonra kullanılmak üzere **$publicIPPrefixsource** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$prips = 'mypublicIPprefixsource'

$publicIPPrefixsource = 
New-AzPublicIpPrefix -Name $prips -ResourceGroupName $rsg -Location $loc -PrefixLength 31
```

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz ayrıntılı olarak açıklanmaktadır:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel BIR IP havuzu ve genel IP öneki.
  - Boşta kalma süresini varsayılan 4 dakikadan 10 dakikaya değiştirin.

[New-AzNatGateway](https://docs.microsoft.com/powershell/module/az.network/new-aznatgateway)ile küresel bir Azure NAT ağ geçidi oluşturun. Bu komutun sonucu, public IP adresi **myPublicIPsource** ve ortak IP öneki **myPublicIPprefixsource**kullanan **myNATgateway** adlı bir ağ geçidi kaynağı oluşturacaktır. Boşta zaman dilimi 10 dakika olarak ayarlanır.  Bu komutun sonucu daha sonra kullanmak üzere **$natGateway** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$nnm = 'myNATgateway'

$natGateway = 
New-AzNatGateway -Name $nnm -ResourceGroupName $rsg -PublicIpAddress $publicIPsource -PublicIpPrefix $publicIPPrefixsource -Location $loc -Sku $sku -IdleTimeoutInMinutes 10      
  ```

Bu noktada, NAT ağ geçidi işlevseldir ve eksik olan tek şey, sanal bir ağın hangi alt ağlarını kullanması gerektiğini yapılandırmaktır.

## <a name="prepare-the-source-for-outbound-traffic"></a>Kaynağı giden trafik için hazırlayın

Tam bir test ortamı nın kurulumu nda size rehberlik edeceğiz. NAT ağ geçidini doğrulamak için açık kaynak araçlarını kullanarak bir test ayarlarsınız. Daha önce oluşturduğumuz NAT ağ geçidini kullanacak kaynakla başlayacağız.

### <a name="configure-virtual-network-for-source"></a>Kaynak için sanal ağı yapılandırma

Sanal ağı oluşturun ve alt ağı ağ geçidine ilişkilendirin.

**MyResourceGroupNAT'ta** [New-AzVirtualNetwork'ü](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest)kullanarak **MySubnetsource** adlı bir alt ağ ile **myVnetsource** adında bir sanal ağ oluşturun. [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) Sanal ağ için IP adresi alanı **192.168.0.0/16'dır.** Sanal ağ içindeki alt ağ **192.168.0.0/24'dür.**  Komutların sonucu, daha sonra kullanılmak üzere **$subnetsource** ve **$vnetsource** adlı değişkenlerde depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$sbn = 'mySubnetsource'
$spfx = '192.168.0.0/24'
$vnm = 'myVnetsource'
$vpfx = '192.168.0.0/16'
$loc = 'eastus2'

$subnetsource = 
New-AzVirtualNetworkSubnetConfig -Name $sbn -AddressPrefix $spfx -NatGateway $natGateway

$vnetsource = 
New-AzVirtualNetwork -Name $vnm -ResourceGroupName $rsg -Location $loc -AddressPrefix $vpfx -Subnet $subnet
```

İnternet hedeflerine giden tüm giden trafik artık NAT hizmetini kullanıyor.  Bir UDR yapılandırmak için gerekli değildir.

NAT ağ geçidini test etmeden önce bir kaynak VM oluşturmamız gerekiyor.  Bu VM'ye dışarıdan erişmek için örnek düzeyinde bir Genel IP kaynağı olarak genel bir IP adresi kaynağı atayacağız. Bu adres yalnızca test için erişmek için kullanılır.  NAT hizmetinin diğer giden seçeneklerden nasıl öncelikli olduğunu göstereceğiz.

Ayrıca, bu VM'yi genel bir IP olmadan oluşturabilir ve bir egzersiz olarak genel IP'si olmadan atlama kutusu olarak kullanmak üzere başka bir VM oluşturabilirsiniz.

### <a name="create-public-ip-for-source-vm"></a>Kaynak VM için ortak IP oluşturma

VM'ye erişmek için kullanılacak genel bir IP oluştururuz.  **MyResourceGroupNAT'ta** **myPublicIPVM** adlı genel bir IP adresi kaynağı oluşturmak için [Yeni-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) kullanın.  Bu komutun sonucu, daha sonra kullanmak üzere **$publicIpsourceVM** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$pipvm = 'myPublicIpsourceVM'
$alm = 'Static'

$publicIpsourceVM = 
New-AzPublicIpAddress -Name $pipvm -ResourceGroupName $rsg -AllocationMethod $alm -Location $loc -sku $sku
```

### <a name="create-an-nsg-and-expose-ssh-endpoint-for-vm"></a>Bir NSG oluşturun ve VM için SSH uç noktasını ortaya çıkar

Standart Genel IP adresleri 'varsayılan olarak güvenli' olduğundan, ssh için gelen erişime izin vermek için bir NSG oluştururuz. NAT hizmeti akış yönü farkındadır. NAT ağ geçidi aynı alt ağ üzerinde yapılandırıldığında bu NSG giden için kullanılmaz. **myNSGsource**adlı bir NSG kaynağı oluşturmak için [Yeni-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup?view=latest) kullanın. **MyResourceGroupNAT'ta** **ssh** adı verilen SSH erişimi için Bir NSG kuralı oluşturmak için [Yeni-AzNetworkSecurityRuleConfig'i](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest) kullanın. Bu komutun sonucu daha sonra kullanmak üzere **$nsgsource** adlı değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$rnm = 'ssh'
$rdsc = 'SSH access'
$acc = 'Allow'
$prt = 'Tcp'
$dir = 'Inbound'
$nsnm = 'myNSGsource'

$sshrule = 
New-AzNetworkSecurityRuleConfig -Name $rnm -Description $rdsc -Access $acc -Protocol $prt -Direction $dir -Priority 100 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22

$nsgsource = 
New-AzNetworkSecurityGroup -ResourceGroupName $rsg -Name $nsnm -Location $loc -SecurityRules $sshrule 
```

### <a name="create-nic-for-source-vm"></a>Kaynak VM için NIC oluşturma

**MyNicsource**adlı [Yeni-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface?view=azps-2.8.0) ile bir ağ arabirimi oluşturun. Bu komut, Ortak IP adresini ve ağ güvenlik grubunu ilişkilendirecektir. Bu komutun sonucu, daha sonra kullanmak üzere **$nicsource** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$nin = 'myNicsource'

$nicsource = 
New-AzNetworkInterface -ResourceGroupName $rsg -Name $nin -NetworkSecurityGroupID $nsgsource.Id -PublicIPAddressID $publicIPVMsource.Id -SubnetID $vnetsource.Subnets[0].Id -Location $loc
```

### <a name="create-a-source-vm"></a>Kaynak VM oluşturma

#### <a name="create-ssh-key-pair"></a>SSH anahtar çifti oluşturma

Bu hızlı başlangıcı tamamlamak için bir SSH anahtar çifti gerekir. Bir SSH anahtar çiftiniz varsa bu adımı atlayabilirsiniz.

SSH anahtar çifti oluşturmak için ssh-keygen kullanın.

```azurepowershell-interactive
ssh-keygen -t rsa -b 2048
```
PuTTy kullanımı dahil olmak üzere SSH anahtar çiftlerinin oluşturulması konusunda daha ayrıntılı bilgi edinmek için bkz. [Windows ile SSH anahtarları kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

Bulut Kabuğu'nu kullanarak SSH anahtar çiftini oluşturursanız, anahtar çifti bir kapsayıcı görüntüsünde depolanır. Bu [depolama hesabı otomatik olarak oluşturulur.](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage) Anahtarlarınızı alana kadar depolama hesabını veya içindeki dosya paylaşımını silmeyin.

#### <a name="create-vm-configuration"></a>VM Yapılandırması Oluşturma

PowerShell’de bir VM oluşturmak için kullanılacak görüntü, boyut ve kimlik doğrulaması seçenekleri gibi ayarların olduğu bir yapılandırma oluşturursunuz. Ardından bu yapılandırma VM’yi derlemek için kullanılır.

SSH kimlik bilgilerini, işletim sistemi bilgilerini ve VM boyutunu tanımlayın. Bu örnekte, SSH anahtarı ~/.ssh/id_rsa.pub'da depolanır.

```azurepowershell-interactive
# Define a credential object

$securePassword = 
ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = 
New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration
$vmn = 'myVMsource'
$vms = 'Standard_D1'
$pub = 'Canonical'
$off = 'UbuntuServer'
$skus = '18.04-LTS'
$ver = 'latest'

$vmConfigsource = 
New-AzVMConfig -VMName $vmn -VMSize $vms

Set-AzVMOperatingSystem -VM $vmConfigsource -Linux -ComputerName $vmn -Credential $cred -DisablePasswordAuthentication

Set-AzVMSourceImage -VM $vmConfigsource -PublisherName $pub -Offer $off -Skus $skus -Version $ver

Add-AzVMNetworkInterface -VM $vmConfigsource -Id $nicsource.Id

# Configure the SSH key

$sshPublicKey = cat ~/.ssh/id_rsa.pub

Add-AzVMSshPublicKey -VM $vmConfigsource -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

```
**MyResourceGroupNAT'ta** **MyVMsource** adlı bir VM oluşturmak için yapılandırma tanımlarını [birleştirin.](/powershell/module/az.compute/new-azvm?view=azps-2.8.0)

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzVM -ResourceGroupName $rsg -Location $loc -VM $vmConfigsource
```

Komut hemen geri dönecek olsa da, VM'nin devreye salması birkaç dakika sürebilir.

## <a name="prepare-destination-for-outbound-traffic"></a>Giden trafik için hedef hazırlama

Şimdi, NAT hizmeti tarafından çevrilen giden trafik için bir hedef oluşturarak bunu test edebilirsiniz.

### <a name="configure-virtual-network-for-destination"></a>Hedef için sanal ağı yapılandırma

Hedef sanal makinenin olacağı bir sanal ağ oluşturmamız gerekiyor.  Bu komutlar kaynak VM ile aynı adımlardır. Hedef bitiş noktasını ortaya çıkarmak için küçük değişiklikler eklendi.

**MyResourceGroupNAT'ta** [New-AzVirtualNetwork'ü](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest)kullanarak **MySubnetdestination** adlı bir alt ağı olan **myVnetdestination** adında bir sanal ağ oluşturun. [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) Sanal ağ için IP adresi alanı **192.168.0.0/16'dır.** Sanal ağ içindeki alt ağ **192.168.0.0/24'dür.**  Komutların sonucu, daha sonra kullanılmak üzere **$subnetdestination** ve **$vnetdestination** adlı değişkenlerde depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sbdn = 'mySubnetdestination'
$spfx = '192.168.0.0/24'
$vdn = 'myVnetdestination'
$vpfx = '192.168.0.0/16'

$subnetdestination = 
New-AzVirtualNetworkSubnetConfig -Name $sbdn -AddressPrefix $spfx

$vnetdestination = 
New-AzVirtualNetwork -Name $vdn -ResourceGroupName $rsg -Location $loc -AddressPrefix $vpfx -Subnet $subnetdestination
```

### <a name="create-public-ip-for-destination-vm"></a>Hedef VM için genel IP oluşturma

Hedef VM'ye erişmek için kullanılmak üzere genel bir IP oluşturuyoruz.  **MyResourceGroupNAT'ta** **myPublicIPdestinationVM** adlı genel bir IP adresi kaynağı oluşturmak için [Yeni-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) kullanın.  Bu komutun sonucu, daha sonra kullanılmak üzere **$publicIpdestinationVM** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$all = 'Static'
$pipd = 'myPublicIPdestinationVM'

$publicIpdestinationVM = 
New-AzPublicIpAddress -Name $pipd -ResourceGroupName $rsg -AllocationMethod $all -Location $loc -Sku $sku
```

### <a name="create-an-nsg-and-expose-ssh-and-http-endpoint-for-vm"></a>Bir NSG oluşturun ve VM için SSH ve HTTP uç noktasını ortaya çıkar

Standart Genel IP adresleri 'varsayılan olarak güvenli', biz ssh için gelen erişim sağlamak için bir NSG oluşturun. **myNSGdestination**adlı bir NSG kaynağı oluşturmak için [Yeni-AzNetworkSecurityGroup'u](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup?view=latest) kullanın. **SSH**erişimi için bir NSG kuralı oluşturmak için [Yeni-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest) kullanın.  **Http erişim**için http adlı bir NSG kuralı oluşturmak için [Yeni-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest) kullanın. Her iki kural **da myResourceGroupNAT'ta**oluşturulur. Bu komutun sonucu, daha sonra kullanılmak üzere **$nsgdestination** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$snm = 'ssh'
$sdsc = 'SSH access'
$acc = 'Allow'
$prt = 'Tcp'
$dir = 'Inbound'
$hnm = 'http'
$hdsc = 'HTTP access'
$nsnm = 'myNSGdestination'

$sshrule = 
New-AzNetworkSecurityRuleConfig -Name $snm -Description $sdsc -Access $acc -Protocol $prt -Direction $dir -Priority 100 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 22

$httprule = 
New-AzNetworkSecurityRuleConfig -Name $hnm -Description $hdsc -Access $acc -Protocol $prt -Direction $dir -Priority 101 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 80

$nsgdestination = 
New-AzNetworkSecurityGroup -ResourceGroupName $rsg -Name $nsnm -Location $loc -SecurityRules $sshrule,$httprule
```

### <a name="create-nic-for-destination-vm"></a>Hedef VM için NIC oluşturma

**MyNicdestination**adlı [Yeni-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface?view=azps-2.8.0) ile bir ağ arabirimi oluşturun. Bu komut, Ortak IP adresi ve ağ güvenlik grubu yla ilişkilendirecektir. Bu komutun sonucu daha sonra kullanmak üzere **$nicdestination** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$nnm = 'myNicdestination'

$nicdestination = 
New-AzNetworkInterface -ResourceGroupName $rsg -Name $nnm -NetworkSecurityGroupID $nsgdestination.Id -PublicIPAddressID $publicIPdestinationVM.Id -SubnetID $vnetdestination.Subnets[0].Id -Location $loc
```

### <a name="create-a-destination-vm"></a>Hedef VM oluşturma

#### <a name="create-vm-configuration"></a>VM Yapılandırması Oluşturma

PowerShell’de bir VM oluşturmak için kullanılacak görüntü, boyut ve kimlik doğrulaması seçenekleri gibi ayarların olduğu bir yapılandırma oluşturursunuz. Ardından bu yapılandırma VM’yi derlemek için kullanılır.

SSH kimlik bilgilerini, işletim sistemi bilgilerini ve VM boyutunu tanımlayın. Bu örnekte, SSH anahtarı ~/.ssh/id_rsa.pub'da depolanır.

```azurepowershell-interactive
# Define a credential object

$securePassword = 
ConvertTo-SecureString ' ' -AsPlainText -Force
$cred = 
New-Object System.Management.Automation.PSCredential ("azureuser", $securePassword)

# Create a virtual machine configuration

$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$vmd = 'myVMdestination'
$vms = 'Standard_D1'
$pub = 'Canonical'
$off = 'UbuntuServer'
$skus = '18.04-LTS'
$ver = 'latest'

$vmConfigdestination = New-AzVMConfig -VMName $vmd -VMSize $vms

Set-AzVMOperatingSystem -VM $vmConfigdestination -Linux -ComputerName $vmd -Credential $cred -DisablePasswordAuthentication

Set-AzVMSourceImage -VM $vmConfigdestination -PublisherName $pub -Offer $off -Skus $skus -Version $ver

Add-AzVMNetworkInterface -VM $vmConfigdestination -Id $nicdestination.Id

# Configure the SSH key

$sshPublicKey = cat ~/.ssh/id_rsa.pub

Add-AzVMSshPublicKey -VM $vmConfigdestination -KeyData $sshPublicKey -Path "/home/azureuser/.ssh/authorized_keys"

```
**MyResourceGroupNAT'ta** **MyVMdestination** adlı bir VM oluşturmak için yapılandırma tanımlarını [birleştirin.](/powershell/module/az.compute/new-azvm?view=azps-2.8.0)

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzVM -ResourceGroupName $rsg -Location $loc -VM $vmConfigdestination
```

Komut hemen geri dönecek olsa da, VM'nin devreye salması birkaç dakika sürebilir.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Hedef VM'de bir web sunucusu hazırlayın ve yükü test edin

Öncelikle hedef VM IP adresini keşfetmek gerekir.  VM'nin genel IP adresini almak için [Get-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=latest)kullanın. 

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$pipn = 'myPublicIPdestinationVM'
  
Get-AzPublicIpAddress -ResourceGroupName $rsg -Name $pipn | select IpAddress
``` 

>[!IMPORTANT]
>Ortak IP adresini kopyalayın ve sonraki adımlarda kullanabilmeniz için not defterine yapıştırın. Bunun hedef sanal makine olduğunu belirtin.

### <a name="sign-in-to-destination-vm"></a>Hedef VM'de oturum açın

SSH kimlik bilgileri, önceki işlemdeki Bulut Kabuğunuzda depolanmalıdır.  Tarayıcınızda bir [Azure Bulut Kabuğu](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```bash
ssh azureuser@<ip-address-destination>
```

Oturum açtıktan sonra aşağıdaki komutları kopyalayıp yapıştırın.  

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get -y install nginx && \
sudo ln -sf /dev/null /var/log/nginx/access.log && \
sudo touch /var/www/html/index.html && \
sudo rm /var/www/html/index.nginx-debian.html && \
sudo dd if=/dev/zero of=/var/www/html/100k bs=1024 count=100
```

Bu komutlar sanal makinenizi günceller, nginx yükler ve 100 KBytes dosyası oluşturur. Bu dosya NAT hizmeti kullanılarak kaynak VM'den alınacaktır.

Hedef VM ile SSH oturumunu kapatın.

## <a name="prepare-test-on-source-vm"></a>Kaynak VM üzerinde test hazırlama

Öncelikle kaynak VM IP adresini keşfetmek gerekir.  VM'nin genel IP adresini almak için [Get-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=latest)kullanın. 

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$pipn = 'myPublicIPsourceVM'

Get-AzPublicIpAddress -ResourceGroupName $rsg -Name $pipn | select IpAddress
``` 

>[!IMPORTANT]
>Ortak IP adresini kopyalayın ve sonraki adımlarda kullanabilmeniz için not defterine yapıştırın. Bunun kaynak sanal makine olduğunu belirtin.

### <a name="log-into-source-vm"></a>Kaynak VM'de oturum aç

Yine, SSH kimlik bilgileri Bulut Kabuğu'nda depolanır. Tarayıcınızda Azure [Bulut Su şutu](https://shell.azure.com) için yeni bir sekme açın.  Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```bash
ssh azureuser@<ip-address-source>
```

NAT hizmetini sınamaya hazırlanmak için aşağıdaki komutları kopyalayın ve yapıştırın.

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get install -y nload golang && \
echo 'export GOPATH=${HOME}/go' >> .bashrc && \
echo 'export PATH=${PATH}:${GOPATH}/bin' >> .bashrc && \
. ~/.bashrc &&
go get -u github.com/rakyll/hey

```

Bu komutlar sanal makinenizi günceller, git'i yükler, GitHub'dan [yüklenir](https://github.com/rakyll/hey) ve kabuk ortamınızı günceller.

Artık NAT hizmetini test etmeye hazırsınız.

## <a name="validate-nat-service"></a>NAT hizmetini doğrulama

Kaynak VM'de oturum açtığınızda, hedef IP adresine istek oluşturmak için **curl** ve **hey'i** kullanabilirsiniz.

100-KBytes dosyasını almak için kıvırma kullanın.  Aşağıdaki örnekte ** \<ip adresi hedef>** daha önce kopyalamış olduğunuz hedef IP adresiyle değiştirin.  **--çıkış** parametresi, alınan dosyanın atılacağını gösterir.

```bash
curl http://<ip-address-destination>/100k --output /dev/null
```

Ayrıca **hey**kullanarak istekleri bir dizi oluşturabilirsiniz. Yine, ** \<ip adresi hedef>** daha önce kopyalamış olduğunuz hedef IP adresiyle değiştirin.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k
```

Bu komut, 30 saniyelik bir zaman aşımıyla 10 aynı anda 100 istek oluşturur. TCP bağlantısı yeniden kullanılmaz.  Her istek 100 Kbyte alır.  Çalışma sonunda, **hey** NAT hizmeti ne kadar iyi yaptığı hakkında bazı istatistikleri rapor edecektir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) komutunu kullanabilirsiniz.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupNAT
```

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir NAT ağ geçidi oluşturdunuz, bir kaynak ve hedef VM oluşturdunuz ve sonra NAT ağ geçidini test ettiniz.

NAT hizmetinizin çalıştığını görmek için Azure Monitor'da ölçümleri inceleyin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanı.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi de eklenerek kolayca giderilir.

- Sanal [Ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI'yi kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmak için hızlı başlatın.
- [Azure PowerShell'i kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmak için hızlı başlatın.
- [Azure portalını kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmak için hızlı başlatın.

> [!div class="nextstepaction"]

