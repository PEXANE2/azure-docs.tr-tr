---
title: 'Hızlı başlangıç: NAT ağ geçidi oluşturma-Azure PowerShell'
titlesuffix: Azure Virtual Network NAT
description: Bu hızlı başlangıçta Azure PowerShell kullanarak bir NAT Gateway oluşturma gösterilmektedir
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
ms.openlocfilehash: 7e8cbadf2c68b97451b40afb876ceb7d88d3758e
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77661065"
---
# <a name="quickstart-create-a-nat-gateway-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell kullanarak NAT ağ geçidi oluşturma

Bu hızlı başlangıçta Azure sanal ağ NAT hizmetini nasıl kullanacağınız gösterilmektedir. Azure 'da bir sanal makine için giden bağlantı sağlamak üzere bir NAT ağ geçidi oluşturacaksınız. 

>[!NOTE] 
>Azure sanal ağ NAT, şu anda genel önizleme olarak kullanılabilir ve sınırlı sayıda [bölgede](https://azure.microsoft.com/global-infrastructure/regions/)kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms).


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bu öğreticiyi Azure Cloud Shell kullanarak tamamlayabilirsiniz veya komutları yerel olarak çalıştırabilirsiniz.  Azure Cloud Shell kullanmadıysanız, [Şimdi oturum açın](https://shell.azure.com).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=latest)ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek **eastus2** konumunda **Myresourcegroupnat** adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
  
New-AzResourceGroup -Name $rsg -Location $loc
```

## <a name="create-the-nat-gateway"></a>NAT ağ geçidini oluşturma

NAT ağ geçidi için genel IP seçenekleri şunlardır:

* **Genel IP adresleri**
* **Genel IP önekleri**

Her ikisi de NAT ağ geçidi ile kullanılabilir.

Göstermek için bu senaryoya genel bir IP adresi ve genel IP öneki ekleyeceğiz.

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

Internet 'e erişmek için NAT ağ geçidi için bir veya daha fazla genel IP adresi gerekir. **Myresourcegroupnat**Içinde **Mypublicıp** adlı bir genel IP adresi kaynağı oluşturmak Için [New-azpublicıpaddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) ' i kullanın. Bu komutun sonucu daha sonra kullanılmak üzere bir değişkende depolanacak **$publicIP** .

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$pbnm = 'myPublicIP'
  
$publicIP = 
New-AzPublicIpAddress -Name $pbnm -ResourceGroupName $rsg -AllocationMethod Static -Location $loc -Sku $sku
```

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

**Myresourcegroupnat**Içinde **Mypublicipprefix** adlı bir genel IP ön eki kaynağı oluşturmak Için [New-azpublicipprefix](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipprefix?view=latest) komutunu kullanın.  Bu komutun sonucu, daha sonra kullanılmak üzere **$publicIPPrefix** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$pxnm = 'myPublicIPprefix'

$publicIPPrefix = 
New-AzPublicIpPrefix -Name $pxnm -ResourceGroupName $rsg -Location $loc -PrefixLength 31
```

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini oluşturma ve yapılandırma işlemlerinin ayrıntıları verilmiştir:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel IP havuzu ve genel IP öneki.
  - Boşta kalma zaman aşımını varsayılan olarak 4 dakika ile 10 dakika arasında değiştirin.

[New-AzNatGateway](https://docs.microsoft.com/powershell/module/az.network/new-aznatgateway)ile genel BIR Azure NAT ağ geçidi oluşturun. Bu komutun sonucu **Mypublicıp** genel IP adresini ve **Mypublicipprefix**genel IP önekini kullanan **mynatgateway** adlı bir ağ geçidi kaynağı oluşturur. Boşta kalma zaman aşımı 10 dakikaya ayarlanır.  Bu komutun sonucu, daha sonra kullanılmak üzere **$natGateway** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$sku = 'Standard'
$gnm = 'myNATgateway'

$natGateway = 
New-AzNatGateway -Name $gnm -ResourceGroupName $rsg -PublicIpAddress $publicIP -PublicIpPrefix $publicIPPrefix -Location $loc -Sku $sku -IdleTimeoutInMinutes 10
  ```

Bu noktada, NAT ağ geçidi çalışır ve yok, bir sanal ağın hangi alt ağlarının bunu kullanması gerektiğini yapılandırmaktır.

## <a name="configure-virtual-network"></a>Sanal ağ yapılandırma

Sanal ağı oluşturun ve alt ağı ağ geçidiyle ilişkilendirin.

Myresourcegroup **adlı bir** alt ağ Ile **myvnet** adlı bir alt ağ ile **myresourcegroup** [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest) [kullanan bir](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) sanal ağ oluşturun. Sanal ağın IP adresi alanı **192.168.0.0/16**' dır. Sanal ağ içindeki alt ağ **192.168.0.0/24**' dir.  Komutların sonucu, daha sonra kullanılmak üzere **$subnet** ve **$VNET** adlı değişkenlere depolanır.

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

Internet hedeflerine giden tüm trafik artık NAT hizmetini kullanıyor.  UDR 'yi yapılandırmak gerekli değildir.

## <a name="create-a-vm-to-use-the-nat-service"></a>NAT hizmetini kullanmak için bir VM oluşturma

Artık NAT hizmetini kullanmak için bir VM oluşturacağız.  Bu VM 'nin VM 'ye erişiminizi sağlamak için örnek düzeyi genel IP olarak kullanılacak genel bir IP 'si vardır.  NAT hizmeti akış yönü farkındadır ve alt ağınızdaki varsayılan Internet hedefini değiştirecek. VM 'nin genel IP adresi giden bağlantılar için kullanılmayacak.

### <a name="create-public-ip-for-source-vm"></a>Kaynak VM için genel IP oluşturma

SANAL makineye erişmek için kullanılacak bir genel IP oluşturacağız.  **Myresourcegroupnat**Içinde **Mypublicipvm** adlı bir genel IP adresi kaynağı oluşturmak Için [New-azpublicıpaddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=latest) komutunu kullanın.  Bu komutun sonucu, daha sonra kullanılmak üzere **$publicIpVM** adlı bir değişkende depolanır.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'
$ipnm = 'myPublicIPVM'
$sku = 'Standard'

$publicIpVM = 
New-AzPublicIpAddress -Name $ipnm -ResourceGroupName $rsg -AllocationMethod Static -Location $loc -Sku $sku
```

### <a name="create-an-nsg-and-expose-ssh-endpoint-for-vm"></a>NSG oluşturma ve sanal makine için SSH uç noktası gösterme

Standart genel IP adresleri ' varsayılan olarak güvenlidir ', SSH için gelen erişime izin vermek üzere bir NSG oluşturmanız gerekir. **Mynsg**adlı bir NSG kaynağı oluşturmak için [New-AzNetworkSecurityGroup](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecuritygroup?view=latest) komutunu kullanın. [New-AzNetworkSecurityRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-aznetworksecurityruleconfig?view=latest) kullanarak, **Myresourcegroupnat**içinde **SSH** adlı SSH erişimi için bir NSG kuralı oluşturun.  Bu komutun sonucu, daha sonra kullanılmak üzere **$NSG** adlı bir değişkende depolanır.

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

**MYNIC**adlı [New-aznetworkınterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface?view=azps-2.8.0) ile bir ağ arabirimi oluşturun. Bu komut genel IP adresini ve ağ güvenlik grubunu ilişkilendirir. Bu komutun sonucu, daha sonra kullanılmak üzere **$Nic** adlı bir değişkende depolanır.

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

SSH anahtar çifti oluşturmak için SSH-keygen kullanın.

```azurepowershell-interactive
ssh-keygen -t rsa -b 2048
```
PuTTy kullanımı dahil olmak üzere SSH anahtar çiftlerinin oluşturulması konusunda daha ayrıntılı bilgi edinmek için bkz. [Windows ile SSH anahtarları kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows).

Cloud Shell kullanarak SSH anahtar çiftini oluşturursanız, anahtar çifti kapsayıcı görüntüde depolanır. Bu [depolama hesabı otomatik olarak oluşturulur](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage). Anahtarlarınızı aldıktan sonra depolama hesabını veya içindeki dosya paylaşımından silme.

#### <a name="create-vm-configuration"></a>VM yapılandırması oluştur

PowerShell 'de bir VM oluşturmak için, görüntü için ayarları, boyutu ve kimlik doğrulama seçeneklerini kullanacak bir yapılandırma oluşturursunuz. Yapılandırma, VM 'yi oluşturmak için kullanılır.

SSH kimlik bilgilerini, işletim sistemi bilgilerini ve VM boyutunu tanımlayın. Bu örnekte, SSH anahtarı ~/. ssh/id_rsa. pub ' da depolanır.

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
**Myresourcegroupnat**içinde [New-Azvm](/powershell/module/az.compute/new-azvm?view=azps-2.8.0) ile **myvm** adlı bir VM oluşturmak için yapılandırma tanımlarını birleştirin.

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$loc = 'eastus2'

New-AzVM -ResourceGroupName $rsg -Location $loc -VM $vmconfig
```

VM 'nin dağıtıma hazırlanması için bekleyin, ardından kalan adımlara devam edin.

## <a name="discover-the-ip-address-of-the-vm"></a>VM 'nin IP adresini bulma

İlk olarak, oluşturduğunuz VM 'nin IP adresini bulduk. VM 'nin genel IP adresini almak için [Get-Azpublicıpaddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=latest)komutunu kullanın. 

```azurepowershell-interactive
$rsg = 'myResourceGroupNAT'
$nmn = 'myPublicIPVM'

Get-AzPublicIpAddress -ResourceGroupName $rsg -Name $nmn | select IpAddress
``` 

>[!IMPORTANT]
>Genel IP adresini kopyalayın ve ardından sanal makineye erişmek için kullanabilmek üzere bir not defteri 'ne yapıştırın.

### <a name="sign-in-to-vm"></a>VM 'de oturum açma

SSH kimlik bilgileri, önceki işlemden Cloud Shell depolanmalıdır.  Tarayıcınızda bir [Azure Cloud Shell](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın.

```bash
ssh azureuser@<ip-address-destination>
```

Artık NAT hizmetini kullanmaya hazırsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=latest) komutunu kullanarak kaynak grubunu ve içinde yer alan tüm kaynakları kaldırabilirsiniz.

```azurepowershell-interactive 
Remove-AzResourceGroup -Name myResourceGroupNAT
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir NAT ağ geçidi ve onu kullanmak için bir VM oluşturdunuz. 

NAT hizmeti 'nin çalışıyor olduğunu görmek için Azure Izleyici 'de ölçümleri gözden geçirin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanılayın.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi eklenerek adreslenir.


- [Azure sanal ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI kullanarak NAT Gateway kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure PowerShell kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure Portal kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmaya yönelik hızlı başlangıç.
- [Genel önizleme hakkında geri bildirim sağlayın](https://aka.ms/natfeedback).
> [!div class="nextstepaction"]


