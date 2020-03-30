---
title: Azure sanal ağında bir alt ağ delegasyonu ekleme veya kaldırma
titlesuffix: Azure Virtual Network
description: Azure'daki bir hizmet için devralınan bir alt ağı nasıl ekleyeceğinizi veya kaldırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2019
ms.author: kumud
ms.openlocfilehash: 6f767abdf8673e3adffc6c4e3748733054ba723d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77201875"
---
# <a name="add-or-remove-a-subnet-delegation"></a>Bir alt ağ delegasyonu ekleme veya kaldırma

Alt ağ delegasyonu, hizmeti dağıtırken benzersiz bir tanımlayıcı kullanarak alt ağda hizmete özgü kaynaklar oluşturmak için hizmete açık izinler verir. Bu makalede, bir Azure hizmeti için devralınan bir alt ağ nasıl ekleyeceğiniz veya kaldırılacak şekilde açıklanmaktadır.

## <a name="portal"></a>Portal

### <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

### <a name="create-the-virtual-network"></a>Sanal ağ oluşturma

Bu bölümde, daha sonra bir Azure hizmetine devreceğiniz bir sanal ağ ve alt ağ oluşturursunuz.

1. Ekranın sol üst tarafında,**Networking** > **Sanal ağ**ağı ağı **oluştur'u** > seçin.
1. **Sanal ağ oluştur'da**bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *MyVirtualNetwork*girin. |
    | Adres alanı | *10.0.0.0/16*girin. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **Yeni Oluştur'u**seçin, *myResourceGroup'u*girin, ardından **Tamam'ı**seçin. |
    | Konum | **EastUS'u**seçin.|
    | Subnet - Adı | *mySubnet*girin. |
    | Alt Ağ - Adres aralığı | *10.0.0.0/24*girin. |
    |||
1. Geri sini varsayılan olarak bırakın ve ardından **Oluştur'u**seçin.

### <a name="permissions"></a>İzinler

Bir Azure hizmetine devretmek istediğiniz alt ağı oluşturmadıysanız, aşağıdaki izinlere `Microsoft.Network/virtualNetworks/subnets/write`ihtiyacınız vardır: .

Yerleşik Ağ [Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü de gerekli izinleri içerir.

### <a name="delegate-a-subnet-to-an-azure-service"></a>Bir alt ağı Azure hizmetine temsilci olarak

Bu bölümde, önceki bölümde oluşturduğunuz alt ağı bir Azure hizmetine devresiniz.

1. Portalın arama çubuğuna *myVirtualNetwork*girin. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin.
2. Arama sonuçlarında *myVirtualNetwork'u*seçin.
3. **AYARLAR**altında **Alt Ağlar'ı**seçin ve sonra **mySubnet'i**seçin.
4. *mySubnet* sayfasında, **Subnet temsilci listesi** **için, Temsilci alt ağı** altında listelenen hizmetlerden bir hizmete (örneğin, **Microsoft.DBforPostgreSQL/serversv2)** seçin.  

### <a name="remove-subnet-delegation-from-an-azure-service"></a>Azure hizmetinden alt ağ delegasyonu kaldırma

1. Portalın arama çubuğuna *myVirtualNetwork*girin. Arama sonuçlarında **myVirtualNetwork** göründüğünde seçin.
2. Arama sonuçlarında *myVirtualNetwork'u*seçin.
3. **AYARLAR**altında **Alt Ağlar'ı**seçin ve sonra **mySubnet'i**seçin.
4. *Subnet sayfamda,* **Subnet temsilci listesi** için, Temsilci alt ağı altında listelenen hizmetlerden **bir hizmete** **Yok'u** seçin. 

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI'yi yerel olarak yüklemeye ve kullanmaya karar verirseniz, bu makalede Azure CLI sürüm 2.0.28 veya sonraki sürümlerini kullanmanız gerekir. Yüklü sürümünüzü bulmak için `az --version`çalıştırın. Bilgileri yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli) bakın.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[az group create](https://docs.microsoft.com/cli/azure/group) ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek, **eastus** konumda **myResourceGroup** adlı bir kaynak grubu oluşturur:

```azurecli-interactive

  az group create \
    --name myResourceGroup \
    --location eastus

```

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma
[az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) komutunu kullanarak **myResourceGroup** içinde **mySubnet** adlı bir alt ağ ile **myVnet** adlı bir sanal ağ oluşturun.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.0.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.0.0.0/24
```
### <a name="permissions"></a>İzinler

Bir Azure hizmetine devretmek istediğiniz alt ağı oluşturmadıysanız, aşağıdaki izinlere `Microsoft.Network/virtualNetworks/subnets/write`ihtiyacınız vardır: .

Yerleşik Ağ [Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü de gerekli izinleri içerir.

### <a name="delegate-a-subnet-to-an-azure-service"></a>Bir alt ağı Azure hizmetine temsilci olarak

Bu bölümde, önceki bölümde oluşturduğunuz alt ağı bir Azure hizmetine devresiniz. 

**mySubnet** adlı alt ağı bir Azure hizmetine bir temsilciyle güncellemek için [az network vnet subnet güncelleştirmesini](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update) kullanın.  Bu örnekte **Microsoft.DBforPostgreSQL/serversv2** örnek delegasyon için kullanılır:

```azurecli-interactive
  az network vnet subnet update \
  --resource-group myResourceGroup \
  --name mySubnet \
  --vnet-name myVnet \
  --delegations Microsoft.DBforPostgreSQL/serversv2
```

Delegasyonun uygulandığını doğrulamak için [az network vnet subnet show'u](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-show)kullanın. Hizmetin özellik **hizmetiAdı**altında alt ağa devrediliş olduğunu doğrulayın:

```azurecli-interactive
  az network vnet subnet show \
  --resource-group myResourceGroup \
  --name mySubnet \
  --vnet-name myVnet \
  --query delegations
```

```json
[
  {
    "actions": [
      "Microsoft.Network/virtualNetworks/subnets/join/action"
    ],
    "etag": "W/\"8a8bf16a-38cf-409f-9434-fe3b5ab9ae54\"",
    "id": "/subscriptions/3bf09329-ca61-4fee-88cb-7e30b9ee305b/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet/delegations/0",
    "name": "0",
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "serviceName": "Microsoft.DBforPostgreSQL/serversv2",
    "type": "Microsoft.Network/virtualNetworks/subnets/delegations"
  }
]
```

### <a name="remove-subnet-delegation-from-an-azure-service"></a>Azure hizmetinden alt ağ delegasyonu kaldırma

**MySubnet**adlı alt ağden delegasyonu kaldırmak için [az network vnet subnet güncelleştirmesini](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update) kullanın:

```azurecli-interactive
  az network vnet subnet update \
  --resource-group myResourceGroup \
  --name mySubnet \
  --vnet-name myVnet \
  --remove delegations
```
Delegasyonun kaldırıldığını doğrulamak için [az network vnet subnet show'u](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-show)kullanın. Hizmetin özellik **hizmetiAdı**altında alt ağdan kaldırıldığını doğrulayın:

```azurecli-interactive
  az network vnet subnet show \
  --resource-group myResourceGroup \
  --name mySubnet \
  --vnet-name myVnet \
  --query delegations
```
Komuttan çıkış null bir parantezdir:
```json
[]
```

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="connect-to-azure"></a>Azure'a Bağlanma

```azurepowershell-interactive
  Connect-AzAccount
```

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
[New-AzResourceGroup](https://docs.microsoft.com/cli/azure/group)ile bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurepowershell-interactive
  New-AzResourceGroup -Name myResourceGroup -Location eastus
```
### <a name="create-virtual-network"></a>Sanal ağ oluşturma

**MyResourceGroup'ta** [New-AzVirtualNetworkConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) kullanarak **mySubnet** adlı bir alt ağı olan **myVnet** adında bir sanal ağ oluşturun. [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest) Sanal ağ için IP adresi alanı **10.0.0.0/16'dır.** Sanal ağ içindeki alt ağ **10.0.0.0/24'dür.**  

```azurepowershell-interactive
  $subnet = New-AzVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix "10.0.0.0/24"

  New-AzVirtualNetwork -Name myVnet -ResourceGroupName myResourceGroup -Location eastus -AddressPrefix "10.0.0.0/16" -Subnet $subnet
```
### <a name="permissions"></a>İzinler

Bir Azure hizmetine devretmek istediğiniz alt ağı oluşturmadıysanız, aşağıdaki izinlere `Microsoft.Network/virtualNetworks/subnets/write`ihtiyacınız vardır: .

Yerleşik Ağ [Katılımcısı](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) rolü de gerekli izinleri içerir.

### <a name="delegate-a-subnet-to-an-azure-service"></a>Bir alt ağı Azure hizmetine temsilci olarak

Bu bölümde, önceki bölümde oluşturduğunuz alt ağı bir Azure hizmetine devresiniz. 

**MySubnet** adlı alt ağı bir Azure hizmetine **myDelegation** adlı bir temsilciyle güncelleştirmek için [Add-AzDelegation'ı](https://docs.microsoft.com/powershell/module/az.network/add-azdelegation?view=latest) kullanın.  Bu örnekte **Microsoft.DBforPostgreSQL/serversv2** örnek delegasyon için kullanılır:

```azurepowershell-interactive
  $vnet = Get-AzVirtualNetwork -Name "myVNet" -ResourceGroupName "myResourceGroup"
  $subnet = Get-AzVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $vnet
  $subnet = Add-AzDelegation -Name "myDelegation" -ServiceName "Microsoft.DBforPostgreSQL/serversv2" -Subnet $subnet
  Set-AzVirtualNetwork -VirtualNetwork $vnet
```
Temsilciliği doğrulamak için [Get-AzDelegation'ı](https://docs.microsoft.com/powershell/module/az.network/get-azdelegation?view=latest) kullanın:

```azurepowershell-interactive
  $subnet = Get-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup" | Get-AzVirtualNetworkSubnetConfig -Name "mySubnet"
  Get-AzDelegation -Name "myDelegation" -Subnet $subnet

  ProvisioningState : Succeeded
  ServiceName       : Microsoft.DBforPostgreSQL/serversv2
  Actions           : {Microsoft.Network/virtualNetworks/subnets/join/action}
  Name              : myDelegation
  Etag              : W/"9cba4b0e-2ceb-444b-b553-454f8da07d8a"
  Id                : /subscriptions/3bf09329-ca61-4fee-88cb-7e30b9ee305b/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet/delegations/myDelegation

```
### <a name="remove-subnet-delegation-from-an-azure-service"></a>Azure hizmetinden alt ağ delegasyonu kaldırma

**mySubnet**adlı alt ağdan delegasyonu kaldırmak için [Remove-AzDelegation](https://docs.microsoft.com/powershell/module/az.network/remove-azdelegation?view=latest) kullanın:

```azurepowershell-interactive
  $vnet = Get-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
  $subnet = Get-AzVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $vnet
  $subnet = Remove-AzDelegation -Name "myDelegation" -Subnet $subnet
  Set-AzVirtualNetwork -VirtualNetwork $vnet
```
Temsilcinin kaldırıldığını doğrulamak için [Get-AzDelegation'ı](https://docs.microsoft.com/powershell/module/az.network/get-azdelegation?view=latest) kullanın:

```azurepowershell-interactive
  $subnet = Get-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup" | Get-AzVirtualNetworkSubnetConfig -Name "mySubnet"
  Get-AzDelegation -Name "myDelegation" -Subnet $subnet

  Get-AzDelegation: Sequence contains no matching element

```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure'da alt ağları nasıl yönetirize](virtual-network-manage-subnet.md)edin.
