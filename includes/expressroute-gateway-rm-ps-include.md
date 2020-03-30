---
title: include dosyası
description: include dosyası
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: include
ms.date: 02/21/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 922ac7eb6cb9676af65700a6a2fe7fbae35a0dc5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188904"
---
Bu görevin adımları, aşağıdaki yapılandırma başvuru listesindeki değerleri temel alan bir VNet kullanır. Ek ayarlar ve adlar da bu listede özetlenmiştir. Bu listedeki değerlere dayalı değişkenler eklesek de, bu listeyi doğrudan adımların hiçbirinde kullanmayız. Listeyi başvuru olarak kullanmak üzere kopyalayarak değerleri kendi değerlerinideğiştirebilirsiniz.

* Sanal Ağ Adı = "TestVNet"
* Sanal Ağ adres alanı = 192.168.0.0/16
* Kaynak Grubu = "TestRG"
* Subnet1 Adı = "FrontEnd" 
* Subnet1 adres alanı = "192.168.1.0/24"
* Ağ Geçidi Subnet adı: "GatewaySubnet" Her zaman bir ağ geçidi alt ağ *GatewaySubnet*adlandırmanız gerekir.
* Ağ Geçidi Subnet adres alanı = "192.168.200.0/26"
* Bölge = "Doğu ABD"
* Ağ Geçidi Adı = "GW"
* Ağ Geçidi IP Adı = "GWIP"
* Ağ geçidi IP yapılandırma Adı = "gwipconf"
* Type = "ExpressRoute" Bu tür bir ExpressRoute yapılandırması için gereklidir.
* Ağ Geçidi Genel IP Adı = "gwpip"

## <a name="add-a-gateway"></a>Ağ geçidi ekleme
1. Azure Aboneliğinize bağlanın.

   [!INCLUDE [Sign in](expressroute-cloud-shell-connect.md)]
2. Bu alıştırma için değişkenlerinizi bildirin. Kullanmak istediğiniz ayarları yansıtacak şekilde örneği istediğinizden emin olun.

   ```azurepowershell-interactive 
   $RG = "TestRG"
   $Location = "East US"
   $GWName = "GW"
   $GWIPName = "GWIP"
   $GWIPconfName = "gwipconf"
   $VNetName = "TestVNet"
   ```
3. Sanal ağ nesnesini değişken olarak saklayın.

   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name $VNetName -ResourceGroupName $RG
   ```
4. Sanal Ağınıza bir ağ geçidi alt ağı ekleyin. Ağ geçidi alt ağı "GatewaySubnet" olarak adlandırılmalıdır. /27 veya daha büyük (/26, /25, vb.) bir ağ geçidi alt ağı oluşturmalısınız.

   ```azurepowershell-interactive
   Add-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -VirtualNetwork $vnet -AddressPrefix 192.168.200.0/26
   ```
5. Yapılandırmayı ayarlayın.

   ```azurepowershell-interactive
   $vnet = Set-AzVirtualNetwork -VirtualNetwork $vnet
   ```
6. Ağ geçidi alt netini değişken olarak saklayın.

   ```azurepowershell-interactive
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
   ```
7. Genel bir IP adresi isteyin. Ağ geçidini oluşturmadan önce IP adresi istenir. Kullanmak istediğiniz IP adresini belirtemezsiniz; dinamik olarak tahsis edilir. Sonraki yapılandırma bölümünde bu IP adresini kullanacaksınız. Ayırma Yöntemi Dinamik olmalıdır.

   ```azurepowershell-interactive
   $pip = New-AzPublicIpAddress -Name $GWIPName  -ResourceGroupName $RG -Location $Location -AllocationMethod Dynamic
   ```
8. Ağ geçidiniz için yapılandırmaoluşturun. Ağ geçidi yapılandırması, kullanılacak alt ağı ve genel IP adresini tanımlar. Bu adımda, ağ geçidi oluştururken kullanılacak yapılandırmayı belirtin. Bu adım aslında ağ geçidi nesnesi oluşturmaz. Aşağıdaki örneği kullanarak kendi ağ geçidi yapılandırmanızı oluşturun.

   ```azurepowershell-interactive
   $ipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GWIPconfName -Subnet $subnet -PublicIpAddress $pip
   ```
9. Ağ geçidini oluşturun. Bu adımda, **-GatewayType** özellikle önemlidir. **ExpressRoute**değerini kullanmalısınız. Bu cmdlets çalıştırdıktan sonra, ağ geçidi oluşturmak için 45 dakika veya daha fazla sürebilir.

   ```azurepowershell-interactive
   New-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG -Location $Location -IpConfigurations $ipconf -GatewayType Expressroute -GatewaySku Standard
   ```

## <a name="verify-the-gateway-was-created"></a>Ağ geçidinin oluşturulduğunu doğrulama
Ağ geçidinin oluşturulduğunu doğrulamak için aşağıdaki komutları kullanın:

```azurepowershell-interactive
Get-AzVirtualNetworkGateway -ResourceGroupName $RG
```

## <a name="resize-a-gateway"></a>Ağ geçidini yeniden boyutlandırma
[Ağ Geçidi SKUs](../articles/expressroute/expressroute-about-virtual-network-gateways.md)bir dizi vardır. Ağ Geçidi SKU'yu istediğiniz zaman değiştirmek için aşağıdaki komutu kullanabilirsiniz.

> [!IMPORTANT]
> Bu komut UltraPerformance ağ geçidi için çalışmaz. Ağ geçidinizi bir UltraPerformance ağ geçidine değiştirmek için önce varolan ExpressRoute ağ geçidini kaldırın ve ardından yeni bir UltraPerformance ağ geçidi oluşturun. Ağ geçidinizi bir UltraPerformance ağ geçidinden düşürmek için önce UltraPerformance ağ geçidini kaldırın ve ardından yeni bir ağ geçidi oluşturun.
> 
> 

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
Resize-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -GatewaySku HighPerformance
```

## <a name="remove-a-gateway"></a>Ağ geçitlerini kaldırma
Ağ geçidini kaldırmak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Remove-AzVirtualNetworkGateway -Name $GWName -ResourceGroupName $RG
```
