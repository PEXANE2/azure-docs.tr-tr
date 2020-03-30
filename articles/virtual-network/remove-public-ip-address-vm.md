---
title: Azure VM'den herkese açık bir IP adresini ayırma
titlesuffix: Azure Virtual Network
description: Genel bir IP adresini VM'den nasıl ayırılamayı öğrenin
services: virtual-network
documentationcenter: ''
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/04/2019
ms.author: kumud
ms.openlocfilehash: 1c27af30f97ea967d170b2cccaefb2e95f8fedaf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900754"
---
# <a name="dissociate-a-public-ip-address-from-an-azure-vm"></a>Azure VM'den herkese açık bir IP adresini ayırma 

Bu makalede, ortak bir IP adresini bir Azure sanal makinesinden (VM) nasıl ayırabileceğinizi öğrenirsiniz.

Ortak bir IP adresini VM'den ayrıştırmak için [Azure portalını](#azure-portal), Azure [komut satırı arabirimini](#azure-cli) (CLI) veya [PowerShell'i](#powershell) kullanabilirsiniz.

## <a name="azure-portal"></a>Azure portalında

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Ortak IP adresini ayırmak istediğiniz sanal makineye göz atın veya arama yapın ve sonra seçin.
3. VM sayfasında Genel **Bakış'ı**seçin, aşağıdaki resimde gösterildiği gibi genel IP adresini seçin:

   ![Genel IP'yi seçin](./media/remove-public-ip-address/remove-public-ip-address-2.png)

4. Genel IP adresi sayfasında Genel **Bakış'ı**seçin ve ardından aşağıdaki resimde gösterildiği gibi **Dissociate'yi**seçin:

    ![Ayrıştırık Kamu IP](./media/remove-public-ip-address/remove-public-ip-address-3.png)

5. **Dissociate genel IP adresi,** **Evet**seçin .

## <a name="azure-cli"></a>Azure CLI

Azure [CLI'yi](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json)yükleyin veya Azure Bulut Kabuğu'nu kullanın. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir Bash kabuğudur. Azure CLI, kabuğa önceden yüklenmiştir ve kabuk, hesabınızla birlikte kullanılacak şekilde yapılandırılmıştır. Bunu izleyen CLI komutlarında **Try it** düğmesini seçin. Try **it** seçeneğini seçmek, Azure hesabınızda oturum açabileceğiniz bir Bulut Bulutu'nu çağırır.

1. CLI'yi Bash'te yerel olarak kullanıyorsanız, Azure'da '' ile `az login`oturum açın.
2. Ortak IP adresi, VM'ye bağlı bir ağ arabiriminin IP yapılandırması ile ilişkilidir. Bir IP yapılandırmasından ortak bir IP adresini ayrıştırmak için [az ağ nic-ip-config güncelleştirme](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-update) komutunu kullanın. Aşağıdaki örnek, *myResourceGroup*adlı bir kaynak grubunda *myVM* adlı bir VM'e bağlı *myVMVMNic* adlı varolan bir ağ arabiriminin *ipconfigmyVM* adlı IP yapılandırmasından *myVMPublicIP* adlı genel bir IP adresini ayırıyor.
  
   ```azurecli-interactive
    az network nic ip-config update \
    --name ipconfigmyVM \
    --resource-group myResourceGroup \
    --nic-name myVMVMNic \
    --remove PublicIpAddress
   ```

   VM'nize bağlı bir ağ arabiriminin adını bilmiyorsanız, bunları görüntülemek için [az vm nic liste](/cli/azure/vm/nic?view=azure-cli-latest#az-vm-nic-list) komutunu kullanın. Örneğin, aşağıdaki komut, *myResourceGroup*adlı bir kaynak grubunda *myVM* adlı bir VM'ye iliştirilen ağ arabirimlerinin adlarını listeler:

     ```azurecli-interactive
     az vm nic list --vm-name myVM --resource-group myResourceGroup
     ```

     Çıktı, aşağıdaki örneğe benzer bir veya daha fazla satır içerir:
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
     ```

     Önceki örnekte, *myVMVMNic* ağ arabiriminin adıdır.

   - Bir ağ arabirimi için IP yapılandırmasının adını bilmiyorsanız, bunları almak için [az network nic ip-config list](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-list) komutunu kullanın. Örneğin, aşağıdaki komut *myResourceGroup*adlı bir kaynak grubunda *myVMVMNic* adlı bir ağ arabirimi için ortak IP yapılandırmalarının adlarını listeler:

     ```azurecli-interactive
     az network nic ip-config list --nic-name myVMVMNic --resource-group myResourceGroup --out table
     ```

   - Bir ağ arabirimi için genel bir IP yapılandırmasının adını bilmiyorsanız, bunları almak için [az network nic ip-config show](/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-show) komutunu kullanın. Örneğin, aşağıdaki komut *myResourceGroup*adlı bir kaynak grubunda *myVMVMNic* adlı bir ağ arabirimi için ortak IP yapılandırmalarının adlarını listeler:

     ```azurecli-interactive
     az network nic ip-config show --name ipconfigmyVM --nic-name myVMVMNic --resource-group myResourceGroup --query publicIPAddress.id
     ```


## <a name="powershell"></a>PowerShell

[PowerShell'i](/powershell/azure/install-az-ps)yükleyin veya Azure Bulut Kabuğu'nu kullanın. Azure Cloud Shell doğrudan Azure portalının içinde çalıştırabileceğiniz ücretsiz bir kabuktur. PowerShell önceden yüklenmiş ve hesabınızla kullanmak üzere yapılandırılmıştır. Takip eden PowerShell komutlarında **Try it** düğmesini seçin. Try **it** seçeneğini seçmek, Azure hesabınızda oturum açabileceğiniz bir Bulut Bulutu'nu çağırır.

1. PowerShell'i yerel olarak kullanıyorsanız, `Connect-AzAccount`Azure'da 'ile oturum açın.
2. Ortak IP adresi, VM'ye bağlı bir ağ arabiriminin IP yapılandırması ile ilişkilidir. Ağ arabirimi almak için [Get-AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) komutunu kullanın. Genel IP adresi değerini null olarak ayarlayın ve ardından yeni IP yapılandırmasını ağ arabirimine yazmak için [Set-AzNetworkInterface](/powershell/module/Az.Network/Set-AzNetworkInterface) komutunu kullanın.

   Aşağıdaki örnek, *myVMPublicIP* adlı bir ağ arabiriminden *myVMPublicIP* adlı genel bir IP adresini ayırıyor ve *myVM*adlı bir VM'ye iliştirilir. Tüm kaynaklar *myResourceGroup*adlı bir kaynak grubundadır.
  
   ```azurepowershell
    $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroup myResourceGroup
    $nic.IpConfigurations.publicipaddress.id = $null
    Set-AzNetworkInterface -NetworkInterface $nic
   ```

  - VM'nize bağlı bir ağ arabiriminin adını bilmiyorsanız, bunları görüntülemek için [Get-AzVM](/powershell/module/Az.Compute/Get-AzVM) komutunu kullanın. Örneğin, aşağıdaki komut, *myResourceGroup*adlı bir kaynak grubunda *myVM* adlı bir VM'ye iliştirilen ağ arabirimlerinin adlarını listeler:

    ```azurepowershell
    $vm = Get-AzVM -name myVM -ResourceGroupName myResourceGroup
    $vm.NetworkProfile
    ```

     Çıktı, aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda, *myVMVMNic* ağ arabiriminin adıdır.
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic",
     ```

   - Bir ağ arabirimi için IP yapılandırmasının adını bilmiyorsanız, bunları almak için [Get-AzNetworkInterface](/powershell/module/Az.Network/Get-AzNetworkInterface) komutunu kullanın. Örneğin, aşağıdaki komut *myResourceGroup*adlı bir kaynak grubunda *myVMVMNic* adlı bir ağ arabiriminin IP yapılandırmalarının adlarını listeler:

     ```azurepowershell-interactive
     $nic = Get-AzNetworkInterface -Name myVMVMNic -ResourceGroupName myResourceGroup
     $nic.IPConfigurations.id
     ```

     Çıktı, aşağıdaki örneğe benzer bir veya daha fazla satır içerir. Örnek çıktıda *ipconfigmyVM* bir IP yapılandırmasının adıdır.
  
     ```
     "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkInterfaces/myVMVMNic/ipConfigurations/ipconfigmyVM"
     ```

## <a name="next-steps"></a>Sonraki adımlar

- [Ortak bir IP adresini VM](associate-public-ip-address-vm.md)ile nasıl ilişkilendireceklerini öğrenin.
