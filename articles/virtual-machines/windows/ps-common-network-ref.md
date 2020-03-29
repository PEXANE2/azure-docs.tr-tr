---
title: Azure Sanal Ağlar için Ortak PowerShell komutları
description: Sanal ağ ve VM'ler için ilişkili kaynakları oluşturmaya başlamanızı sağlamak için Ortak PowerShell komutları.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 56e1a73c-8299-4996-bd03-f74585caa1dc
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 07/17/2017
ms.author: cynthn
ms.openlocfilehash: b9debe04759772ef51946dc99943ec4eff6f61dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77911840"
---
# <a name="common-powershell-commands-for-azure-virtual-networks"></a>Azure Sanal Ağlar için Ortak PowerShell komutları

Sanal bir makine oluşturmak istiyorsanız, sanal bir [ağ](../../virtual-network/virtual-networks-overview.md) oluşturmanız veya VM'nin eklenebileceği varolan bir sanal ağ hakkında bilgi niz olması gerekir. Genellikle, bir VM oluşturduğunuzda, bu makalede açıklanan kaynakları oluşturmayı da düşünmeniz gerekir.

Azure PowerShell’in en son sürümünü yükleme, aboneliğinizi seçme ve hesabınızda oturum açma hakkında bilgi almak için bkz. [Azure PowerShell’i yükleme ve yapılandırma](/powershell/azure/overview).

Bu makaledeki komutlardan birden fazlasını çalıştırıyorsanız bazı değişkenler sizin için yararlı olabilir:

- $location - Ağ kaynaklarının konumu. Sizin için çalışan bir coğrafi [bölge](https://azure.microsoft.com/regions/) bulmak için [Get-AzLocation'ı](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation) kullanabilirsiniz.
- $myResourceGroup - Ağ kaynaklarının bulunduğu kaynak grubunun adı.

## <a name="create-network-resources"></a>Ağ kaynakları oluşturma

| Görev | Komut |
| ---- | ------- |
| Alt ağ yapılandırmaları oluşturma |$subnet1 = [Yeni-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig) -Adı "mySubnet1" -AddressPrefix XX. X.X.X/XX<BR>$subnet2 = New-AzVirtualNetworkSubnetConfig -Adı "mySubnet2" -AddressPrefix XX. X.X.X/XX<BR><BR>Tipik bir [ağ, yük dengeleyicisi ile karşı karşıya olan](../../load-balancer/load-balancer-internet-overview.md) bir internet için bir alt ağa ve dahili yük [dengeleyicisi](../../load-balancer/load-balancer-internal-overview.md)için ayrı bir alt ağa sahip olabilir. |
| Sanal ağ oluşturma |$vnet = [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork) -Name "myVNet" -ResourceGroupName $myResourceGroup -Location $location -AddressPrefix XX. X.X.X/XX -Alt ağ $subnet1, $subnet2 |
| Benzersiz bir etki alanı adı için test edin |[Test-AzDnsAvailability](https://docs.microsoft.com/powershell/module/az.network/test-azdnsavailability) -DomainNameLabel "myDNS" -Konum $location<BR><BR>Azure tarafından yönetilen DNS sunucularında genel IP adresine domainname.location.cloudapp.azure.com için eşleme oluşturan [ortak bir IP kaynağı](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)için bir DNS etki alanı adı belirtebilirsiniz. Ad yalnızca küçük harf, sayı ve kısa çizgi içerebilir. İlk ve son karakter bir harf veya sayı olmalı ve etki alanı adı Azure konumu içinde benzersiz olmalıdır. **True** döndürülürse, önerilen adınız genel olarak benzersizdir. |
| Genel IP adresi oluşturma |$pip = [New-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/new-azpublicipaddress) -Name "myPublicIp" -ResourceGroupName $myResourceGroup -DomainNameLabel "myDNS" -Location $location -AllocationMethod Dynamic<BR><BR>Ortak IP adresi, daha önce test ettiğiniz etki alanı adını kullanır ve yük dengeleyicisinin ön uç yapılandırması tarafından kullanılır. |
| Frontend IP yapılandırması oluşturma |$frontendIP = [New-AzLoadBalancerFrontendIpConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerfrontendipconfig) -Adı "myFrontendIP" -PublicIpAddress $pip<BR><BR>Ön uç yapılandırması, gelen ağ trafiği için daha önce oluşturduğunuz genel IP adresini içerir. |
| Arka uç adres havuzu oluşturma |$beAddressPool = [Yeni-AzLoadBalancerBackendAddressPoolConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig) -Adı "myBackendAddressPool"<BR><BR>Ağ arabirimi üzerinden erişilen yük dengeleyicisinin arka ucu için dahili adresler sağlar. |
| Sonda oluşturma |$healthProbe = [Yeni-AzLoadBalancerProbeConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerprobeconfig) -Adı "myProbe" -RequestPath 'HealthProbe.aspx' -Protokol http -Port 80 -IntervalInSeconds 15 -ProbeCount 2<BR><BR>Arka uç adres havuzundaki sanal makinelerin kullanılabilirliğini denetlemek için kullanılan sistem durumu sondalarını içerir. |
| Yük dengeleme kuralı oluşturma |$lbRule = [Yeni-AzLoadBalancerRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerruleconfig) -Name HTTP -FrontendIpConfiguration $frontendIP -BackendAddressPool $beAddressPool -Prob $healthProbe -Protocol Tcp -FrontendPort 80 -BackendPort 80<BR><BR>Yük bakiyesi üzerindeki ortak bağlantı noktasını arka uç adres havuzundaki bir bağlantı noktasına atayabilen kurallar içerir. |
| Gelen NAT kuralı oluşturma |$inboundNATRule = [New-AzLoadBalancerInboundNatRuleConfig](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancerinboundnatruleconfig) -Name "myInboundRule1" -FrontendIpConfiguration $frontendIP -Protocol TCP -FrontendPort 3441 -BackendPort 3389<BR><BR>Arka uç adres havuzundaki belirli bir sanal makine için yük dengeleyicisindeki ortak bağlantı noktasını bir bağlantı noktasına eşleme kuralları içerir. |
| Yük dengeleyici oluşturma |$loadBalancer = [New-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/new-azloadbalancer) -ResourceGroupName $myResourceGroup -Name "myLoadBalancer" -Location $location -FrontendIpConfiguration $frontendIP -InboundNatRule $inboundNATRule -LoadBalancingRule $lbRule -BackendAddressPool $beAddressPool -Probe $healthProbe |
| Ağ arabirimi oluşturma |$nic1= [New-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/new-aznetworkinterface) -ResourceGroupName $myResourceGroup -Name "myNIC" -Yer $location -PrivateIpAddress XX. X.X.X -Subnet $subnet2 -LoadBalancerBackendAddressPool $loadBalancer.BackendAddressPools[0] -LoadBalancerInboundNatRule $loadBalancer.InboundNatRules[0]<BR><BR>Daha önce oluşturduğunuz genel IP adresini ve sanal ağ alt netini kullanarak bir ağ arabirimi oluşturun. |

## <a name="get-information-about-network-resources"></a>Ağ kaynakları hakkında bilgi alın

| Görev | Komut |
| ---- | ------- |
| Sanal ağları listele |[Get-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetwork) -ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki tüm sanal ağları listeler. |
| Sanal ağ hakkında bilgi alın |Get-AzVirtualNetwork -Adı "myVNet" -ResourceGroupName $myResourceGroup |
| Sanal ağdaki alt ağları listele |Get-AzVirtualNetwork -Name "myVNet" -ResourceGroupName $myResourceGroup &#124; Select Subnets |
| Bir alt ağ hakkında bilgi alın |[Get-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/get-azvirtualnetworksubnetconfig) -Adı "mySubnet1" -VirtualNetwork $vnet<BR><BR>Belirtilen sanal ağdaki alt ağ hakkında bilgi alır. $vnet değeri Get-AzVirtualNetwork tarafından döndürülen nesneyi temsil eder. |
| IP adreslerini listele |[Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) -ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki ortak IP adreslerini listeler. |
| Yük dengeleyicileri listele |[Get-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer) -ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki tüm yük dengeleyicilerini listeler. |
| Ağ arabirimlerini listele |[Get-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkinterface) -ResourceGroupName $myResourceGroup<BR><BR>Kaynak grubundaki tüm ağ arabirimlerini listeler. |
| Ağ arabirimi hakkında bilgi alın |Get-AzNetworkInterface -Adı "myNIC" -ResourceGroupName $myResourceGroup<BR><BR>Belirli bir ağ arabirimi hakkında bilgi alır. |
| Ağ arabiriminin IP yapılandırmasını alma |[Get-AzNetworkInterfaceIPConfig](https://docs.microsoft.com/powershell/module/az.network/get-aznetworkinterfaceipconfig) -Adı "myNICIP" -NetworkInterface $nic<BR><BR>Belirtilen ağ arabiriminin IP yapılandırması hakkında bilgi alır. $nic değeri Get-AzNetworkInterface tarafından döndürülen nesneyi temsil eder. |

## <a name="manage-network-resources"></a>Ağ kaynaklarını yönetme

| Görev | Komut |
| ---- | ------- |
| Sanal ağa alt ağ ekleme |[Ekle-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/add-azvirtualnetworksubnetconfig) -AdresPrefix XX. X.X.X/XX -Adı "mySubnet1" -VirtualNetwork $vnet<BR><BR>Varolan bir sanal ağa bir alt ağ ekler. $vnet değeri Get-AzVirtualNetwork tarafından döndürülen nesneyi temsil eder. |
| Sanal ağı silme |[Remove-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/remove-azvirtualnetwork) -Adı "myVNet" -ResourceGroupName $myResourceGroup<BR><BR>Belirtilen sanal ağı kaynak grubundan kaldırır. |
| Ağ arabirimini silme |[Remove-AzNetworkInterface](https://docs.microsoft.com/powershell/module/az.network/remove-aznetworkinterface) -Adı "myNIC" -ResourceGroupName $myResourceGroup<BR><BR>Belirtilen ağ arabirimini kaynak grubundan kaldırır. |
| Yük dengeleyici silme |[Remove-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/remove-azloadbalancer) -Adı "myLoadBalancer" -ResourceGroupName $myResourceGroup<BR><BR>Belirtilen yük dengeleyicisini kaynak grubundan kaldırır. |
| Genel bir IP adresini silme |[Remove-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/remove-azpublicipaddress)-Adı "myIPAddress" -ResourceGroupName $myResourceGroup<BR><BR>Belirtilen ortak IP adresini kaynak grubundan kaldırır. |

## <a name="next-steps"></a>Sonraki Adımlar
[VM oluştururken](../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)oluşturduğunuz ağ arabirimini kullanın.


