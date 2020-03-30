---
title: Sanal ağ oluşturma - quickstart - Azure CLI
titlesuffix: Azure Virtual Network
description: Bu hızlı başlangıçta, Azure CLI'yi kullanarak sanal ağ oluşturmayı öğrenirsiniz. Sanal ağ, sanal makineler gibi Azure kaynaklarının birbirleriyle ve internetle özel olarak iletişim kurmasına olanak tanır.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/22/2019
ms.author: kumud
ms.openlocfilehash: 2a4c63aeaa303692fa0f2d115a3df0d80cfab0b1
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80235190"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLI kullanarak sanal ağ oluşturma

Sanal ağ, sanal makineler (VM'ler) gibi Azure kaynaklarının birbirleriyle ve internetle özel olarak iletişim kurmasını sağlar. Bu hızlı başlangıçta, sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ oluşturduktan sonra, sanal ağa iki sanal makine dağıtacaksınız. Daha sonra Internet'ten Sanal M'lere bağlanır ve yeni sanal ağ üzerinden özel olarak iletişim kurarsınız.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLI'yi yerel olarak yüklemeye ve kullanmaya karar verirseniz, bu hızlı başlangıç Azure CLI sürüm 2.0.28 veya sonraki sürümlerini kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için `az --version`çalıştırın. Bilgileri yüklemek veya yükseltmek için [Azure CLI'yi yükle'ye](/cli/azure/install-azure-cli) bakın.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Kaynak grubu ve sanal ağ oluşturma

Sanal ağ oluşturmadan önce, sanal ağı barındıracak bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Bu örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[az network vnet create](/cli/azure/network/vnet) komutu ile bir sanal ağ oluşturun. Bu örnekte varsayılan adlı bir alt ağ ile *myVirtualNetwork* adlı *varsayılan*bir sanal ağ oluşturur:

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

[az vm create](/cli/azure/vm) ile bir VM oluşturun. SSH anahtarları varsayılan anahtar konumunda zaten yoksa, komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. `--no-wait` seçeneği, sonraki adıma devam edebilmeniz için arka planda sanal makineyi oluşturur. Bu örnek *myVm1*adlı bir VM oluşturur:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

Önceki adımda `--no-wait` seçeneği kullandığınızdan, devam edin ve *myVm2*adlı ikinci VM oluşturabilirsiniz.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="azure-cli-output-message"></a>Azure CLI çıktı iletisi

Sanal makinelerin oluşturulması birkaç dakika sürebilir. Azure VM'leri oluşturduktan sonra, Azure CLI çıktısını şu şekilde döndürür:

```output
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
  "zones": ""
}
```

**publicIpAddress** değerini not alın. Bir sonraki adımda internetten VM'ye bağlanmak için bu adresi kullanacaksınız.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Bu komutta, `<publicIpAddress>` *myVm2* VM'nizin genel IP adresiyle değiştirin:

```bash
ssh <publicIpAddress>
```

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

*myVm2* ve *myVm1* VM'ler arasındaki özel iletişimi onaylamak için şu komutu girin:

```bash
ping myVm1 -c 4
```

*10.0.0.4'ten*dört yanıt alırsınız.

*myVm2* sanal makinesiyle SSH oturumundan çıkın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve sahip olduğu tüm kaynakları kaldırmak için [az group delete'i](/cli/azure/group) kullanabilirsiniz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternetten bir VM'ye bağlandınız ve iki VM arasında özel olarak iletişim kurdunuz. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Azure, VM'ler arasında sınırsız özel iletişim sağlar. Varsayılan olarak Azure, yalnızca Internet'ten Windows VM'lere gelen uzak masaüstü bağlantılarını sağlar. Farklı VM ağ iletişimi türlerini yapılandırma hakkında daha fazla bilgi edinmek için [Filtre ağı trafiği](tutorial-filter-network-traffic.md) öğreticisine gidin.
