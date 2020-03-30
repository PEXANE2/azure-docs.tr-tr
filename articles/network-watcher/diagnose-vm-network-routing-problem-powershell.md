---
title: VM ağ yönlendirme sorununu tanıla - Azure PowerShell
titleSuffix: Azure Network Watcher
description: Bu makalede, Azure Ağ İzleyicisi'nin bir sonraki atlama özelliğini kullanarak sanal makine ağı yönlendirme sorununu nasıl tanıladığınızı öğrenirsiniz.
services: network-watcher
documentationcenter: network-watcher
author: damendo
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: damendo
ms.custom: ''
ms.openlocfilehash: b5a636471eab188dc8648761afedd81694331953
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76834714"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-powershell"></a>Sanal makine ağı yönlendirme sorununu tanılama - Azure PowerShell

Bu makalede, sanal bir makine (VM) dağıtın ve ardından bir IP adresi ve URL'ye iletişimi denetlersiniz. Bir iletişim hatasının nedenini ve bu hatayı nasıl çözeceğinizi belirlersiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

PowerShell'i yerel olarak yüklemeyi ve kullanmayı seçerseniz, `Az` bu makalede Azure PowerShell modülü gerekir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps). PowerShell'i yerel olarak çalıştırıyorsanız Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu da çalıştırmanız gerekir.



## <a name="create-a-vm"></a>VM oluşturma

Bir sanal makine oluşturabilmeniz için sanal makineyi içerecek bir kaynak grubu oluşturmanız gerekir. [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup)ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

[Yeni-AzVM](/powershell/module/az.compute/new-azvm)ile VM oluşturun. Bu adımı çalıştırırken kimlik bilgileri istenir. Girdiğiniz değerler, sanal makinenin kullanıcı adı ve parolası olarak yapılandırılır.

```azurepowershell-interactive
$vM = New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVm" `
    -Location "East US"
```

Sanal makinenin oluşturulması birkaç dakika sürer. Sanal makine oluşturulup PowerShell çıktı döndürünceye kadar kalan adımlara devam etmeyin.

## <a name="test-network-communication"></a>Ağ iletişimini test etme

Ağ İzleyicisi ile ağ iletişimini test etmek için önce test etmek istediğiniz VM bölgesinde bir ağ izleyicisine etkinleştirmeniz ve ardından iletişimi sınamak için Network Watcher'ın bir sonraki atlama özelliğini kullanmanız gerekir.

## <a name="enable-network-watcher"></a>Ağ izleyicisini etkinleştirme

Doğu ABD bölgesinde zaten etkin bir ağ izleyiciniz varsa, ağ izleyicisini almak için [Get-AzNetworkWatcher'ı](/powershell/module/az.network/get-aznetworkwatcher) kullanın. Aşağıdaki örnekte, *NetworkWatcherRG* kaynak grubunda bulunan *NetworkWatcher_eastus* adlı mevcut ağ izleyicisi alınır:

```azurepowershell-interactive
$networkWatcher = Get-AzNetworkWatcher `
  -Name NetworkWatcher_eastus `
  -ResourceGroupName NetworkWatcherRG
```

Doğu ABD bölgesinde zaten etkin bir ağ izleyiciniz yoksa, Doğu ABD bölgesinde bir ağ izleyicisi oluşturmak için [New-AzNetworkWatcher'ı](/powershell/module/az.network/new-aznetworkwatcher) kullanın:

```azurepowershell-interactive
$networkWatcher = New-AzNetworkWatcher `
  -Name "NetworkWatcher_eastus" `
  -ResourceGroupName "NetworkWatcherRG" `
  -Location "East US"
```

### <a name="use-next-hop"></a>Sonraki atlamayı kullanma

Azure, varsayılan hedeflerin yollarını otomatik olarak oluşturur. Varsayılan yolları geçersiz kılmak için özel yollar oluşturabilirsiniz. Bazı durumlarda, özel yollar iletişimin başarısız olmasına neden olabilir. Bir VM yönlendirmeyi test etmek için, trafiğin belirli bir adrese mukadder olduğu bir sonraki yönlendirme atlamasını belirlemek için [Get-AzNetworkWatcherNextHop](/powershell/module/az.network/get-aznetworkwatchernexthop) komutunu kullanın.

Sanal makineden, www.bing.com adresinin IP adreslerinden birine giden iletişimi test etme:

```azurepowershell-interactive
Get-AzNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 13.107.21.200
```

Birkaç saniye sonra, çıktı **NextHopType** **Internet**olduğunu bildirir , ve **RouteTableId** **Sistem Rotası**olduğunu. Bu sonuç, hedefe giden geçerli bir rota olduğunu bilmenizi sağlar.

Sanal makineden 172.31.0.100 adresine giden iletişimi test etme:

```azurepowershell-interactive
Get-AzNetworkWatcherNextHop `
  -NetworkWatcher $networkWatcher `
  -TargetVirtualMachineId $VM.Id `
  -SourceIPAddress 192.168.1.4 `
  -DestinationIPAddress 172.31.0.100
```

Döndürülen çıktı, **Hiçbiri** **NextHopType**olduğunu ve **RouteTableId'in** de **Sistem Rotası**olduğunu bildirir. Bu sonuç, hedefin geçerli bir sistem yolu olmasına rağmen trafiği hedefe yönlendiren bir sonraki atlama olmadığını size bildirir.

## <a name="view-details-of-a-route"></a>Bir yolun ayrıntılarını görüntüleme

Yönlendirmeyi daha fazla analiz etmek için [Get-AzEffectiveRouteTable](/powershell/module/az.network/get-azeffectiveroutetable) komutuyla ağ arabiriminin etkili rotalarını gözden geçirin:

```azurepowershell-interactive
Get-AzEffectiveRouteTable `
  -NetworkInterfaceName myVm `
  -ResourceGroupName myResourceGroup |
  Format-table
```

Aşağıdaki metni içeren çıktı döndürülür:

```powershell
Name State  Source  AddressPrefix           NextHopType NextHopIpAddress
---- -----  ------  -------------           ----------- ----------------
     Active Default {192.168.0.0/16}        VnetLocal   {}              
     Active Default {0.0.0.0/0}             Internet    {}              
     Active Default {10.0.0.0/8}            None        {}              
     Active Default {100.64.0.0/10}         None        {}              
     Active Default {172.16.0.0/12}         None        {}              
```

Önceki çıktıda da görebileceğiniz gibi, **0.0.0.0/0** **AdresPrefix'i** içeren rota, bir sonraki **Internet**atlamasıyla diğer rotanın adres önekleri içindeki adresler için mukadder olmayan tüm trafiği yönlendirir. Çıktıda da görebileceğiniz gibi, 172.16.0.0/12 önekinin varsayılan bir rotası olmasına rağmen, 172.31.0.100 adresini içeren **nextHopType** **Yok.** Azure, 172.16.0.0/12 için varsayılan bir yol oluşturur ancak bir neden olmadıkça sonraki atlama türünü belirtmez. Örneğin, sanal ağın adres alanına 172.16.0.0/12 adres aralığını eklediyseniz, Azure **sonraki HopType'ı** rota için **Sanal ağa** değiştirir. Bir denetim daha sonra **sonraki HopType**olarak **Sanal ağ** gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup) kullanabilirsiniz:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir VM oluşturdunuz ve VM'den ağ yönlendirmesi tanısı koydunuz. Azure’un birkaç varsayılan yol oluşturduğunu öğrendiniz ve iki farklı hedefin yolunu test ettiniz. [Azure'da yönlendirme](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve [özel yollar oluşturma](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route) hakkında daha fazla bilgi edinin.

Giden VM bağlantıları için, Ağ İzleyicisi'nin [bağlantı sorun giderme](network-watcher-connectivity-powershell.md) özelliğini kullanarak VM ile bitiş noktası arasındaki gecikmeyi ve ağ trafiğini belirleyebilirsiniz. Ağ İzleyicisi bağlantı izleme özelliğini kullanarak, bir VM ile IP adresi veya URL gibi bir uç nokta arasındaki iletişimi zaman içinde izleyebilirsiniz. Nasıl yapılacağını öğrenmek için [bkz.](connection-monitor.md)
