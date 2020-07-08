---
title: dosya dahil etme
description: dosya dahil etme
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 02/21/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 922ac7eb6cb9676af65700a6a2fe7fbae35a0dc5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188904"
---
Bu görevin adımları aşağıdaki yapılandırma başvurusu listesindeki değerleri temel alarak bir sanal ağ kullanır. Ek ayarlar ve adlar bu listede de özetlenmiştir. Bu liste, bu listedeki değerlere göre değişken eklediğimiz halde, bu listeyi doğrudan herhangi bir adımda kullanmıyoruz. Bir başvuru olarak kullanmak için listeyi kopyalayabilir ve değerleri kendi değerlerinizle değiştirin.

* Sanal ağ adı = "TestVNet"
* Sanal ağ adres alanı = 192.168.0.0/16
* Kaynak grubu = "TestRG"
* Subnet1 Name = "ön uç" 
* Subnet1 adres alanı = "192.168.1.0/24"
* Ağ geçidi alt ağ adı: "GatewaySubnet" her zaman bir ağ geçidi alt ağı *gatewaysubnet*adını vermelisiniz.
* Ağ geçidi alt ağ adres alanı = "192.168.200.0/26"
* Region = "Doğu ABD"
* Ağ geçidi adı = "GW"
* Ağ geçidi IP adı = "GWıP"
* Ağ geçidi IP yapılandırma adı = "gwipconf"
* Type = "ExpressRoute" Bu tür bir ExpressRoute yapılandırması için gereklidir.
* Ağ Geçidi genel IP adı = "gwpıp"

## <a name="add-a-gateway"></a>Ağ geçidi ekleme
1. Azure aboneliğinize bağlanın.

   [!INCLUDE [Sign in](expressroute-cloud-shell-connect.md)]
2. Bu alıştırma için değişkenlerinizi bildirin. Örneği, kullanmak istediğiniz ayarları yansıtacak şekilde düzenlemediğinizden emin olun.

   ```azurepowershell-interactive 
   $RG = "TestRG"
   $Location = "East US"
   $GWName = "GW"
   $GWIPName = "GWIP"
   $GWIPconfName = "gwipconf"
   $VNetName = "TestVNet"
   ```
3. Sanal ağ nesnesini bir değişken olarak depolayın.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG
   ```
4. Sanal ağınıza bir ağ geçidi alt ağı ekleyin. Ağ geçidi alt ağı "GatewaySubnet" olarak adlandırılmalıdır. /27 veya daha büyük (/26,/25 vb.) bir ağ geçidi alt ağı oluşturmanız gerekir.

   ```azurepowershell-interactive
   Add-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
   ```
5. Yapılandırmayı ayarlayın.

   ```azurepowershell-interactive
   $vnet = Set-AzVirtualNetwork -VirtualNetwork $vnet
   ```
6. Ağ geçidi alt ağını bir değişken olarak depolayın.

   ```azurepowershell-interactive
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
   ```
7. Genel bir IP adresi isteyin. Ağ Geçidi oluşturulmadan önce IP adresi istendi. Kullanmak istediğiniz IP adresini belirtemezsiniz; dinamik olarak ayrılır. Sonraki yapılandırma bölümünde bu IP adresini kullanacaksınız. AllocationMethod dinamik olmalıdır.

   ```azurepowershell-interactive
   $pip = New-AzPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
   ```
8. Ağ geçidinizin yapılandırmasını oluşturun. Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Bu adımda, ağ geçidini oluştururken kullanılacak yapılandırmayı belirtirsiniz. Bu adım aslında ağ geçidi nesnesini oluşturmaz. Aşağıdaki örneği kullanarak kendi ağ geçidi yapılandırmanızı oluşturun.

   ```azurepowershell-interactive
   $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
   ```
9. Ağ geçidini oluşturun. Bu adımda **-gatewaytype** özellikle önemlidir. **ExpressRoute**değerini kullanmanız gerekir. Bu cmdlet 'leri çalıştırdıktan sonra ağ geçidinin oluşturulması 45 dakika veya daha fazla sürebilir.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
   ```

## <a name="verify-the-gateway-was-created"></a>Ağ geçidinin oluşturulduğunu doğrulayın
Ağ geçidinin oluşturulduğunu doğrulamak için aşağıdaki komutları kullanın:

```azurepowershell-interactive
Get-AzVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Ağ geçidini yeniden boyutlandırma
Birçok [ağ geçidi SKU](../articles/expressroute/expressroute-about-virtual-network-gateways.md)'su vardır. Ağ Geçidi SKU 'sunu dilediğiniz zaman değiştirmek için aşağıdaki komutu kullanabilirsiniz.

> [!IMPORTANT]
> Bu komut UltraPerformance ağ geçidi için çalışmıyor. Ağ geçidinizi bir UltraPerformance ağ geçidine dönüştürmek için, önce mevcut ExpressRoute Gateway 'i kaldırın ve ardından yeni bir UltraPerformance Ağ Geçidi oluşturun. Ağ geçidinizin bir UltraPerformance ağ geçidiyle indirgenmesini sağlamak için önce UltraPerformance ağ geçidini kaldırın ve ardından yeni bir ağ geçidi oluşturun.
> 
> 

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Ağ geçitlerini kaldırma
Bir ağ geçidini kaldırmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Remove-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```
