---
title: Öğretici - Uygulamaları Azure PowerShell ile bir ölçek kümesine yükleme
description: Azure PowerShell kullanarak Özel Betik Uzantısı ile sanal makine ölçek kümelerine uygulama yükleme işleminin nasıl yapılacağını öğrenin
author: cynthn
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.topic: tutorial
ms.date: 11/08/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 5e1b21b1d00defdb090a35c067fa533a482c828d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76271510"
---
# <a name="tutorial-install-applications-in-virtual-machine-scale-sets-with-azure-powershell"></a>Öğretici: Azure PowerShell ile sanal makine ölçek kümelerine uygulama yükleme

Bir ölçek kümesindeki sanal makine (VM) örneklerinde uygulamaları çalıştırmak için önce uygulama bileşenlerini ve gerekli dosyaları yüklemeniz gerekir. Önceki bir öğreticide, sanal makine örneklerinizi dağıtmak için nasıl özel sanal makine görüntüsü oluşturulacağını ve kullanılacağını öğrendiniz. Bu özel görüntüde, el ile uygulama yüklemeleri ve yapılandırmaları yer alıyordu. Her sanal makine örneği dağıtıldıktan sonra bir ölçek kümesine uygulamaların yüklenmesini otomatikleştirebilir veya önceden ölçek kümesinde çalıştırılan bir uygulamayı güncelleştirebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Ölçek kümenize otomatik olarak uygulama yükleme
> * Azure Özel Betik Uzantısı’nı kullanma
> * Ölçek kümesinde çalıştırılan bir uygulamayı güncelleştirme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


## <a name="what-is-the-azure-custom-script-extension"></a>Azure Özel Betik Uzantısı nedir?
Özel Betik Uzantısı, Azure VM’lerinde betik indirir ve yürütür. Bu uzantı dağıtım sonrası yapılandırma, yazılım yükleme veya diğer yapılandırma/yönetim görevleri için kullanışlıdır. Betikler Azure depolama veya GitHub konumlarından indirilebilir ya da Azure portalına uzantı çalışma zamanında iletilebilir.

Özel Komut Dosyası uzantısı, Azure Kaynak Yöneticisi şablonlarıyla tümleşir. Azure CLI, Azure PowerShell, Azure portalı veya REST API ile de kullanılabilir. Daha fazla bilgi için bkz. [Özel Betik Uzantısı'na genel bakış](../virtual-machines/windows/extensions-customscript.md).

Özel Betik Uzantısı’nı çalışır halde görmek için, IIS web sunucusunu yükleyen ve ölçek kümesi sanal makine örneğinin ana bilgisayar adını veren bir ölçek kümesi oluşturun. Özel Betik Uzantısı tanımı, GitHub’dan bir örnek betiği indirir, gerekli paketleri yükler, sonra sanal makine örneği ana bilgisayar adını bir temel HTML sayfasına yazar.


## <a name="create-a-scale-set"></a>Ölçek kümesi oluşturma
Şimdi [Yeni-AzVmss](/powershell/module/az.compute/new-azvmss)ile ayarlanmış sanal bir makine ölçeği oluşturun. Her bir sanal makine örneklerine trafiği dağıtmak için bir yük dengeleyici de oluşturulur. Yük dengeleyicisi, TCP bağlantı noktası 80'deki trafiği dağıtmak için kurallar içerir. Ayrıca TCP portu 3389 ve PowerShell remoting TCP port 5985 üzerinde uzak masaüstü trafiği sağlar. İstendiğinde, ölçek kümesindeki VM örnekleri için kendi yönetim kimlik bilgilerinizi ayarlayabilirsiniz:

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myScaleSet" `
  -Location "EastUS" `
  -VirtualNetworkName "myVnet" `
  -SubnetName "mySubnet" `
  -PublicIpAddressName "myPublicIPAddress" `
  -LoadBalancerName "myLoadBalancer" `
  -UpgradePolicyMode "Automatic"
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.


## <a name="create-custom-script-extension-definition"></a>Özel Betik Uzantısı tanımı oluşturma
Azure PowerShell, indirilecek dosyayı ve yürütülecek komutu depolamak için bir hashtable kullanır. Aşağıdaki örnekte, GitHub’dan örnek bir betik kullanılır. İlk olarak, aşağıdaki adımları uygulayarak bu yapılandırma nesnesini oluşturun:

```azurepowershell-interactive
$customConfig = @{
  "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}
```


Şimdi, [Add-AzVmssExtension](/powershell/module/az.Compute/Add-azVmssExtension)ile Özel Komut Dosyası Uzantısı uygulayın. Önceden tanımlanan yapılandırma nesnesi, uzantıya geçirilir. [Update-AzVmss](/powershell/module/az.compute/update-azvmss)ile VM örneklerindeki uzantıyı güncelleştirin ve çalıştırın.


```azurepowershell-interactive
# Get information about the scale set
$vmss = Get-AzVmss `
          -ResourceGroupName "myResourceGroup" `
          -VMScaleSetName "myScaleSet"

# Add the Custom Script Extension to install IIS and configure basic website
$vmss = Add-AzVmssExtension `
  -VirtualMachineScaleSet $vmss `
  -Name "customScript" `
  -Publisher "Microsoft.Compute" `
  -Type "CustomScriptExtension" `
  -TypeHandlerVersion 1.9 `
  -Setting $customConfig

# Update the scale set and apply the Custom Script Extension to the VM instances
Update-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

Ölçek kümesindeki her sanal makine örneği, GitHub’dan betiği indirip çalıştırır. Daha karmaşık bir örnekte, birden fazla uygulama bileşeni ve dosya yüklenebilir. Ölçek kümesinin ölçeği genişletilirse yeni sanal makine örnekleri otomatik olarak aynı Özel Betik Uzantısı tanımını uygular ve gerekli uygulamayı yükler.


## <a name="allow-traffic-to-application"></a>Uygulamaya giden trafiğe izin verme

Temel web uygulamasına erişime izin vermek için, [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig) ve [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup)ile bir ağ güvenlik grubu oluşturun. Daha fazla bilgi [için Azure sanal makine ölçek kümeleri için Ağ'a](virtual-machine-scale-sets-networking.md)bakın.

```azurepowershell-interactive

#Create a rule to allow traffic over port 80
$nsgFrontendRule = New-AzNetworkSecurityRuleConfig `
  -Name myFrontendNSGRule `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange * `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80 `
  -Access Allow

#Create a network security group and associate it with the rule
$nsgFrontend = New-AzNetworkSecurityGroup `
  -ResourceGroupName  "myResourceGroup" `
  -Location EastUS `
  -Name myFrontendNSG `
  -SecurityRules $nsgFrontendRule

$vnet = Get-AzVirtualNetwork `
  -ResourceGroupName  "myResourceGroup" `
  -Name myVnet

$frontendSubnet = $vnet.Subnets[0]

$frontendSubnetConfig = Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $vnet `
  -Name mySubnet `
  -AddressPrefix $frontendSubnet.AddressPrefix `
  -NetworkSecurityGroup $nsgFrontend

Set-AzVirtualNetwork -VirtualNetwork $vnet

```



## <a name="test-your-scale-set"></a>Ölçek kümenizi test etme
Web sunucunuzun iş başında olduğunu görmek için [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress)ile yük bakiyecinizin genel IP adresini alın. Aşağıdaki örnek, *myResourceGroup* kaynak grubunda oluşturulan IP adresini görüntüler:

```azurepowershell-interactive
Get-AzPublicIpAddress -ResourceGroupName "myResourceGroup" | Select IpAddress
```

Yük dengeleyicinin genel IP adresini bir web tarayıcısına girin. Aşağıdaki örnekte gösterildiği gibi yük dengeleyici trafiği VM örneklerinizden birine dağıtır:

![IIS’deki temel web sayfası](media/tutorial-install-apps-powershell/running-iis.png)

Sonraki adımda güncelleştirilmiş bir sürümü görebilmeniz için web tarayıcısını açık bırakın.


## <a name="update-app-deployment"></a>Uygulama dağıtımını güncelleştirme
Bir ölçek kümesinin yaşam döngüsü boyunca, uygulamanızın güncelleştirilmiş bir sürümünü dağıtmanız gerekebilir. Özel Betik Uzantısı ile, güncelleştirilmiş bir dağıtım betiğine başvurabilir ve sonra uzantıyı ölçek kümenize yeniden uygulayabilirsiniz. Ölçek kümesi önceki adımda oluşturulduğunda, `-UpgradePolicyMode` *otomatik*olarak ayarlandı. Bu ayar, ölçek kümesindeki sanal makine örneklerinin otomatik olarak güncelleştirilmesini ve uygulamanızın en son sürümünü uygulamasını sağlar.

*customConfigv2* adlı yeni bir yapılandırma tanımı oluşturun. Bu tanım, uygulama yükleme betiğinin güncelleştirilmiş bir *v2* sürümünü çalıştırır:

```azurepowershell-interactive
$customConfigv2 = @{
  "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis-v2.ps1");
  "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis-v2.ps1"
}
```

Özel Komut Dosyası Uzantısı yapılandırmasını ölçek kümenizdeki VM örnekleriyle güncelleştirin. Uygulamanın güncelleştirilmiş sürümünü uygulamak için *customConfigv2* tanımı kullanılır:

```azurepowershell-interactive
$vmss = Get-AzVmss `
          -ResourceGroupName "myResourceGroup" `
          -VMScaleSetName "myScaleSet"
 
$vmss.VirtualMachineProfile.ExtensionProfile[0].Extensions[0].Settings = $customConfigv2
 
Update-AzVmss `
  -ResourceGroupName "myResourceGroup" `
  -Name "myScaleSet" `
  -VirtualMachineScaleSet $vmss
```

Ölçek kümesindeki tüm sanal makine örnekleri otomatik olarak örnek web sayfasının en son sürümüyle güncelleştirilir. Güncelleştirilmiş sürümü görmek için tarayıcınızda web sitesini yenileyin:

![IIS’deki güncelleştirilmiş web sayfası](media/tutorial-install-apps-powershell/running-iis-updated.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme
Ölçek kümenizi ve ek kaynaklarınızı kaldırmak için Kaynak grubunu ve tüm kaynaklarını [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup)ile silin. `-Force` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar. `-AsJob` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür.

```azurepowershell-interactive
Remove-AzResourceGroup -Name "myResourceGroup" -Force -AsJob
```


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure PowerShell ile ölçek kümenizde otomatik olarak uygulama yükleme ve güncelleştirme işleminin nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Ölçek kümenize otomatik olarak uygulama yükleme
> * Azure Özel Betik Uzantısı’nı kullanma
> * Ölçek kümesinde çalıştırılan bir uygulamayı güncelleştirme

Ölçek kümenizi otomatik olarak ölçeklendirme hakkında bilgi almak için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Ölçek kümelerinizi otomatik olarak ölçeklendirme](tutorial-autoscale-powershell.md)
