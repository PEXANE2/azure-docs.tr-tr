---
title: Statik özel IP adresine sahip bir VM oluşturma - Azure PowerShell
description: PowerShell'i kullanarak özel IP adresine sahip sanal bir makineyi nasıl oluşturabilirsiniz öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/07/2019
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 1745ca176fac18b4903686cb556670531ee40a1a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244764"
---
# <a name="create-a-virtual-machine-with-a-static-private-ip-address-using-powershell"></a>PowerShell'i kullanarak statik özel IP adresine sahip sanal bir makine oluşturun

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Statik özel IP adresine sahip sanal bir makine (VM) oluşturabilirsiniz. Bir alt ağdan hangi adresin VM'ye atandığını seçmek istiyorsanız, dinamik bir adres yerine statik bir özel IP adresi atayın. [Statik özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#allocation-method)hakkında daha fazla bilgi edinin. Varolan bir VM'ye atanan özel bir IP adresini dinamikten statike değiştirmek veya genel IP adresleriyle çalışmak için [bkz.](virtual-network-network-interface-addresses.md)

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Aşağıdaki adımları yerel bilgisayarınızdan veya Azure Bulut Kabuğu'nu kullanarak tamamlayabilirsiniz. Yerel bilgisayarınızı kullanmak için Azure [PowerShell](/powershell/azure/install-az-ps?toc=%2fazure%2fvirtual-network%2ftoc.json)yüklü olduğundan emin olun. Azure Bulut Kabuğu'nu kullanmak için, aşağıdaki komut kutusunun sağ üst köşesinde **Bunu Dene'yi** seçin. Bulut Kabuğu sizi Azure'a imzalar.

1. Bulut Kabuğu'nu kullanıyorsanız, adım 2'ye geçin. Bir komut oturumu açın ve `Connect-AzAccount`Azure'da 'ile oturum açın
2. [Yeni-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnek, Doğu ABD Azure bölgesinde bir kaynak grubu oluşturur:

   ```azurepowershell-interactive
   $RgName = "myResourceGroup"
   $Location = "eastus"
   New-AzResourceGroup -Name $RgName -Location $Location
   ```

3. [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) ve [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) komutları ile bir alt ağ yapılandırması ve sanal ağ oluşturun:

   ```azurepowershell-interactive
   # Create a subnet configuration
   $SubnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name MySubnet `
   -AddressPrefix 10.0.0.0/24

   # Create a virtual network
   $VNet = New-AzVirtualNetwork `
   -ResourceGroupName $RgName `
   -Location $Location `
   -Name MyVNet `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $subnetConfig

   # Get the subnet object for use in a later step.
   $Subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
   ```

4. Sanal ağda bir ağ arabirimi oluşturun ve [Yeni-AzNetworkInterfaceIpConfig](/powershell/module/Az.Network/New-AzNetworkInterfaceIpConfig) ve [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) komutları ile alt ağdan ağ arabirimine özel bir IP adresi atayın:

   ```azurepowershell-interactive
   $IpConfigName1 = "IPConfig-1"
   $IpConfig1     = New-AzNetworkInterfaceIpConfig `
     -Name $IpConfigName1 `
     -Subnet $Subnet `
     -PrivateIpAddress 10.0.0.4 `
     -Primary

   $NIC = New-AzNetworkInterface `
     -Name MyNIC `
     -ResourceGroupName $RgName `
     -Location $Location `
     -IpConfiguration $IpConfig1
   ```

5. [New-AzVMConfig](/powershell/module/Az.Compute/New-AzVMConfig)ile bir VM yapılandırması oluşturun ve sonra [New-AzVM](/powershell/module/az.Compute/New-azVM)ile VM oluşturun. İstendiğinde, VM için oturum açma kimlik bilgileri olarak kullanılacak bir kullanıcı adı ve parola sağlayın:

   ```azurepowershell-interactive
   $VirtualMachine = New-AzVMConfig -VMName MyVM -VMSize "Standard_DS3"
   $VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName MyServerVM -ProvisionVMAgent -EnableAutoUpdate
   $VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
   $VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest
   New-AzVM -ResourceGroupName $RgName -Location $Location -VM $VirtualMachine -Verbose
   ```

> [!WARNING]
> İşletim sistemine özel IP adresi ayarları ekleyebilmenize rağmen, [işletim sistemine özel bir IP adresi ekle'yi](virtual-network-network-interface-addresses.md#private)okuyana kadar bunu yapmamanızı öneririz.
> 
> 
> <a name = "change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a>
> 
> [!IMPORTANT]
> VM'ye internetten erişmek için VM'ye genel bir IP adresi atamalısınız. Dinamik bir özel IP adresi atamasını statik bir atamayla da değiştirebilirsiniz. Ayrıntılar için [IP adresi ekle veya değiştir'e](virtual-network-network-interface-addresses.md)bakın. Ayrıca, bir ağ güvenlik grubunu ağ arabirimiyle, ağ arabirimini oluşturduğunuz alt ağla veya her ikisiyle ilişkilendirerek ağ trafiğini VM'inizle sınırlamanız önerilir. Ayrıntılar için [ağ güvenlik gruplarını yönet'e](manage-network-security-group.md)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup'u](/powershell/module/az.resources/remove-azresourcegroup) kullanabilirsiniz:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure sanal makinesine statik özel [IP adresi](virtual-network-network-interface-addresses.md#add-ip-addresses) atama ve [özel IP adresleri](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) hakkında daha fazla bilgi edinin.
- [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) sanal makineleri oluşturma hakkında daha fazla bilgi edinin.
