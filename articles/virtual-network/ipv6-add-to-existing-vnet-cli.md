---
title: Azure sanal ağındaki bir IPv4 uygulamasına IPv6 ekleme - Azure CLI
titlesuffix: Azure Virtual Network
description: Bu makalede, IPv6 adreslerinin Azure CLI kullanarak Azure sanal ağındaki varolan bir uygulamaya nasıl dağıtılanıncaya kadar dağıtılanın.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/23/2019
ms.author: kumud
ms.openlocfilehash: 5dc231febc2e9b605b9e7f603f5d036b8a2c62eb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240751"
---
# <a name="add-ipv6-to-an-ipv4-application-in-azure-virtual-network---azure-cli-preview"></a>Azure sanal ağındaki bir IPv4 uygulamasına IPv6 ekleme - Azure CLI (Önizleme)

Bu makalede, Azure CLI kullanan bir Standart Yük Dengeleyicisi için Bir Azure sanal ağında IPv4 genel IP adresini kullanan bir uygulamaya IPv6 adresleri nasıl ekleyeceğiniz gösterilmektedir. Yerinde yükseltme sanal ağ ve alt ağ, IPv4 + IPV6 ön uç yapılandırmaları ile standart yük dengeleyici, IPv4 + IPv6 yapılandırmaları, ağ güvenlik grubu ve ortak IP'ler olan NIC'ler içerir.

> [!Important]
> Azure Sanal Ağı için IPv6 desteği şu anda genel önizlemededir. Bu önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI'yi yerel olarak yüklemeye ve kullanmaya karar verirseniz, bu hızlı başlangıç Azure CLI sürüm 2.0.28 veya sonraki sürümlerini kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için `az --version`çalıştırın. Bilgileri yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli) bakın.

## <a name="prerequisites"></a>Ön koşullar

### <a name="register-the-service"></a>Hizmeti kaydedin

Azure'da bir çift yığın uygulaması dağıtmadan önce, aboneliğinizi bu önizleme özelliği için aşağıdaki Azure CLI'yi kullanarak yapılandırmanız gerekir:

```azurecli
az feature register --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
az feature register --name AllowIPv6CAOnStandardLB --namespace Microsoft.Network
```

Özellik kaydının tamamlanması 30 dakika kadar sürer. Aşağıdaki Azure CLI komutunu çalıştırarak kayıt durumunuzu kontrol edebilirsiniz:

```azurecli
az feature show --name AllowIPv6VirtualNetwork --namespace Microsoft.Network
az feature show --name AllowIPv6CAOnStandardLB --namespace Microsoft.Network
```

Kayıt tamamlandıktan sonra aşağıdaki komutu çalıştırın:

```azurecli
az provider register --namespace Microsoft.Network
```

### <a name="create-a-standard-load-balancer"></a>Standart Yük Dengeleyici oluşturma
Bu makalede, [Quickstart: Create a Standard Load Balancer - Azure CLI'de](../load-balancer/quickstart-load-balancer-standard-public-cli.md)açıklandığı gibi bir Standart Yük Dengeleyicisi dağıtıldığınızı varsayar.

## <a name="create-ipv6-addresses"></a>IPv6 adresleri oluşturma

Standart Yük Dengeleyiciniz için [az ağ public-ip oluşturma](/cli/azure/network/public-ip) ile ortak IPv6 adresi oluşturun. Aşağıdaki örnek, *myResourceGroupSLB* kaynak grubunda *PublicIP_v6* adlı bir IPv6 genel IP adresi oluşturur:

```azurecli
  
az network public-ip create \
--name PublicIP_v6 \
--resource-group MyResourceGroupSLB \
--location EastUS \
--sku Standard \
--allocation-method static \
--version IPv6
```

## <a name="configure-ipv6-load-balancer-frontend"></a>IPv6 yük dengeleyici ön uç yapılandırma

COnfigure az [ağ lb frontend-ip](https://docs.microsoft.com/cli/azure/network/lb/frontend-ip?view=azure-cli-latest#az-network-lb-frontend-ip-create) kullanarak yeni IPv6 IP adresi ile yük dengeleyici aşağıdaki gibi oluşturun:

```azurecli
az network lb frontend-ip create \
--lb-name myLoadBalancer \
--name dsLbFrontEnd_v6 \
--resource-group MyResourceGroupSLB \
--public-ip-address PublicIP_v6
```

## <a name="configure-ipv6-load-balancer-backend-pool"></a>IPv6 yük dengeleyici arka uç havuzuyapıla

[Az network lb adresi havuzunu](https://docs.microsoft.com/cli/azure/network/lb/address-pool?view=azure-cli-latest#az-network-lb-address-pool-create) kullanarak IPv6 adreslerine sahip NIC'ler için arka uç havuzunu aşağıdaki gibi oluşturun:

```azurecli
az network lb address-pool create \
--lb-name myLoadBalancer \
--name dsLbBackEndPool_v6 \
--resource-group MyResourceGroupSLB
```

## <a name="configure-ipv6-load-balancer-rules"></a>IPv6 yük dengeleyici kurallarını yapılandırma

Az ağ lb kuralı oluşturmak ile IPv6 yük dengeleyici kuralları [oluşturun.](https://docs.microsoft.com/cli/azure/network/lb/rule?view=azure-cli-latest#az-network-lb-rule-create)

```azurecli
az network lb rule create \
--lb-name myLoadBalancer \
--name dsLBrule_v6 \
--resource-group MyResourceGroupSLB \
--frontend-ip-name dsLbFrontEnd_v6 \
--protocol Tcp \
--frontend-port 80 \
--backend-port 80 \
--backend-pool-name dsLbBackEndPool_v6
```

## <a name="add-ipv6-address-ranges"></a>IPv6 adres aralıkları ekleme

IPv6 adres aralıklarını sanal ağa ve yük dengeleyicisini barındıran alt ağa aşağıdaki gibi ekleyin:

```azurecli
az network vnet update \
--name myVnet  `
--resource-group MyResourceGroupSLB \
--address-prefixes  "10.0.0.0/16"  "ace:cab:deca::/48"

az network vnet subnet update \
--vnet-name myVnet \
--name mySubnet \
--resource-group MyResourceGroupSLB \
--address-prefixes  "10.0.0.0/24"  "ace:cab:deca:deed::/64"  
```

## <a name="add-ipv6-configuration-to-nics"></a>NIC'lere IPv6 yapılandırması ekleme

[Az network nic ip-config kullanarak](https://docs.microsoft.com/cli/azure/network/nic/ip-config?view=azure-cli-latest#az-network-nic-ip-config-create) VM NIC'leri IPv6 adresiyle yapılandırın:

```azurecli
az network nic ip-config create \
--name dsIp6Config_NIC1 \
--nic-name myNicVM1 \
--resource-group MyResourceGroupSLB \
--vnet-name myVnet \
--subnet mySubnet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name dsLB

az network nic ip-config create \
--name dsIp6Config_NIC2 \
--nic-name myNicVM2 \
--resource-group MyResourceGroupSLB \
--vnet-name myVnet \
--subnet mySubnet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name myLoadBalancer

az network nic ip-config create \
--name dsIp6Config_NIC3 \
--nic-name myNicVM3 \
--resource-group MyResourceGroupSLB \
--vnet-name myVnet \
--subnet mySubnet \
--private-ip-address-version IPv6 \
--lb-address-pools dsLbBackEndPool_v6 \
--lb-name myLoadBalancer

```

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Azure portalında IPv6 çift yığın sanal ağı görüntüleyin
Azure portalındaki IPv6 çift yığın sanal ağını aşağıdaki gibi görüntüleyebilirsiniz:
1. Portalın arama çubuğuna *myVnet*girin.
2. **myVnet** arama sonuçlarında göründüğünde, onu seçin. Bu *myVNet*adlı çift yığını sanal ağın **Genel Bakış** sayfası başlattı. Çift yığın sanal ağ *mySubnet*adlı çift yığın alt ağında bulunan hem IPv4 ve IPv6 yapılandırmaları ile üç NIC gösterir.

  ![Azure'da IPv6 çift yığın sanal ağ](./media/ipv6-add-to-existing-vnet-powershell/ipv6-dual-stack-vnet.png)

> [!NOTE]
> Azure için IPv6 sanal ağı, bu önizleme sürümü için salt okunur olarak Azure portalında kullanılabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, VM'i ve ilgili tüm kaynakları kaldırmak için [Kaldır-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) komutunu kullanabilirsiniz.

```azurepowershell-interactive
Remove-AzResourceGroup -Name MyAzureResourceGroupSLB
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, iPv4 frontend IP yapılandırması ile varolan bir Standart Yük Dengeleyicisini çift yığın (IPv4 ve IPv6) yapılandırmasına güncelleştirmişsiniz. Ayrıca, arka uç havuzundaki VM'lerin NIC'lerine IPv6 yapılandırmaları eklediniz. Azure sanal ağlarında IPv6 desteği hakkında daha fazla bilgi edinmek için Azure [Sanal Ağı için IPv6 nedir'](ipv6-overview.md) e bakın?
