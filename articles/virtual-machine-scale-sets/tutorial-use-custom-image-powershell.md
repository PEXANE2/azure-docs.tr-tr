---
title: Öğretici-Azure PowerShell bir ölçek kümesinde özel bir VM görüntüsü kullanma
description: Azure PowerShell kullanarak, sanal makine ölçek kümesini dağıtmak için kullanabileceğiniz bir özel sanal makine görüntüsünün nasıl oluşturulacağını öğrenin
author: cynthn
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: tutorial
ms.date: 03/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: daef03b411a451fc3e5b73e46091672810b0f9bd
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76278288"
---
# <a name="tutorial-create-and-use-a-custom-image-for-virtual-machine-scale-sets-with-azure-powershell"></a>Öğretici: Azure PowerShell ile sanal makine ölçek kümeleri için özel görüntü oluşturma ve kullanma

Ölçek kümesi oluşturduğunuzda, sanal makine örnekleri dağıtılırken kullanılacak bir görüntü belirtirsiniz. Sanal makine örnekleri dağıtıldıktan sonraki görev sayısını azaltmak için özel bir sanal makine görüntüsünü kullanabilirsiniz. Bu özel sanal makine görüntüsü, gerekli uygulama yüklemelerini veya yapılandırmalarını içerir. Ölçek kümesinde oluşturulan tüm sanal makine örnekleri, özel sanal makine görüntüsünü kullanır ve uygulama trafiğinizi sunmaya hazır olur. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Sanal makine oluşturma ve özelleştirme
> * Sanal makinenin sağlamasını kaldırma ve sanal makineyi genelleştirme
> * Kaynak sanal makineden özel bir sanal makine görüntüsü oluşturma
> * Özel sanal makine görüntüsünü kullanan bir ölçek kümesini dağıtma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


## <a name="create-and-configure-a-source-vm"></a>Kaynak sanal makine oluşturma ve yapılandırma

>[!NOTE]
> Bu öğreticide, genelleştirilmiş bir sanal makine görüntüsü oluşturma ve kullanma işlemi gösterilmektedir. Özelleştirilmiş VHD dosyasından ölçek kümesi oluşturulması desteklenmez.

İlk olarak, [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)ile bir kaynak grubu oluşturun ve ardından [New-AZVM](/powershell/module/az.compute/new-azvm)ile bir VM oluşturun. Bu sanal makine daha sonra özel bir sanal makine görüntüsü için kaynak olarak kullanılır. Aşağıdaki örnek, *myResourceGroup* adlı kaynak grubunda *myCustomVM* adlı bir sanal makine oluşturur. İstendiğinde, sanal makine için oturum açma kimlik bilgileri olarak kullanılacak bir kullanıcı adı ve parola girin:

```azurepowershell-interactive
# Create a resource a group
New-AzResourceGroup -Name "myResourceGroup" -Location "EastUS"

# Create a Windows Server 2016 Datacenter VM
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Name "myCustomVM" `
  -ImageName "Win2016Datacenter" `
  -OpenPorts 3389
```

VM 'nize bağlanmak için [Get-Azpublicıpaddress](/powershell/module/az.network/get-azpublicipaddress) Ile genel IP adresini şu şekilde listeleyin:

```azurepowershell-interactive
Get-AzPublicIpAddress -ResourceGroupName myResourceGroup | Select IpAddress
```

Sanal makineyle uzaktan bağlantı oluşturun. Azure Cloud Shell kullanıyorsanız, bu adımı yerel PowerShell isteminden veya Uzak Masaüstü İstemcisinden gerçekleştirin. Önceki komuttan kendi IP adresinizi sağlayın. İstendiğinde, ilk adımda sanal makineyi oluştururken kullandığınız kimlik bilgilerini girin:

```powershell
mstsc /v:<IpAddress>
```

Şimdi sanal makinenizi özelleştirmek için bir temel web sunucusu yükleyelim. Ölçek kümesindeki sanal makine örneği dağıtılacağı zaman, bir web uygulamasını çalıştırmak için gerekli tüm paketleri içerir. Sanal makinede yerel bir PowerShell istemini açın ve aşağıdaki adımları uygulayarak [Install-WindowsFeature](/powershell/module/servermanager/install-windowsfeature) ile IIS web sunucusunu yükleyin:

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

Sanal makinenizi özel görüntü olarak kullanılmaya hazırlamanın son adımı, sanal makinenizin genelleştirilmesidir. Sysprep tüm kişisel hesap bilgilerinizi ve yapılandırmalarınızı kaldırır ve sanal makineyi gelecekteki dağıtımlar için sıfırlayıp temiz bir duruma getirir. Daha fazla bilgi için bkz. [Sysprep İşlemini Kullanma: Giriş](https://technet.microsoft.com/library/bb457073.aspx).

Sanal makineyi genelleştirmek için Sysprep işlemini çalıştırıp sanal makineyi ayarlayarak hızlı bir deneyim elde edin. İşlem tamamlandığında, Sysprep işlemine sanal makineyi kapatmasını bildirin:

```powershell
C:\Windows\system32\sysprep\sysprep.exe /oobe /generalize /shutdown
```

Sysprep, işlemi tamamladığında ve sanal makine kapatıldığında sanal makineyle uzaktan bağlantı otomatik olarak kapanır.


## <a name="create-a-custom-vm-image-from-the-source-vm"></a>Kaynak sanal makineden özel bir sanal makine görüntüsü oluşturma
Kaynak sanal makine şimdi IIS web sunucusunun yüklenmesiyle birlikte özelleştirilir. Şimdi ölçek kümesi ile kullanılacak özel sanal makine görüntüsü oluşturalım.

Bir görüntü oluşturmak için VM’nin serbest bırakılması gerekir. [Stop-AzVm](/powershell/module/az.compute/stop-azvm)ile VM 'yi serbest bırakın. Daha sonra, Azure platformunun sanal makinenin özel bir görüntü kullanmaya hazırsa bilmesini sağlamak için VM 'nin durumunu [set-AzVm](/powershell/module/az.compute/set-azvm) ile Genelleştirilmiş olarak ayarlayın. Yalnızca genelleştirilmiş bir sanal makineden görüntü oluşturabilirsiniz:

```azurepowershell-interactive
Stop-AzVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Force
Set-AzVM -ResourceGroupName "myResourceGroup" -Name "myCustomVM" -Generalized
```

Sanal makinenin serbest bırakılıp genelleştirilmesi birkaç dakika sürebilir.

Şimdi [New-Azımageconfig](/powershell/module/az.compute/new-azimageconfig) ve [New-AZıMAGE](/powershell/module/az.compute/new-azimage)ile VM 'nin bir görüntüsünü oluşturun. Aşağıdaki örnek, sanal makinenizden *myImage* adlı bir görüntü oluşturur:

```azurepowershell-interactive
# Get VM object
$vm = Get-AzVM -Name "myCustomVM" -ResourceGroupName "myResourceGroup"

# Create the VM image configuration based on the source VM
$image = New-AzImageConfig -Location "EastUS" -SourceVirtualMachineId $vm.ID 

# Create the custom VM image
New-AzImage -Image $image -ImageName "myImage" -ResourceGroupName "myResourceGroup"
```

## <a name="configure-the-network-security-group-rules"></a>Ağ güvenlik grubu kurallarını yapılandırma
Ölçek kümesini oluşturmadan önce, ağ güvenlik grubu kurallarını HTTP, RDP ve Uzaktan erişime izin verecek şekilde yapılandırmamız gerekir 

```azurepowershell-interactive
$rule1 = New-AzNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 80

$rule2 = New-AzNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 3389

$rule3 = New-AzNetworkSecurityRuleConfig -Name remoting-rule -Description "Allow PS Remoting" -Access Allow -Protocol Tcp -Direction Inbound -Priority 120 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 5985

New-AzNetworkSecurityGroup -Name "myNSG" -ResourceGroupName "myResourceGroup" -Location "EastUS" -SecurityRules $rule1,$rule2,$rule3
```

## <a name="create-a-scale-set-from-the-custom-vm-image"></a>Özel bir sanal makine görüntüsünden ölçek kümesi oluşturma
Şimdi, önceki adımda oluşturulan özel VM görüntüsünü tanımlamak için `-ImageName` parametresini kullanan [New-azvmss](/powershell/module/az.compute/new-azvmss) ile bir ölçek kümesi oluşturun. Her bir sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Yük dengeleyici hem 80 numaralı TCP bağlantı noktasında trafiği dağıtmak hem de 3389 numaralı TCP bağlantı noktasında uzak masaüstü trafiğine ve 5985 numaralı TCP bağlantı noktasında PowerShell uzaktan iletişimine olanak tanımak için kurallar içerir. İstendiğinde, ölçek kümesindeki sanal makine örnekleri için kendi istediğiniz yönetici kimlik bilgilerini sağlayın:

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Location "EastUS" `
  -VMScaleSetName "myScaleSet" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -SecurityGroupName "myNSG"
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic" `
  -ImageName "myImage"
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.


## <a name="test-your-scale-set"></a>Ölçek kümenizi test etme
Ölçek kümesini eylem bölümünde görmek için [Get-Azpublicıpaddress](/powershell/module/az.network/Get-AzPublicIpAddress) ile yük dengeleyicinizin genel IP adresini aşağıdaki şekilde alın:


```azurepowershell-interactive
Get-AzPublicIpAddress `
  -ResourceGroupName "myResourceGroup" `
  -Name "myPublicIPAddress" | Select IpAddress
```

Genel IP adresini bir web tarayıcınıza yazın. Aşağıdaki örnekte gösterildiği gibi varsayılan IIS web sayfası görüntülenir:

![Özel sanal makine görüntüsünden çalıştırılan IIS](media/tutorial-use-custom-image-powershell/default-iis-website.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Ölçek kümesini ve ek kaynaklarınızı kaldırmak için, [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)komutunu kullanarak kaynak grubunu ve tüm kaynaklarını silin. `-Force` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar. `-AsJob` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür.

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure PowerShell ile ölçek kümeleriniz için özel sanal makine görüntüsü oluşturma ve kullanma işleminin nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Sanal makine oluşturma ve özelleştirme
> * Sanal makinenin sağlamasını kaldırma ve sanal makineyi genelleştirme
> * Özel bir sanal makine görüntüsü oluşturma
> * Özel sanal makine görüntüsünü kullanan bir ölçek kümesini dağıtma

Uygulamaların ölçek kümenize nasıl dağıtılacağını öğrenmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Ölçek kümelerinize uygulama dağıtma](tutorial-install-apps-powershell.md)
