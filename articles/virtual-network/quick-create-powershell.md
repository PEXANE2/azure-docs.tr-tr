---
title: Sanal ağ oluşturma - quickstart - Azure PowerShell
titlesuffix: Azure Virtual Network
description: Bu hızlı başlangıçta, Azure portalını kullanarak sanal ağ oluşturmayı öğreneceksiniz. Sanal ağ, sanal makineler gibi Azure kaynaklarının birbirleriyle ve internetle özel olarak iletişim kurmasına olanak tanır.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.service: virtual-network
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: kumud
ms.openlocfilehash: 1d30b35264707c59c899cc3a224e4affa2a4696e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79241447"
---
# <a name="quickstart-create-a-virtual-network-using-powershell"></a>Hızlı başlangıç: PowerShell kullanarak sanal ağ oluşturma

Sanal ağ, sanal makineler (VM'ler) gibi Azure kaynaklarının birbirleriyle ve internetle özel olarak iletişim kurmasına olanak tanır. Bu hızlı başlangıçta, sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ oluşturduktan sonra, sanal ağa iki sanal makine dağıtacaksınız. Daha sonra Internet'ten Sanal M'lere bağlanır ve sanal ağ üzerinden özel olarak iletişim kurarsınız.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine PowerShell'i yerel olarak yüklemeye ve kullanmaya karar verirseniz, bu hızlı başlatma, Azure PowerShell modülü sürüm 1.0.0 veya sonraki sürümlerini kullanmanızı gerektirir. Yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükleme ve yükseltme bilgileri için [Azure PowerShell modüllerini yükleyin'](/powershell/azure/install-az-ps) e bakın.

Son olarak, PowerShell'i yerel olarak çalıştırıyorsanız, `Connect-AzAccount`çalıştırmanız da gerekir. Bu komut Azure ile bir bağlantı oluşturur.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Kaynak grubu ve sanal ağ oluşturma

Kaynak grubunuzu ve sanal ağınızı yapılandırmak için yürümeniz gereken birkaç adım vardır.

### <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Sanal ağ oluşturmadan önce, sanal ağı barındıracak bir kaynak grubu oluşturmanız gerekir. [New-AzResourceGroup](/powershell/module/az.Resources/New-azResourceGroup)ile bir kaynak grubu oluşturun. Bu örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

[New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)ile sanal ağ oluşturun. Bu örnek, *EastUS* konumunda *myVirtualNetwork* adında varsayılan bir sanal ağ oluşturur:

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Alt ağ ekleme

Azure kaynakları sanal ağdaki bir alt ağa dağıtır, bu nedenle bir alt ağ oluşturmanız gerekir. [Add-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/add-azvirtualnetworksubnetconfig)ile *varsayılan* adlı bir alt net yapılandırması oluşturun:

```azurepowershell-interactive
$subnetConfig = Add-AzVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

### <a name="associate-the-subnet-to-the-virtual-network"></a>Alt ağı sanal ağla ilişkilendirme

[Set-AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork)ile sanal ağa alt ağ yapılandırması yazabilirsiniz. Bu komut alt ağı oluşturur:

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

[New-AzVM](/powershell/module/az.compute/new-azvm)ile ilk VM oluşturun. Bir sonraki komutu çalıştırdığınızda, kimlik bilgileri için istenirsiniz. VM için bir kullanıcı adı ve parola girin:

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

Seçenek `-AsJob` arka planda VM oluşturur. Bir sonraki adıma devam edebilirsiniz.

Azure arka planda VM oluşturmaya başladığında, böyle bir şeyi geri alırsınız:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzVM
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

Bu komutla ikinci VM'yi oluşturun:

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

Başka bir kullanıcı ve parola oluşturmanız gerekir. Azure'un VM'yi oluşturması birkaç dakika sürer.

> [!IMPORTANT]
> Azure tamamlanana kadar bir sonraki adıma devam etmeyin.  PowerShell çıktısını geri verdiğinde yapıldığını anlarsınız.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Bir VM'nin genel IP adresini döndürmek için [Get-AzPublicIpAddress'i](/powershell/module/az.network/get-azpublicipaddress) kullanın. Bu *örnek, myVm1* VM'nin genel IP adresini döndürür:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Yerel bilgisayarınızda bir komut istemi açın. `mstsc` komutunu çalıştırın. Son `<publicIpAddress>` adımdan döndürülen genel IP adresiyle değiştirin:

> [!NOTE]
> Bu komutları yerel bilgisayarınızdaki bir PowerShell komut isteminden çalıştırıyorsanız ve Az PowerShell modül sürüm 1.0 veya daha sonrakullanıyorsanız, bu arabirimde devam edebilirsiniz.

```cmd
mstsc /v:<publicIpAddress>
```
1. İstendiğinde **Bağlan**’ı seçin.

1. Sanal makine oluştururken belirttiğiniz kullanıcı adını ve parolayı girin.

    > [!NOTE]
    > VM'yi oluşturduğunuzda girdiğiniz kimlik bilgilerini belirtmek için**farklı bir hesap kullanın,** **daha fazla seçenek** > seçmeniz gerekebilir.

1. **Tamam'ı**seçin.

1. Bir sertifika uyarısı alabilirsiniz. Bunu **yaparsanız, Evet** veya **Devam'ı**seçin.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

1. *myVm1*uzak masaüstünde, PowerShell açın.

1. `ping myVm2` yazın.

    Böyle bir şeyi geri alacaksın:

    ```powershell
    PS C:\Users\myVm1> ping myVm2

    Pinging myVm2.ovvzzdcazhbu5iczfvonhg2zrb.bx.internal.cloudapp.net
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.0.0.5:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Internet Denetim İleti Protokolü (ICMP) kullandığından ping başarısız olur. Varsayılan olarak, ICMP'nin Windows güvenlik duvarınızdan geçmesine izin verilmez.

1. *myVm2'nin* daha sonraki bir adımda *myVm1* pingini atamasını sağlamak için şu komutu girin:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Bu komut, ICMP'nin Windows güvenlik duvarından içeri girişini sağlar.

1. *myVm1* ile uzak masaüstü bağlantısını kapatın.

1. [Internet'ten VM'ye Bağlan'daki adımları yineleyin.](#connect-to-a-vm-from-the-internet) Bu sefer, *myVm2'ye*bağlanın.

1. *myVm2* sanal makinesindeki bir komut isteminden `ping myvm1` komutunu girin.

    Böyle bir şeyi geri alacaksın:

    ```cmd
    C:\windows\system32>ping myVm1

    Pinging myVm1.e5p2dibbrqtejhq04lqrusvd4g.bx.internal.cloudapp.net [10.0.0.4] with 32 bytes of data:
    Reply from 10.0.0.4: bytes=32 time=2ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.0.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 2ms, Average = 0ms
    ```

    Önceki bir adımda *myVm1* sanal makinesinde Windows güvenlik duvarı üzerinden ICMP’ye izin verdiğinizden, *myVm1*’den yanıt alırsınız.

1. *myVm2* ile uzak masaüstü bağlantısını kapatın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sanal ağ ve Sanal Kaynaklar'la iş yaptığınızda, kaynak grubunu ve sahip olduğu tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup) kullanın:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternet’ten bir sanal makineye bağlandınız ve sanal makine ile başka bir sanal makine arasında özel olarak iletişim kurdunuz. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Azure, sanal makineler arasında sınırsız özel iletişim sağlar. Varsayılan olarak Azure, Windows VM'lerine yalnızca Internet'ten gelen uzak masaüstü bağlantılarına izin verir. Farklı VM ağ iletişimi türlerini yapılandırma hakkında daha fazla bilgi edinmek için [Filtre ağı trafiği](tutorial-filter-network-traffic.md) öğreticisine gidin.
