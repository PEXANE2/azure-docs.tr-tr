---
title: Statik genel IP adresine sahip bir VM oluşturma - PowerShell | Microsoft Dokümanlar
description: PowerShell'i kullanarak statik genel IP adresine sahip bir VM'yi nasıl oluşturabilirsiniz öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: kumud
ms.openlocfilehash: 0eb4f86a2484486658171ab4b099794e4ba3e4bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76043387"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-powershell"></a>PowerShell'i kullanarak statik genel IP adresine sahip sanal bir makine oluşturun

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Statik genel IP adresine sahip sanal bir makine oluşturabilirsiniz. Genel BIR IP adresi, internetten sanal bir makineyle iletişim kurmanızı sağlar. Adresin hiçbir zaman değişmemesini sağlamak için dinamik bir adres yerine statik bir genel IP adresi atayın. [Statik genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#allocation-method)hakkında daha fazla bilgi edinin. Varolan bir sanal makineye atanan genel bir IP adresini dinamikten statike değiştirmek veya özel IP adresleriyle çalışmak için [bkz.](virtual-network-network-interface-addresses.md) Ortak IP adreslerinin [nominal ücreti](https://azure.microsoft.com/pricing/details/ip-addresses)vardır ve abonelik başına kullanabileceğiniz genel IP adreslerinin sayısı için bir [sınır](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) vardır.

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Aşağıdaki adımları yerel bilgisayarınızdan veya Azure Bulut Kabuğu'nu kullanarak tamamlayabilirsiniz. Yerel bilgisayarınızı kullanmak için Azure [PowerShell](/powershell/azure/install-az-ps?toc=%2fazure%2fvirtual-network%2ftoc.json)yüklü olduğundan emin olun. Azure Bulut Kabuğu'nu kullanmak için, aşağıdaki komut kutusunun sağ üst köşesinde **Bunu Dene'yi** seçin. Bulut Kabuğu sizi Azure'a imzalar.

1. Bulut Kabuğu'nu kullanıyorsanız, adım 2'ye geçin. Bir komut oturumu açın ve `Connect-AzAccount`Azure'da 'ile oturum açın
2. [Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, Doğu ABD Azure bölgesinde bir kaynak grubu oluşturur:

   ```azurepowershell-interactive
   New-AzResourceGroup -Name myResourceGroup -Location EastUS
   ```

3. [New-AzVM](/powershell/module/az.Compute/New-azVM) komutu ile sanal bir makine oluşturun. Seçenek, `-AllocationMethod "Static"` sanal makineye statik bir genel IP adresi atar. Aşağıdaki örnek, *myPublicIpAddress*adlı statik, temel SKU genel IP adresine sahip bir Windows Server sanal makine oluşturur. İstendiğinde, sanal makine için oturum açma kimlik bilgileri olarak kullanılacak bir kullanıcı adı ve parola sağlayın:

   ```azurepowershell-interactive
   New-AzVm `
     -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -PublicIpAddressName "myPublicIpAddress" `
     -AllocationMethod "Static"
   ```

   Genel IP adresi standart bir SKU olması gerekiyorsa, [ortak bir IP adresi oluşturmanız,](virtual-network-public-ip-address.md#create-a-public-ip-address)ağ [arabirimi oluşturmanız,](virtual-network-network-interface.md#create-a-network-interface) [ortak IP adresini ağ arabirimine atamanız](virtual-network-network-interface-addresses.md#add-ip-addresses)ve ardından [ağ arabirimine sahip sanal bir makine oluşturmanız](virtual-network-network-interface-vm.md#add-existing-network-interfaces-to-a-new-vm)gerekir. [Genel IP adresi SK'ler](virtual-network-ip-addresses-overview-arm.md#sku)hakkında daha fazla bilgi edinin. Sanal makine ortak bir Azure Yük Dengeleyicisinin arka uç havuzuna eklenecekse, sanal makinenin genel IP adresinin SKU'su yük bakiyesi ortak IP adresinin SKU'suyla eşleşmelidir. Ayrıntılar için Azure [Yük Dengeleyicisi'ne](../load-balancer/concepts-limitations.md#skus)bakın.

4. Atanan genel IP adresini görüntüleyin ve [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress)ile statik bir adres olarak oluşturulduğunu onaylayın:

   ```azurepowershell-interactive
   Get-AzPublicIpAddress `
     -ResourceGroupName "myResourceGroup" `
     -Name "myPublicIpAddress" `
     | Select "IpAddress", "PublicIpAllocationMethod" `
     | Format-Table
   ```

   Azure, sanal makineyi oluşturduğunuz bölgede kullandığınız adreslerden genel bir IP adresi atadı. Azure [Genel](https://www.microsoft.com/download/details.aspx?id=56519), [US government](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062) ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için bu aralıkların (ön ekler) listesini indirebilirsiniz.

> [!WARNING]
> Sanal makinenin işletim sistemindeki IP adresi ayarlarını değiştirmeyin. İşletim sistemi Azure genel IP adreslerinden habersizdir. İşletim sistemine özel IP adresi ayarları ekleyebilmenize rağmen, gerekli olmadıkça bunu yapmamanızı ve bir [işletim sistemine özel bir IP adresi eklemeyi](virtual-network-network-interface-addresses.md#private)okuyana kadar yapmamanızı öneririz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup) kullanabilirsiniz:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure'da [genel IP adresleri](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) hakkında daha fazla bilgi edinin
- Tüm genel [IP adresi ayarları](virtual-network-public-ip-address.md#create-a-public-ip-address) hakkında daha fazla bilgi edinin
- Azure sanal makinesine statik özel [IP adresi](virtual-network-network-interface-addresses.md#add-ip-addresses) atama ve [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) hakkında daha fazla bilgi edinin
- [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineleri oluşturma hakkında daha fazla bilgi edinin
