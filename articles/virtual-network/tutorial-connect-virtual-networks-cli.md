---
title: Sanal ağları VNet eşlemesiyle bağlayın - Azure CLI
description: Bu makalede, Azure CLI'yi kullanarak sanal ağları sanal ağ eşlemesiyle nasıl bağlayabileceğinizi öğreneceksiniz.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: aa2d75173b14e768a207336b54b3dc10a8c3ea5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235157"
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>Azure CLI'yi kullanarak sanal ağ eşlemesiyle sanal ağları birbirine bağlayın

Sanal ağ eşlemesi ile sanal ağları birbirine bağlayabilirsiniz. Sanal ağlar eşlendikten sonra, kaynaklar aynı sanal ağ üzerindeymiş gibi, aynı gecikme süresi ve bant genişliği ile her iki sanal ağdaki kaynaklar birbiriyle iletişim kurabilir. Bu makalede şunları öğreneceksiniz:

* İki sanal ağ oluşturma
* Sanal ağ eşlemesi iki sanal ağı bağlama
* Her sanal ağa sanal makine (VM) dağıtma
* Sanal makineler arasında iletişim

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.28 veya sonraki sürümlerini çalıştırdığınız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme](/cli/azure/install-azure-cli). 

## <a name="create-virtual-networks"></a>Sanal ağlar oluşturma

Sanal ağ oluşturmadan önce, sanal ağ ve bu makalede oluşturulan diğer tüm kaynaklar için bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

[az network vnet create](/cli/azure/network/vnet) komutu ile bir sanal ağ oluşturun. Aşağıdaki örnek, adres öneki *10.0.0.0/16*ile *myVirtualNetwork1* adlı bir sanal ağ oluşturur.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.0.0.0/24
```

adres öneki *10.1.0.0/16*ile *myVirtualNetwork2* adlı bir sanal ağ oluşturun:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --address-prefixes 10.1.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.1.0.0/24
```

## <a name="peer-virtual-networks"></a>Sanal ağları eşleme

Peerings sanal ağ kimlikleri arasında kurulur, bu nedenle öncelikle [az ağ vnet göstermek](/cli/azure/network/vnet) ve bir değişken kimliği depolamak ile her sanal ağ kimliğini almak gerekir.

```azurecli-interactive
# Get the id for myVirtualNetwork1.
vNet1Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork1 \
  --query id --out tsv)

# Get the id for myVirtualNetwork2.
vNet2Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork2 \
  --query id \
  --out tsv)
```

[az ağ vnet peering oluşturmak](/cli/azure/network/vnet/peering)ile *myVirtualNetwork1* *myVirtualNetwork2* bir peering oluşturun. `--allow-vnet-access` Parametre belirtilmemişse, bir eşleme kurulur, ancak içinden hiçbir iletişim akmaz.

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --remote-vnet $vNet2Id \
  --allow-vnet-access
```

Önceki komut yürütüldünden sonra döndürülen çıktıda, **peeringState'in** *başlatıldığını*görürsünüz. *MyVirtualNetwork2'den myVirtualNetwork1'e* kadar eşleme oluşturana *myVirtualNetwork1*kadar eşleme *Başlatılan* durumda kalır. *myVirtualNetwork2'den* *myVirtualNetwork1'e*bir eşleme oluşturun. 

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork2-myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork2 \
  --remote-vnet $vNet1Id \
  --allow-vnet-access
```

Önceki komut yürütüldünden sonra döndürülen çıktıda, **peeringState'in** *Bağlı*olduğunu görürsünüz. Azure ayrıca *myVirtualNetwork1-myVirtualNetwork2'nin* *Connected'e*bakarak bakış durumunu da değiştirdi. *MyVirtualNetwork1-myVirtualNetwork2'nin* eşleme durumunun [az network vnet peering show](/cli/azure/network/vnet/peering)ile *Bağlantılı* olarak değiştirilmelerini onaylayın.

```azurecli-interactive
az network vnet peering show \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --query peeringState
```

Bir sanal ağdaki kaynaklar, her iki sanal ağdaki **eşlemeler için peeringState** *Bağlanıncaya*kadar diğer sanal ağdaki kaynaklarla iletişim kuramaz. 

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sonraki bir adımda aralarında iletişim kurabilmeniz için her sanal ağ üzerinde bir sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

[az vm create](/cli/azure/vm) ile bir VM oluşturun. Aşağıdaki örnek, *myVirtualNetwork1* sanal ağında *myVm1* adında bir VM oluşturur. SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. Seçenek `--no-wait` arka planda VM oluşturur, böylece bir sonraki adıma devam edebilirsiniz.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork1 \
  --subnet Subnet1 \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

*myVirtualNetwork2* sanal ağında bir VM oluşturun.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork2 \
  --subnet Subnet1 \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulduktan sonra, Azure CLI aşağıdaki örneğe benzer bilgileri gösterir: 

```output
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.1.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

**publicIpAddress** değerini not alın. Bu adres, daha sonraki bir adımda VM'ye internetten erişmek için kullanılır.

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

*myVm2* VM ile bir SSH oturumu oluşturmak için aşağıdaki komutu kullanın. VM'nizin genel IP adresiyle değiştirin. `<publicIpAddress>` Önceki örnekte, genel IP adresi *13.90.242.231'dir.*

```bash
ssh <publicIpAddress>
```

*Ping myVirtualNetwork1*vm .

```bash
ping 10.0.0.4 -c 4
```

Dört yanıt alırsınız. 

SSH oturumunu *myVm2* VM'ye kapatın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az grubu silme'yi](/cli/azure/group) kullanın.

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, aynı Azure bölgesindeki iki ağın sanal ağ eşlemesiyle nasıl bağlandığınızı öğrendiniz. Farklı [desteklenen bölgelerde](virtual-network-manage-peering.md#cross-region) ve [farklı Azure aboneliklerinde](create-peering-different-subscriptions.md#cli) sanal ağları eşleyebilir ve eşleme ile [hub ve bağlı bileşen ağ tasarımları](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke#virtual-network-peering) oluşturabilirsiniz. Sanal ağ eşlemesi hakkında daha fazla bilgi için bkz. [Sanal ağ eşlemesine genel bakış](virtual-network-peering-overview.md) ve [Sanal ağ eşlemelerini yönetme](virtual-network-manage-peering.md).

VPN üzerinden [kendi bilgisayarınızı sanal ağa bağlayabilir](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ve sanal ağdaki veya eşlenen sanal ağlardaki kaynaklarla etkileşimkurabilirsiniz. Yeniden kullanılabilir komut dosyaları için sanal ağ makalelerinde kapsanan görevlerin çoğunu tamamlamak için [komut dosyası örneklerine](cli-samples.md)bakın.
