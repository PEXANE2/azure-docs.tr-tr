---
title: Azure PowerShell kullanarak yük dengelemeyi ve giden kuralları yapılandırma
titlesuffix: Azure Load Balancer
description: Bu makalede, Azure PowerShell kullanılarak Standart Load Balancer yük dengelemenin ve giden kuralların nasıl yapılandırılacağı gösterilmektedir.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: 6e85d31e40e35b9d796fc66394a6eb446c7302ad
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262628"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-using-azure-powershell"></a>Azure PowerShell kullanarak standart yük dengeleyicide yük dengelemeyi ve giden kuralları yapılandırma

Bu makalede, Azure PowerShell kullanarak standart yük dengeleyicide giden kuralların nasıl yapılandırılacağı gösterilir.  

İşiniz bittiğinde, yük dengeleyici kaynağı iki ön uç ve bununla ilişkili kuralları içerir: biri gelen ve çıkış için bir tane.  Her ön uç genel IP adresi ve bu senaryo kullanır, farklı bir genel IP adresi için gelen ve giden bir başvuru içeriyor.   Yük Dengeleme kuralını yalnızca gelen Yük Dengeleme sağlar ve giden NAT VM için sağlanan giden kuralı denetler.  Bu makalede, bir diğeri gelen ve giden için bir tane olmak üzere iki ayrı arka uç havuzu kullanılır ve bu senaryo için esneklik ve esneklik sağlar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)] 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="connect-to-azure-account"></a>Azure hesabına Bağlan
[Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin:
    
```azurepowershell-interactive
Connect-AzAccount
```
## <a name="create-resource-group"></a>Kaynak grubu oluşturma

[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0)ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnekte adlı bir kaynak grubu oluşturur *myresourcegroupoutbound* içinde *eastus2* konumu:

```azurepowershell-interactive
New-AzResourceGroup -Name myresourcegroupoutbound -Location eastus
```
## <a name="create-virtual-network"></a>Sanal ağ oluşturma
[New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=azps-2.6.0) ve [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=azps-2.6.0)kullanarak *myresourcegroupoutbağlanmadı* *adlı bir* alt ağ ile *myvnetoutbağlanmadı* adlı bir sanal ağ oluşturun:

```azurepowershell-interactive
$subnet = New-AzVirtualNetworkSubnetConfig -Name mysubnetoutbound -AddressPrefix "192.168.0.0/24"

New-AzVirtualNetwork -Name myvnetoutbound -ResourceGroupName myresourcegroupoutbound -Location eastus -AddressPrefix "192.168.0.0/16" -Subnet $subnet
```

## <a name="create-inbound-public-ip-address"></a>Gelen genel IP adresi oluştur 

Web uygulamanıza İnternet’ten erişmek için yük dengeleyicinin genel IP adresi gereklidir. Standart yük dengeleyici yalnızca standart genel IP adreslerini destekler. *Myresourcegroupoutbound*içinde *mypublicipınbound* ADLı standart bir genel IP adresi oluşturmak Için [New-azpublicıpaddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0) ' i kullanın.

```azurepowershell-interactive
$pubIPin = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipinbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-outbound-public-ip-address"></a>Giden genel IP adresi oluşturma 

[New-Azpublicıpaddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress?view=azps-2.6.0)kullanarak yük dengeleyicisinin ön uç giden yapılandırması için standart bir IP adresi oluşturun.

```azurepowershell-interactive
$pubIPout = New-AzPublicIpAddress -ResourceGroupName myresourcegroupoutbound -Name mypublicipoutbound -AllocationMethod Static -Sku Standard -Location eastus
```

## <a name="create-azure-load-balancer"></a>Azure yük dengeleyici oluşturma

Bu bölümde yük dengeleyicinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz açıklanmaktadır:
  - Yük dengeleyicideki gelen ağ trafiğini alan bir ön uç IP.
  - Ön uç IP 'nin yük dengeli ağ trafiğini gönderdiği bir arka uç Havuzu.
  - Giden bağlantı için bir arka uç Havuzu. 
  - arka uç sanal makine örneklerinin durumunu belirleyen bir durum araştırması.
  - Trafiğin sanal makinelere dağıtımını tanımlayan bir yük dengeleyici gelen kuralı.
  - Trafiğin sanal makinelerinden dağıtımını tanımlayan bir yük dengeleyici giden kuralı.

### <a name="create-inbound-frontend-ip"></a>Gelen ön uç IP 'si oluştur
Genel IP adresiyle ilişkili myfrontendinbound adlı bir gelen ön uç IP yapılandırması içeren [New-Azloadbalancerfrontendıpconfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0) ile yük dengeleyici IÇIN giden ön uç IP yapılandırmasını oluşturun  *mypublicipınbound*

```azurepowershell-interactive
$frontendIPin = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendinbound" -PublicIpAddress $pubIPin
```
### <a name="create-outbound-frontend-ip"></a>Giden ön uç IP oluşturma
Genel IP adresiyle ilişkili myfrontendoutbound adlı bir giden ön uç IP yapılandırması içeren [New-Azloadbalancerfrontendıpconfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig?view=azps-2.6.0) ile yük dengeleyici IÇIN giden ön uç IP yapılandırmasını oluşturun  *mypublicıpoıb utbağımlıdır*:

```azurepowershell-interactive
$frontendIPout = New-AzLoadBalancerFrontendIPConfig -Name "myfrontendoutbound" -PublicIpAddress $pubIPout
```
### <a name="create-inbound-backend-pool"></a>Gelen arka uç Havuzu Oluştur
*Bepoolınbound*adlı [New-Azloadbalancerbackendadddresspoolconfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0) ile yük dengeleyici için arka uç gelen havuzunu oluşturun:

```azurepowershell-interactive
$bepoolin = New-AzLoadBalancerBackendAddressPoolConfig -Name bepoolinbound
``` 

### <a name="create-outbound-backend-pool"></a>Giden arka uç Havuzu Oluştur
*Bepooloutbound*adlı [New-Azloadbalancerbackendadddresspoolconfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig?view=azps-2.6.0) ile bir VM havuzu için giden bağlantıyı tanımlamak üzere ek bir arka uç adres havuzu oluşturun. Ayrı bir giden havuzun oluşturulması en yüksek esnekliği sağlar, ancak bu adımı atlayabilir ve yalnızca gelen *bepoolınas* 'yı kullanabilirsiniz.  :

```azurepowershell-interactive
$bepoolout = New-AzLoadBalancerBackendAddressPoolConfig -Name bepooloutbound
``` 

### <a name="create-health-probe"></a>Durum araştırması oluşturma

Sistem durumu araştırması tüm sanal makine örneklerini denetleyerek ağ trafiği gönderdiklerinden emin olur. Sistem durumu denetimi başarısız olan sanal makine örnekleri tekrar çevrimiçi olana ve sistem durumu denetimi iyi olduğuna karar verene kadar yük dengeleyiciden kaldırılır. Sanal makinelerin sistem durumunu izlemek için [New-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig?view=azps-2.6.0) ile bir sistem durumu araştırması oluşturun. 

```azurepowershell-interactive
$probe = New-AzLoadBalancerProbeConfig -Name http -Protocol "http" -Port 80 -IntervalInSeconds 15 -ProbeCount 2 -RequestPath /
```
### <a name="create-load-balancing-rule"></a>Yük Dengeleme kuralı oluşturma

Yük Dengeleyici kuralı, gelen trafik ve gerekli kaynak ve hedef bağlantı noktalarının yanı sıra trafiği almak için arka uç havuzu için ön uç IP yapılandırması tanımlar. Ön *uç havuzundaki 80* numaralı bağlantı noktasını dinlemek ve arka uç adres havuzuna [](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig?view=azps-2.6.0) *Yük dengeli ağ trafiği göndermek için New-azloadbalancerruleconfig ile myınboundlbrule adlı bir yük dengeleyici kuralı oluşturun* Ayrıca, 80 numaralı bağlantı noktasını kullanan bepoolın. 

>[!NOTE]
>Bu yük dengeleme kuralı, bu kuralın **-disableoutboundsnat** parametresiyle bir sonucu olarak OTOMATIK giden NAT 'yi devre dışı bırakır. Giden NAT, yalnızca giden kuralı tarafından sağlanır.

```azurepowershell-interactive
$inboundRule = New-AzLoadBalancerRuleConfig -Name inboundlbrule -FrontendIPConfiguration $frontendIPin -BackendAddressPool $bepoolin -Probe $probe -Protocol "Tcp" -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -EnableFloatingIP -LoadDistribution SourceIP -DisableOutboundSNAT
```

### <a name="create-outbound-rule"></a>Giden kuralı oluşturma

Ön uç tarafından temsil edilen ön uç genel IP, bir giden kuralı tanımlayan *myfrontendoutbound*, doğrulayacak bu kuralın uygulandığı arka uç havuzu yanı sıra tüm giden NAT trafiği için.  Bir giden kuralı oluşturma *myoutboundrule* ağdaki tüm sanal makinelerin (NIC IP yapılandırmaları) giden ağ çeviri *bepool* arka uç havuzu.  Aşağıdaki komutta giden boşta kalma zaman aşımı 4'ten 15 dakika olarak değiştirir ve 10000 SNAT 1024 yerine bağlantı noktası ayırır.  Daha fazla ayrıntı için [New-AzLoadBalancerOutboundRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalanceroutboundruleconfig?view=azps-2.7.0) ' i gözden geçirin.

```azurepowershell-interactive
 $outboundRule = New-AzLoadBalancerOutBoundRuleConfig -Name outboundrule -FrontendIPConfiguration $frontendIPout -BackendAddressPool $bepoolout -Protocol All -IdleTimeoutInMinutes 15 -AllocatedOutboundPort 10000
```
Ayrı bir giden havuz kullanmak istemiyorsanız, bunun yerine *$bepoolin* belirtmek için önceki komutta adres havuzu bağımsız değişkenini değiştirebilirsiniz.  Elde edilen yapılandırmanın esneklik ve okunabilirlik için ayrı havuzlar kullanmanızı öneririz.

### <a name="create-load-balancer"></a>Yük dengeleyici oluştur

Gelen bir ön uç IP yapılandırması ve genel IP adresiyle [](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer?view=azps-2.6.0) *ilişkili bir arka uç havuzu olan lb ADLı New-azloadbalancer kullanarak gelen IP adresine sahip bir yük dengeleyici oluşturun önceki adımda oluşturduğunuz mypublicipınbound* .

```azurepowershell-interactive
New-AzLoadBalancer -Name lb -Sku Standard -ResourceGroupName myresourcegroupoutbound -Location eastus -FrontendIpConfiguration $frontendIPin,$frontendIPout -BackendAddressPool $bepoolin,$bepoolout -Probe $probe -LoadBalancingRule $inboundrule -OutboundRule $outboundrule 
```

Bu noktada, [Add-Aznetworkınterfaceipconfig](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest)komutunu kullanarak ilgili NIC kaynaklarının IP yapılandırmasını güncelleştirerek sanal makinelerinizi, *bepoolgelen* __ve__ *bepooloutbound* arka uç havuzuna eklemeye devam edebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.7.0) komutunu kullanarak kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları kaldırabilirsiniz.

```azurepowershell-interactive 
  Remove-AzResourceGroup -Name myresourcegroupoutbound
```

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, arka uç havuzundaki VM 'Ler için hem gelen yük dengeleyici trafik kuralları, hem de yapılandırılan sistem durumu araştırması yapılandırılmış standart yük dengeleyici oluşturdunuz. Azure Load Balancer hakkında daha fazla bilgi edinmek için, Azure yük dengeleyicisine yönelik öğreticilere geçin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
