---
title: 'Rota tabanlı Azure VPN Ağ Geçidi oluşturma: CLI'
description: CLI kullanarak VPN Ağ Geçidi oluşturmayı hızlı bir şekilde öğrenin
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 10/04/2018
ms.author: cherylmc
ms.openlocfilehash: 121790fce220874babedf67cd72471caa7e92ae6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80241097"
---
# <a name="create-a-route-based-vpn-gateway-using-cli"></a>CLI kullanarak rota tabanlı VPN ağ geçidi oluşturma

Bu makale, Azure CLI'yi kullanarak rota tabanlı bir Azure VPN ağ geçidi oluşturmanıza yardımcı olur. Şirket içi ağınıza VPN bağlantısı oluşturulurken VPN ağ geçidi kullanılır. VNets'i bağlamak için vpn ağ geçidi de kullanabilirsiniz.

Bu makaledeki adımlar bir VNet, bir alt ağ, ağ geçidi alt ağı ve rota tabanlı VPN ağ geçidi (sanal ağ ağ geçidi) oluşturur. Sanal ağ ağ geçidinin oluşturulması 45 dakika veya daha uzun sürebilir. Ağ geçidi oluşturma tamamlandıktan sonra, bağlantılar oluşturabilirsiniz. Bu adımlar için Azure aboneliği gerekir. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

az grubu oluşturma komutunu kullanarak bir kaynak grubu [oluşturun.](/cli/azure/group) Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 


```azurecli-interactive
az group create --name TestRG1 --location eastus
```

## <a name="create-a-virtual-network"></a><a name="vnet"></a>Sanal ağ oluşturma

[az ağ vnet oluşturma](/cli/azure/network/vnet) komutunu kullanarak sanal ağ oluşturun. Aşağıdaki örnek, **EastUS** konumunda **VNet1** adlı bir sanal ağ oluşturur:

```azurecli-interactive
az network vnet create \
  -n VNet1 \
  -g TestRG1 \
  -l eastus \
  --address-prefix 10.1.0.0/16 \
  --subnet-name Frontend \
  --subnet-prefix 10.1.0.0/24
```

## <a name="add-a-gateway-subnet"></a><a name="gwsubnet"></a>Ağ geçidi alt ağı ekleme

Ağ geçidi alt ağı, sanal ağ ağ geçidi hizmetlerinin kullandığı ayrılmış IP adreslerini içerir. Ağ geçidi alt ağı eklemek için aşağıdaki örnekleri kullanın:

```azurecli-interactive
az network vnet subnet create \
  --vnet-name VNet1 \
  -n GatewaySubnet \
  -g TestRG1 \
  --address-prefix 10.1.255.0/27 
```

## <a name="request-a-public-ip-address"></a><a name="PublicIP"></a>Herkese açık bir IP adresi isteme

VPN ağ geçidinin dinamik olarak ayrılmış genel BIR IP adresi olmalıdır. Genel IP adresi, sanal ağınız için oluşturduğunuz VPN ağ geçidine tahsis edilir. Genel bir IP adresi istemek için aşağıdaki örneği kullanın:

```azurecli-interactive
az network public-ip create \
  -n VNet1GWIP \
  -g TestRG1 \
  --allocation-method Dynamic 
```

## <a name="create-the-vpn-gateway"></a><a name="CreateGateway"></a>VPN ağ geçidini oluşturma

[az network vnet-gateway create](/cli/azure/group) komutunu kullanarak VPN ağ geçidini oluşturun.

Bu komutu `--no-wait` parametreyi kullanarak çalıştırın, herhangi bir geri bildirim veya çıktı görmezsiniz. Parametre ağ `--no-wait` geçidinin arka planda oluşturulmasına izin verir. Bu VPN ağ geçidi hemen oluşturulur anlamına gelmez.

```azurecli-interactive
az network vnet-gateway create \
  -n VNet1GW \
  -l eastus \
  --public-ip-address VNet1GWIP \
  -g TestRG1 \
  --vnet VNet1 \
  --gateway-type Vpn \
  --sku VpnGw1 \
  --vpn-type RouteBased \
  --no-wait
```

VPN ağ geçidinin oluşturulması 45 dakika veya daha uzun sürebilir.

## <a name="view-the-vpn-gateway"></a><a name="viewgw"></a>VPN ağ geçidini görüntüleme

```azurecli-interactive
az network vnet-gateway show \
  -n VNet1GW \
  -g TestRG1
```

Yanıt şuna benzer:

```output
{
  "activeActive": false,
  "bgpSettings": null,
  "enableBgp": false,
  "etag": "W/\"6c61f8cb-d90f-4796-8697\"",
  "gatewayDefaultSite": null,
  "gatewayType": "Vpn",
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW",
  "ipConfigurations": [
    {
      "etag": "W/\"6c61f8cb-d90f-4796-8697\"",
      "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/vnetGatewayConfig0",
      "name": "vnetGatewayConfig0",
      "privateIpAllocationMethod": "Dynamic",
      "provisioningState": "Updating",
      "publicIpAddress": {
        "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP",
        "resourceGroup": "TestRG1"
      },
      "resourceGroup": "TestRG1",
      "subnet": {
        "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG11/providers/Microsoft.Network/virtualNetworks/VNet1/subnets/GatewaySubnet",
        "resourceGroup": "TestRG1"
      }
    }
  ],
  "location": "eastus",
  "name": "VNet1GW",
  "provisioningState": "Updating",
  "resourceGroup": "TestRG1",
  "resourceGuid": "69c269e3-622c-4123-9231",
  "sku": {
    "capacity": 2,
    "name": "VpnGw1",
    "tier": "VpnGw1"
  },
  "tags": null,
  "type": "Microsoft.Network/virtualNetworkGateways",
  "vpnClientConfiguration": null,
  "vpnType": "RouteBased"
}
```

### <a name="view-the-public-ip-address"></a>Genel IP adresini görüntüleme

Ağ geçidinize atanan genel IP adresini görüntülemek için aşağıdaki örneği kullanın:

```azurecli-interactive
az network public-ip show \
  --name VNet1GWIP \
  --resource-group TestRG11
```

**ipAddress** alanıyla ilişkili değer, VPN ağ geçidinizin genel IP adresidir.

Örnek yanıt:

```output
{
  "dnsSettings": null,
  "etag": "W/\"a12d4d03-b27a-46cc-b222-8d9364b8166a\"",
  "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/publicIPAddresses/VNet1GWIP",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "13.90.195.184",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/<subscription ID>/resourceGroups/TestRG1/providers/Microsoft.Network/virtualNetworkGateways/VNet1GW/ipConfigurations/vnetGatewayConfig0",
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında, kaynak grubunu silmek için [az group delete'i](/cli/azure/group) kullanın. Böylece, kaynak grubu ve içerdiği tüm kaynaklar silinir.

```azurecli-interactive 
az group delete --name TestRG1 --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Ağ geçidi oluşturma yı bitirdikten sonra, sanal ağınızla başka bir VNet arasında bağlantı oluşturabilirsiniz. Veya sanal ağınızla şirket içi konumunuz arasında bir bağlantı kurun.

> [!div class="nextstepaction"]
> [Siteden siteye bağlantı oluşturma](vpn-gateway-create-site-to-site-rm-powershell.md)<br><br>
> [Noktadan siteye bağlantı oluşturma](vpn-gateway-howto-point-to-site-rm-ps.md)<br><br>
> [Başka bir VNet'e bağlantı oluşturma](vpn-gateway-vnet-vnet-rm-ps.md)
