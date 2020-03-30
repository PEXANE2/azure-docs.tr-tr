---
title: 'Sanal WAN: NVA için sanal hub rota tablosu oluşturun: Azure PowerShell'
description: Trafiği bir ağ sanal cihazına yönlendirmek için sanal WAN sanal hub rota tablosu.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to work with routing tables for NVA.
ms.openlocfilehash: a55e1453fe7fe4d135286b22dabf58d434762581
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645115"
---
# <a name="create-a-virtual-hub-route-table-to-steer-traffic-to-a-network-virtual-appliance"></a>Ağı Sanal Cihaz'a yönlendirmek için Sanal Hub rota tablosu oluşturma

Bu makalede, sanal hub'dan Ağ Sanal Cihaz'a nasıl trafik yönlendirilen gösterilmektedir. 

![Sanal WAN diyagramı](./media/virtual-wan-route-table/vwanroute.png)

Bu makalede şunları öğreneceksiniz:

* WAN oluşturma
* Hub oluşturma
* Hub sanal ağ bağlantıları oluşturma
* Hub rotası oluşturma
* Yönlendirme tablosu oluşturma
* Rota tablosunu uygulama

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Aşağıdaki ölçütleri karşıladığınızı doğrulayın:

1. Bir Ağ Sanal Cihaz (NVA) var. Bu, genellikle sanal ağdaki Azure Marketi'nden sağlanan seçtiğiniz üçüncü taraf bir yazılımdır.
2. NVA ağ arabirimine atanmış özel bir IP'niz var. 
3. NVA sanal hub'da dağıtılamaz. Ayrı bir VNet'te dağıtılmalıdır. Bu makalede, NVA VNet 'DMZ VNet' olarak adlandırılır.
4. 'DMZ VNet'in bir veya çok sayıda sanal ağı olabilir. Bu makalede, bu VNet 'Dolaylı vNet konuştu' olarak adlandırılır. Bu VNet'ler VNet eşleme kullanılarak DMZ VNet'e bağlanabilir.
5. Zaten oluşturulmuş 2 VNet'inolduğunu doğrulayın. Bunlar vnets konuştu olarak kullanılacaktır. Bu makale için, VNet konuşan adres boşlukları 10.0.2.0/24 ve 10.0.3.0/24'tür. VNet oluşturma hakkında bilgiye ihtiyacınız varsa, [PowerShell'i kullanarak sanal ağ oluşturma](../virtual-network/quick-create-powershell.md)bilgisine bakın.
6. Herhangi bir VNet'te sanal ağ ağ geçidi olmadığından emin olun.

## <a name="1-sign-in"></a><a name="signin"></a>1. Oturum aç

Resource Manager PowerShell cmdlets'in en son sürümünü yüklediğinizden emin olun. PowerShell cmdlet'lerini yükleme hakkında daha fazla bilgi için bkz. [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/install-az-ps). Bu önemlidir, çünkü cmdlet’lerin daha önceki sürümleri bu alıştırma için gereken geçerli değerleri içermez.

1. PowerShell konsolunuzu yüksek ayrıcalıklarla açın ve Azure hesabınızda oturum açın. Bu cmdlet oturum açma kimlik bilgilerini ister. Oturum açma işleminden sonra hesap ayarlarınızı Azure PowerShell'e göre indirebilir.

   ```powershell
   Connect-AzAccount
   ```
2. Azure aboneliklerinizin bir listesini alın.

   ```powershell
   Get-AzSubscription
   ```
3. Kullanmak istediğiniz aboneliği belirtin.

   ```powershell
   Select-AzSubscription -SubscriptionName "Name of subscription"
   ```

## <a name="2-create-resources"></a><a name="rg"></a>2. Kaynak oluşturma

1. Bir kaynak grubu oluşturun.

   ```powershell
   New-AzResourceGroup -Location "West US" -Name "testRG"
   ```
2. Sanal bir WAN oluşturun.

   ```powershell
   $virtualWan = New-AzVirtualWan -ResourceGroupName "testRG" -Name "myVirtualWAN" -Location "West US"
   ```
3. Sanal bir hub oluşturun.

   ```powershell
   New-AzVirtualHub -VirtualWan $virtualWan -ResourceGroupName "testRG" -Name "westushub" -AddressPrefix "10.0.1.0/24" -Location "West US"
   ```

## <a name="3-create-connections"></a><a name="connections"></a>3. Bağlantı oluşturma

Dolaylı Spoke VNet ve DMZ VNet'ten sanal hub'a hub sanal ağ bağlantıları oluşturun.

  ```powershell
  $remoteVirtualNetwork1= Get-AzVirtualNetwork -Name "indirectspoke1" -ResourceGroupName "testRG"
  $remoteVirtualNetwork2= Get-AzVirtualNetwork -Name "indirectspoke2" -ResourceGroupName "testRG"
  $remoteVirtualNetwork3= Get-AzVirtualNetwork -Name "dmzvnet" -ResourceGroupName "testRG"

  New-AzVirtualHubVnetConnection -ResourceGroupName "testRG" -VirtualHubName "westushub" -Name  "testvnetconnection1" -RemoteVirtualNetwork $remoteVirtualNetwork1
  New-AzVirtualHubVnetConnection -ResourceGroupName "testRG" -VirtualHubName "westushub" -Name  "testvnetconnection2" -RemoteVirtualNetwork $remoteVirtualNetwork2
  New-AzVirtualHubVnetConnection -ResourceGroupName "testRG" -VirtualHubName "westushub" -Name  "testvnetconnection3" -RemoteVirtualNetwork $remoteVirtualNetwork3
  ```

## <a name="4-create-a-virtual-hub-route"></a><a name="route"></a>4. Sanal hub rotası oluşturma

Bu makale için, Dolaylı Spoke VNet adres boşlukları 10.0.2.0/24 ve 10.0.3.0/24 ve DMZ NVA ağ arabirimi özel IP adresi 10.0.4.5'tir.

```powershell
$route1 = New-AzVirtualHubRoute -AddressPrefix @("10.0.2.0/24", "10.0.3.0/24") -NextHopIpAddress "10.0.4.5"
```

## <a name="5-create-a-virtual-hub-route-table"></a><a name="applyroute"></a>5. Sanal hub rota tablosu oluşturma

Sanal hub rota tablosu oluşturun ve oluşturulan rotayı uygulayın.
 
```powershell
$routeTable = New-AzVirtualHubRouteTable -Route @($route1)
```

## <a name="6-commit-the-changes"></a><a name="commit"></a>6. Değişiklikleri gerçekleştirme

Değişiklikleri sanal hub'a ada.

```powershell
Update-AzVirtualHub -ResourceGroupName "testRG" -Name "westushub" -RouteTable $routeTable
```

## <a name="next-steps"></a>Sonraki adımlar

Sanal WAN hakkında daha fazla bilgi için [Sanal WAN'a Genel Bakış](virtual-wan-about.md) sayfasına bakın.
