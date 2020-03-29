---
title: Genel bir IP adresini sanal makineyle ilişkilendirme
titlesuffix: Azure Virtual Network
description: Genel bir IP adresini sanal bir makineyle nasıl ilişkilendireceklerini öğrenin.
services: virtual-network
documentationcenter: ''
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2019
ms.author: kumud
ms.openlocfilehash: 5acda69ce08bc493d5349b084d1cfafc8432145b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75647467"
---
# <a name="associate-a-public-ip-address-to-a-virtual-machine"></a>Genel bir IP adresini sanal makineyle ilişkilendirme

Bu makalede, genel bir IP adresini varolan bir sanal makineyle (VM) nasıl ilişkilendirdiğinizi öğrenirsiniz. Bir VM'ye internetten bağlanmak istiyorsanız, VM'nin bununla ilişkili ortak bir IP adresi olmalıdır. Herkese açık bir IP adresine sahip yeni bir VM oluşturmak istiyorsanız, bunu [Azure portalını](virtual-network-deploy-static-pip-arm-portal.md), [Azure komut satırı arabirimini (CLI)](virtual-network-deploy-static-pip-arm-cli.md)veya [PowerShell'i](virtual-network-deploy-static-pip-arm-ps.md)kullanarak yapabilirsiniz. Genel IP adreslerinin nominal ücreti vardır. Ayrıntılar için [fiyatlandırmaya](https://azure.microsoft.com/pricing/details/ip-addresses/)bakın. Abonelik başına kullanabileceğiniz genel IP adreslerinin sayısının bir sınırı vardır. Ayrıntılar için [sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#publicip-address)görün.

Ortak bir IP adresini Bir VM ile ilişkilendirmek için [Azure portalını](#azure-portal), Azure [komut satırı arabirimini](#azure-cli) (CLI) veya [PowerShell'i](#powershell) kullanabilirsiniz.

## <a name="azure-portal"></a>Azure portalında

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Herkese açık IP adresini eklemek istediğiniz sanal makineye göz atın veya arama yapın ve sonra seçin.
3. **Ayarlar**altında **Ağ'ı**seçin ve ardından aşağıdaki resimde gösterildiği gibi genel IP adresini eklemek istediğiniz ağ arabirimini seçin:

   ![Ağ arabirimini seçin](./media/associate-public-ip-address-vm/select-nic.png)

   > [!NOTE]
   > Ortak IP adresleri, VM'ye bağlı ağ arabirimleriyle ilişkilidir. Önceki resimde, VM yalnızca bir ağ arabirimi vardır. VM'de birden çok ağ arabirimi varsa, bunların tümü görünür ve ortak IP adresini ilişkilendirmek istediğiniz ağ arabirimini seçersiniz.

4. Aşağıdaki resimde gösterildiği gibi **IP yapılandırmalarını** seçin ve ardından bir IP yapılandırması seçin:

   ![IP yapılandırması seçin](./media/associate-public-ip-address-vm/select-ip-configuration.png)

   > [!NOTE]
   > Ortak IP adresleri, ağ arabirimi için IP yapılandırmaları ile ilişkilidir. Önceki resimde, ağ arabiriminde bir IP yapılandırması vardır. Ağ arabiriminde birden çok IP yapılandırması varsa, bunların tümü listede görünür ve ortak IP adresini ilişkilendirmek istediğiniz IP yapılandırmasını seçersiniz.

5. **Etkin'i**seçin, ardından IP adresini seçin **( Gerekli ayarları*yapılandırın*)**. **Genel IP adres** kutusunu otomatik olarak kapatan varolan bir genel IP adresi seçin. Listelenen herkese açık genel IP adresleri yoksa, bir tane oluşturmanız gerekir. Nasıl yapılacağını öğrenmek için [bkz.](virtual-network-public-ip-address.md#create-a-public-ip-address) Aşağıdaki resimde gösterildiği gibi **Kaydet'i**seçin ve ardından IP yapılandırması için kutuyu kapatın.

   ![Genel IP adresini etkinleştirme](./media/associate-public-ip-address-vm/enable-public-ip-address.png)

   > [!NOTE]
   > Görünen ortak IP adresleri, VM ile aynı bölgede bulunan adreslerdir. Bölgede oluşturulan birden çok genel IP adresiniz varsa, tümü burada görünür. Varsa, adres zaten farklı bir kaynakla ilişkili olmasıdır.

6. Aşağıdaki resimde gösterildiği gibi IP yapılandırmasına atanan genel IP adresini görüntüleyin. Bir IP adresinin görünmesi birkaç saniye sürebilir.

   ![Atanan genel IP adresini görüntüleme](./media/associate-public-ip-address-vm/view-assigned-public-ip-address.png)

   > [!NOTE]
   > Adres, her Azure bölgesinde kullanılan adresler havuzundan atanır. Her bölgede kullanılan adres havuzlarının listesini görmek için [Microsoft Azure Veri Merkezi IP Aralıkları'na](https://www.microsoft.com/download/details.aspx?id=41653)bakın. Atanan adres, bölge için kullanılan havuzlarda herhangi bir adres olabilir. Adresin bölgedeki belirli bir havuzdan atanması gerekiyorsa, [Genel IP adresi öneki](public-ip-address-prefix.md)kullanın.

7. Ağ güvenlik grubunda güvenlik kuralları yla [VM'ye ağ trafiğine izin verin.](#allow-network-traffic-to-the-vm)

## <a name="azure-cli"></a>Azure CLI

Azure [CLI'yi](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)yükleyin veya Azure Bulut Kabuğu'nu kullanın. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bunu izleyen CLI komutlarında **Try it** düğmesini seçin. Try **it** seçeneğini seçmek, Azure hesabınızda oturum açabileceğiniz bir Bulut Bulutu'nu çağırır.

1. CLI'yi Bash'te yerel olarak kullanıyorsanız, Azure'da '' ile `az login`oturum açın.
2. Ortak IP adresi, VM'ye bağlı bir ağ arabiriminin IP yapılandırması ile ilişkilidir. Ortak bir IP adresini bir IP yapılandırmasına ilişkilendirmek için [az network nic-ip-config güncelleştirme](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-update) komutunu kullanın. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubunda bulunan *myVMVMNic* adlı varolan bir ağ arabiriminin *ipconfigmyVM* adlı IP yapılandırmasına *myVMPublicIP* adlı varolan bir genel IP adresini ilişkilendirer.
  
   ```azurecli-interactive
   az network nic ip-config update \
     --name ipconfigmyVM \
     --nic-name myVMVMNic \
     --resource-group myResourceGroup \
     --public-ip-address myVMPublicIP
   ```

   - Varolan bir genel IP adresiniz yoksa, bir tane oluşturmak için [az ağı public-ip oluşturma](/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create) komutunu kullanın. Örneğin, aşağıdaki komut *myResourceGroup*adlı bir kaynak grubunda *myVMPublicIP* adlı genel bir IP adresi oluşturur.
  
     ```azurecli-interactive
     az network public-ip create --name myVMPublicIP --resource-group myResourceGroup
     ```

     > [!NOTE]
     > Önceki komut, özelleştirmek isteyebileceğin birkaç ayar için varsayılan değerlere sahip genel bir IP adresi oluşturur. Tüm genel IP adresi ayarları hakkında daha fazla bilgi edinmek için [bkz.](virtual-network-public-ip-address.md#create-a-public-ip-address) Adres, her Azure bölgesi için kullanılan genel IP adresleri havuzundan atanır. Her bölgede kullanılan adres havuzlarının listesini görmek için [Microsoft Azure Veri Merkezi IP Aralıkları'na](https://www.microsoft.com/download/details.aspx?id=41653)bakın.

   - VM'nize bağlı bir ağ arabiriminin adını bilmiyorsanız, bunları görüntülemek için [az vm nic liste](/cli/azure/vm/nic?view=azure-cli-latest#az-vm-nic-list) komutunu kullanın. Örneğin, aşağıdaki komut, *myResourceGroup*adlı bir kaynak grubunda *myVM* adlı bir VM'ye iliştirilen ağ arabirimlerinin adlarını listeler:

     ```azurecli-interactive
     az vm nic list --vm-name myVM --resource-group myResourceGroup
     ```

     Çıktı, aşağıdaki örneğe benzer bir veya daha fazla satır içerir:
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
     ```

     Önceki örnekte, *myVMVMNic* ağ arabiriminin adıdır.

   - Bir ağ arabirimi için IP yapılandırmasının adını bilmiyorsanız, bunları almak için [az network nic ip-config list](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-list) komutunu kullanın. Örneğin, aşağıdaki komut *myResourceGroup*adlı bir kaynak grubunda *myVMVMNic* adlı bir ağ arabiriminin IP yapılandırmalarının adlarını listeler:

     ```azurecli-interactive
     az network nic ip-config list --nic-name myVMVMNic --resource-group myResourceGroup --out table
     ```

3. [AZ vm list-ip adresleri](/cli/azure/vm?view=azure-cli-latest#az-vm-list-ip-addresses) komutuyla IP yapılandırmasına atanan genel IP adresini görüntüleyin. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubunda *myVM* adlı varolan bir VM'ye atanan IP adreslerini gösterir.

   ```azurecli-interactive
   az vm list-ip-addresses --name myVM --resource-group myResourceGroup --out table
   ```

   > [!NOTE]
   > Adres, her Azure bölgesinde kullanılan adresler havuzundan atanır. Her bölgede kullanılan adres havuzlarının listesini görmek için [Microsoft Azure Veri Merkezi IP Aralıkları'na](https://www.microsoft.com/download/details.aspx?id=41653)bakın. Atanan adres, bölge için kullanılan havuzlarda herhangi bir adres olabilir. Adresin bölgedeki belirli bir havuzdan atanması gerekiyorsa, [Genel IP adresi öneki](public-ip-address-prefix.md)kullanın.

4. Ağ güvenlik grubunda güvenlik kuralları yla [VM'ye ağ trafiğine izin verin.](#allow-network-traffic-to-the-vm)

## <a name="powershell"></a>PowerShell

[PowerShell'i](/powershell/azure/install-az-ps)yükleyin veya Azure Bulut Kabuğu'nu kullanın. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir kabuktur. PowerShell önceden yüklenmiş ve hesabınızla kullanmak üzere yapılandırılmıştır. Takip eden PowerShell komutlarında **Try it** düğmesini seçin. Try **it** seçeneğini seçmek, Azure hesabınızda oturum açabileceğiniz bir Bulut Bulutu'nu çağırır.

1. PowerShell'i yerel olarak kullanıyorsanız, `Connect-AzAccount`Azure'da 'ile oturum açın.
2. Ortak IP adresi, VM'ye bağlı bir ağ arabiriminin IP yapılandırması ile ilişkilidir. Ağ arabiriminin içinde olduğu sanal ağı ve alt ağı elde etmek için [Get-AzVirtualNetwork](/powershell/module/Az.Network/Get-AzVirtualNetwork) ve [Get-AzVirtualNetworkSubnetConfig](/powershell/module/Az.Network/Get-AzVirtualNetworkSubnetConfig) komutlarını kullanın. Ardından, varolan bir genel IP adresini almak için ağ arabirimi ve [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) komutunu almak için [Get-AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) komutunu kullanın. Ardından, yeni IP yapılandırmasını ağ arabirimine yazmak için ortak IP adresini IP yapılandırması ve [Set-AzNetworkInterface](/powershell/module/Az.Network/Set-AzNetworkInterface) komutuyla ilişkilendirmek için [Set-AzNetworkInterfaceIpConfig](/powershell/module/Az.Network/Set-AzNetworkInterfaceIpConfig) komutunu kullanın.

   Aşağıdaki örnek, *myVMVNet*adlı sanal bir ağda *myVMSubnet* adlı bir alt ağda bulunan *myVMVMNic* adlı varolan bir ağ arabiriminin *ipconfigmyVM* adlı IP yapılandırmasına *myVMPublicIP* adlı mevcut bir genel IP adresini ilişkilendirer. Tüm kaynaklar *myResourceGroup*adlı bir kaynak grubundadır.
  
   ```azurepowershell-interactive
   $vnet = Get-AzVirtualNetwork -Name myVMVNet -ResourceGroupName myResourceGroup
   $subnet = Get-AzVirtualNetworkSubnetConfig -Name myVMSubnet -VirtualNetwork $vnet
   $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
   $pip = Get-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup
   $nic | Set-AzNetworkInterfaceIpConfig -Name ipconfigmyVM -PublicIPAddress $pip -Subnet $subnet
   $nic | Set-AzNetworkInterface
   ```

   - Varolan bir genel IP adresiniz yoksa, bir tane oluşturmak için [New-AzPublicIpAddress](/powershell/module/Az.Network/New-AzPublicIpAddress) komutunu kullanın. Örneğin, aşağıdaki komut, *eastus* bölgesindeki *myResourceGroup* adlı bir kaynak grubunda *myVMPublicIP* adlı *dinamik* bir genel IP adresi oluşturur.
  
     ```azurepowershell-interactive
     New-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup -AllocationMethod Dynamic -Location eastus
     ```

     > [!NOTE]
     > Önceki komut, özelleştirmek isteyebileceğin birkaç ayar için varsayılan değerlere sahip genel bir IP adresi oluşturur. Tüm genel IP adresi ayarları hakkında daha fazla bilgi edinmek için [bkz.](virtual-network-public-ip-address.md#create-a-public-ip-address) Adres, her Azure bölgesi için kullanılan genel IP adresleri havuzundan atanır. Her bölgede kullanılan adres havuzlarının listesini görmek için [Microsoft Azure Veri Merkezi IP Aralıkları'na](https://www.microsoft.com/download/details.aspx?id=41653)bakın.

   - VM'nize bağlı bir ağ arabiriminin adını bilmiyorsanız, bunları görüntülemek için [Get-AzVM](/powershell/module/Az.Compute/Get-AzVM) komutunu kullanın. Örneğin, aşağıdaki komut, *myResourceGroup*adlı bir kaynak grubunda *myVM* adlı bir VM'ye iliştirilen ağ arabirimlerinin adlarını listeler:

     ```azurepowershell-interactive
     $vm = Get-AzVM -name myVM -ResourceGroupName myResourceGroup
     $vm.NetworkProfile
     ```

     Çıktı, aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda, *myVMVMNic* ağ arabiriminin adıdır.
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
     ```

   - Ağ arabiriminin içinde olduğu sanal ağın veya alt ağın adını `Get-AzNetworkInterface` bilmiyorsanız, bilgileri görüntülemek için komutu kullanın. Örneğin, aşağıdaki komut *myResourceGroup*adlı bir kaynak grubunda *myVMVMNic* adlı bir ağ arabirimi için sanal ağ ve alt ağ bilgilerini alır:

     ```azurepowershell-interactive
     $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
     $ipConfigs = $nic.IpConfigurations
     $ipConfigs.Subnet | Select Id
     ```

     Çıktı, aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda, *myVMVNET* sanal ağın adıdır ve *myVMSubnet* alt ağın adıdır.
  
     ```
     "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVMVNET/subnets/myVMSubnet",
     ```

   - Bir ağ arabirimi için IP yapılandırmasının adını bilmiyorsanız, bunları almak için [Get-AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) komutunu kullanın. Örneğin, aşağıdaki komut *myResourceGroup*adlı bir kaynak grubunda *myVMVMNic* adlı bir ağ arabiriminin IP yapılandırmalarının adlarını listeler:

     ```azurepowershell-interactive
     $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
     $nic.IPConfigurations
     ```

     Çıktı, aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda *ipconfigmyVM* bir IP yapılandırmasının adıdır.
  
     ```
     Id     : /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM
     ```

3. Ip yapılandırmasına atanan genel IP adresini [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) komutu yla görüntüleyin. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubunda *myVMPublicIP* adlı genel bir IP adresine atanan adresi gösterir.

   ```azurepowershell-interactive
   Get-AzPublicIpAddress -Name myVMPublicIP -ResourceGroupName myResourceGroup | Select IpAddress
   ```

   BIR IP yapılandırmasına atanan genel IP adresinin adını bilmiyorsanız, bunu almak için aşağıdaki komutları çalıştırın:

   ```azurepowershell-interactive
   $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
   $nic.IPConfigurations
   $address = $nic.IPConfigurations.PublicIpAddress
   $address | Select Id
   ```

   Çıktı, aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda, *myVMPublicIP* IP yapılandırmasına atanan genel IP adresinin adıdır.

   ```
   "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myVMPublicIP"
   ```

   > [!NOTE]
   > Adres, her Azure bölgesinde kullanılan adresler havuzundan atanır. Her bölgede kullanılan adres havuzlarının listesini görmek için [Microsoft Azure Veri Merkezi IP Aralıkları'na](https://www.microsoft.com/download/details.aspx?id=41653)bakın. Atanan adres, bölge için kullanılan havuzlarda herhangi bir adres olabilir. Adresin bölgedeki belirli bir havuzdan atanması gerekiyorsa, [Genel IP adresi öneki](public-ip-address-prefix.md)kullanın.

4. Ağ güvenlik grubunda güvenlik kuralları yla [VM'ye ağ trafiğine izin verin.](#allow-network-traffic-to-the-vm)

## <a name="allow-network-traffic-to-the-vm"></a>VM'ye ağ trafiğine izin verme

Internet'ten genel IP adresine bağlanmadan önce, ağ arabirimiyle ilişkili olabileceğiniz herhangi bir ağ güvenlik grubunda, ağ arabiriminin içinde bulunan alt ağda veya her ikisinde de gerekli bağlantı noktalarının açık olduğundan emin olun. Güvenlik grupları trafiği ağ arabiriminin özel IP adresine filtrelese de, gelen internet trafiği genel IP adresine ulaştığında, Azure ortak adresi özel IP adresine çevirir, böylece bir ağ güvenlik grubu trafik akışı, genel IP adresi ile iletişim başarısız olur. [Portal,](diagnose-network-traffic-filter-problem.md#diagnose-using-azure-portal) [CLI](diagnose-network-traffic-filter-problem.md#diagnose-using-azure-cli)veya [PowerShell'i](diagnose-network-traffic-filter-problem.md#diagnose-using-powershell)kullanarak ağ arabirimi ve alt ağı için etkili güvenlik kurallarını görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir ağ güvenlik grubuyla VM'nize gelen internet trafiğine izin verin. Ağ güvenlik grubu oluşturmayı öğrenmek için [bkz.](manage-network-security-group.md#work-with-network-security-groups) Ağ güvenlik grupları hakkında daha fazla bilgi edinmek için [Güvenlik gruplarına](security-overview.md)bakın.
