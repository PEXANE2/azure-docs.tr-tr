---
title: Azure PowerShell'i kullanarak yük dengeleme ve giden kuralları yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure PowerShell kullanarak Standart Yük Dengeleyici'nde yük dengeleme ve giden kurallarınasıl yapılandırılanın gösterilmektedir.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: 5fd68f4559420ca688b3f4d6f6d66ee52db5191e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74225440"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-by-using-azure-powershell"></a>Azure PowerShell'i kullanarak Standart Yük Dengeleyici'nde yük dengeleme ve giden kuralları yapılandırma

Bu makalede, Azure PowerShell kullanarak Standart Yük Dengeleyici'nde giden kuralları nasıl yapılandırabileceğiniz gösterilmektedir.  

Bu makalenin senaryosunu tamamladığınızda, yük dengeleyici kaynağı iki ön uç ve ilişkili kuralları içerir. Gelen trafik için bir ön uç ve giden trafik için başka bir ön uç var.  

Her ön uç genel bir IP adresine başvurur. Bu senaryoda, gelen trafiğin genel IP adresi giden trafiğin adresinden farklıdır.   Yük dengeleme kuralı yalnızca gelen yük dengelemesağlar. Giden kural VM için giden ağ adresi çevirisini (NAT) denetler.  

Senaryoda iki arka uç havuzu kullanır: biri gelen trafik, diğeri giden trafik için. Bu havuzlar yeteneği gösterir ve senaryo için esneklik sağlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="connect-to-your-azure-account"></a>Azure hesabınıza bağlanma
[Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutunu kullanarak Azure aboneliğinizde oturum açın. Ardından ekrandaki yönergeleri izleyin.
    
```azurepowershell-interactive
Connect-AzAccount
```
## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0)kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı mantıksal bir kapsayıcıdır. Kaynaklar daha sonra gruptan yönetilir.

Aşağıdaki örnek, *eastus2* konumunda *myresourcegroupoutbound* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
New-AzResourceGroup -Name myresourcegroupoutbound -Location eastus
```
## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
*myvnetoutbound*adlı bir sanal ağ oluşturun. Onun subnet *mysubnetoutbound*adı . Yeni-AzVirtualNetwork ve [Yeni-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=azps-2.6.0)kullanarak *myresourcegroupoutbound* yerleştirin. [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=azps-2.6.0)

```azurepowershell-interactive
$subnet = New-AzVirtualNetworkSubnetConfig -Name mysubnetoutbound -AddressPrefix "192.168.0.0/24"

New-AzVirtualNetwork -Name myvnetoutbound -ResourceGroupName myresourcegroupoutbound -Location eastus -AddressPrefix "192.168.0.0/16" -Subnet $subnet
```

## <a name="create-an-inbound-public-ip-address"></a>Gelen genel IP adresi oluşturma 

Web uygulamanıza internet üzerinden erişmek için yük dengeleyicisi için genel bir IP adresine ihtiyacınız vardır. Standart Yük Dengeleyicisi yalnızca standart genel IP adreslerini destekler. 

*Myresourcegroupoutbound'da* *mypublicipinbound* adlı standart bir genel IP adresi oluşturmak için [Yeni-AzPublicIpAddress'i](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0) kullanın.

```azurepowershell-interactive
$pubIPin = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipinbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-an-outbound-public-ip-address"></a>Giden genel IP adresi oluşturma 

[Yeni-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0)kullanarak yük bakiyesi ön uç giden yapılandırma için standart bir IP adresi oluşturun.

```azurepowershell-interactive
$pubIPout = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipoutbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-an-azure-load-balancer"></a>Azure yük dengeleyici oluşturma

Bu bölümde, yük dengeleyicisinin aşağıdaki bileşenlerinin nasıl oluşturulup yapılandırılabildiğini açıklar:
  - Yük bakiyesi üzerinde gelen ağ trafiğini alan bir ön uç IP
  - Ön uç IP'sinin yük dengeli ağ trafiğini gönderdiği bir arka uç havuzu
  - Giden bağlantı için arka uç havuzu
  - Arka uç VM örneklerinin durumunu belirleyen bir sistem durumu sondası
  - Trafiğin VM'lere nasıl dağıtıldığını tanımlayan bir yük dengeleyici gelen kuralı
  - VM'lerden trafiğin nasıl dağıtıldığını tanımlayan bir yük dengeleyici giden kural

### <a name="create-an-inbound-front-end-ip"></a>Gelen bir ön uç IP'si oluşturma
[New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0)kullanarak yük dengeleyicisi için gelen ön uç IP yapılandırmasını oluşturun. Yük *dengeleyicisi myfrontendinbound*adlı bir gelen ön uç IP yapılandırma içermelidir. Bu yapılandırmayı ortak IP adresi *mypublicipinbound*ile ilişkilendirin.

```azurepowershell-interactive
$frontendIPin = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendinbound" -PublicIpAddress $pubIPin
```
### <a name="create-an-outbound-front-end-ip"></a>Giden bir ön uç IP'si oluşturma
[New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0)kullanarak yük dengeleyici için giden ön uç IP yapılandırmaoluşturun. Bu yük *dengeleyicisi myfrontendoutbound*adlı giden bir ön uç IP yapılandırma içermelidir. Bu yapılandırmayı genel IP adresi *mypublicipoutbound*ile ilişkilendirin.

```azurepowershell-interactive
$frontendIPout = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendoutbound" -PublicIpAddress $pubIPout
```
### <a name="create-an-inbound-back-end-pool"></a>Gelen arka uç havuzu oluşturma
[New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0)kullanarak yük dengeleyiciiçin arka uç gelen havuzu oluşturun. Havuzun *bepoolinbound*adını söyle.

```azurepowershell-interactive
$bepoolin = New-AzLoadBalancerBackendAddressPoolConfig -Name bepoolinbound
``` 

### <a name="create-an-outbound-back-end-pool"></a>Giden arka uç havuzu oluşturma
[New-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0)kullanarak VMs havuzu için giden bağlantı tanımlamak için başka bir arka uç adresi havuzu oluşturmak için aşağıdaki komutu kullanın. Bu havuzun *bepooloutbound*adını ver. 

Ayrı bir giden havuz oluşturarak, maksimum esneklik sağlarsınız. Ama bu adımı atlayabilir ve isterseniz sadece gelen *bepoolinbound* kullanabilirsiniz.  

```azurepowershell-interactive
$bepoolout = New-AzLoadBalancerBackendAddressPoolConfig -Name bepooloutbound
``` 

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Bir sistem durumu sondası, ağ trafiği gönderebildiklerinden emin olmak için tüm VM örneklerini denetler. Sonda denetimlerinde başarısız olan VM örneği, yeniden çevrimiçi olana kadar yük dengeleyicisinden kaldırılır ve sonda denetimi bunun sağlıklı olup olmadığını belirler. 

VM'lerin durumunu izlemek için, [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig?view=azps-2.6.0)kullanarak bir sağlık sondası oluşturun. 

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig -Name http -Protocol "http" -Port 80 -IntervalInSeconds 15 -ProbeCount 2 -RequestPath /
```
### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı, gelen trafik için ön uç IP yapılandırmasını ve trafiği almak için arka uç havuzunu tanımlar. Ayrıca gerekli kaynak ve hedef bağlantı noktasını tanımlar. 

[Yeni-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig?view=azps-2.6.0)kullanarak *myinboundlbrule* adlı bir yük dengeleyici kuralı oluşturun. Bu kural ön uç havuzu *myfrontendinbound*port 80 dinleyecek. Ayrıca arka uç adresi *havuzuna*yük dengeli ağ trafiği göndermek için bağlantı noktası 80 kullanır. 

```azurepowershell-interactive
$inboundRule = New-AzLoadBalancerRuleConfig -Name inboundlbrule -FrontendIPConfiguration $frontendIPin -BackendAddressPool $bepoolin -Probe $probe -Protocol "Tcp" -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -DisableOutboundSNAT
```

>[!NOTE]
>Bu yük dengeleme kuralı otomatik giden güvenli NAT (SNAT) devre dışı çünkü **-DisableOutboundSNAT** parametresi. Giden NAT yalnızca giden kural tarafından sağlanır.

### <a name="create-an-outbound-rule"></a>Giden kuralı oluşturma

Giden kural ön uç *myfrontendoutbound*tarafından temsil edilir ön uç ortak IP, tanımlar. Bu ön uç, tüm giden NAT trafiği nin yanı sıra kuralın geçerli olduğu arka uç havuzu için de kullanılır.  

*Bepool* arka uç havuzundaki tüm VM'lerin (NIC IP yapılandırmalarında) giden ağ çevirisi için giden kural *myoutboundrule* oluşturmak için aşağıdaki komutu kullanın.  Komut, giden boşta kalma süresini 4 dakikadan 15 dakikaya değiştirir. 1.024 yerine 10.000 SNAT bağlantı noktası ayırır. Daha fazla bilgi için Bkz. [New-AzLoadBalancerOutboundRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalanceroutboundruleconfig?view=azps-2.7.0).

```azurepowershell-interactive
 $outboundRule = New-AzLoadBalancerOutBoundRuleConfig -Name outboundrule -FrontendIPConfiguration $frontendIPout -BackendAddressPool $bepoolout -Protocol All -IdleTimeoutInMinutes 15 -AllocatedOutboundPort 10000
```
Ayrı bir giden havuz kullanmak istemiyorsanız, bunun yerine *$bepoolin* belirtmek için önceki komuttaki adres havuzu bağımsız değişkenini değiştirebilirsiniz.  Ortaya çıkan yapılandırmayı esnek ve okunabilir hale getirmek için ayrı havuzlar kullanmanızı öneririz.

### <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma

[New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer?view=azps-2.6.0)kullanarak gelen IP adresi için bir yük dengeleyicioluşturmak için aşağıdaki komutu kullanın. Yük *dengeleyicilb*adı . Gelen bir ön uç IP yapılandırması içermelidir. Onun arka uç havuzu *bepoolinbound* önceki adımda oluşturduğunuz genel IP adresi *mypublicipinbound* ile ilişkili olmalıdır.

```azurepowershell-interactive
New-AzLoadBalancer -Name lb -Sku Standard -ResourceGroupName myresourcegroupoutbound -Location eastus -FrontendIpConfiguration $frontendIPin,$frontendIPout -BackendAddressPool $bepoolin,$bepoolout -Probe $probe -LoadBalancingRule $inboundrule -OutboundRule $outboundrule 
```

Bu noktada, ilgili NIC kaynaklarının IP yapılandırmasını güncelleştirerek Hem *havuza bağlı* hem de *havuza bağlı* arka uç havuzlarına VM'lerinizi eklemeye devam edebilirsiniz. [Add-AzNetworkInterfaceIpConfig](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest)kullanarak kaynak yapılandırmasını güncelleştirin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kaynak grubuna, yük bakiyesine ve ilgili kaynaklara artık ihtiyacınız olmadığında, [Bunları Kaldır-AzResourceGroup'u](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.7.0)kullanarak kaldırabilirsiniz.

```azurepowershell-interactive 
  Remove-AzResourceGroup -Name myresourcegroupoutbound
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, standart bir yük dengeleyicisi oluşturdunuz, hem gelen hem de giden yük dengeleyici trafik kurallarını yapılandırıldınız ve arka uç havuzundaki VM'ler için bir sistem durumu sondası yapılandırıldınız. 

Daha fazla bilgi edinmek [için Azure Yük Dengeleyici'nin öğreticilerine](tutorial-load-balancer-standard-public-zone-redundant-portal.md)devam edin.
