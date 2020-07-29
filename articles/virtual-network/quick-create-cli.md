---
title: Sanal ağ oluşturma-hızlı başlangıç-Azure CLı
titlesuffix: Azure Virtual Network
description: Bu hızlı başlangıçta, Azure CLı kullanarak bir sanal ağ oluşturmayı öğrenin. Bir sanal ağ, Azure kaynaklarının birbirleriyle ve internet ile iletişim kurmasına olanak tanır.
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
ms.openlocfilehash: ed535c76ae3c456c8aa7d2e14301b9cb9cb288be
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87283189"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLI kullanarak sanal ağ oluşturma

Sanal ağ, sanal makineler (VM) gibi Azure kaynaklarının birbiriyle ve internet ile özel olarak iletişim kurmasına olanak sağlar. Bu hızlı başlangıçta, sanal ağ oluşturmayı öğreneceksiniz. Bir sanal ağ oluşturduktan sonra, sanal ağa iki sanal makine dağıtacaksınız. Daha sonra internet 'ten VM 'lere bağlanır ve yeni sanal ağ üzerinden özel olarak iletişim kurabilirsiniz.

Azure aboneliğiniz yoksa şimdi [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Bunun yerine Azure CLı 'yı yüklemek ve kullanmak isterseniz, bu hızlı başlangıç, Azure CLı sürüm 2.0.28 veya sonraki bir sürümünü kullanmanızı gerektirir. Yüklü sürümünüzü bulmak için öğesini çalıştırın `az --version` . Bkz. Install veya Upgrade Info for [Azure CLI](/cli/azure/install-azure-cli) .

## <a name="create-a-resource-group-and-a-virtual-network"></a>Kaynak grubu ve sanal ağ oluşturma

Bir sanal ağ oluşturabilmeniz için önce sanal ağı barındırmak üzere bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group) ile bir kaynak grubu oluşturun. Bu örnek *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[az network vnet create](/cli/azure/network/vnet) komutu ile bir sanal ağ oluşturun. Bu örnek, *varsayılan*adlı bir alt ağ ile *myVirtualNetwork* adlı varsayılan bir sanal ağ oluşturur:

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Sanal ağ üzerinde iki sanal makine oluşturun.

### <a name="create-the-first-vm"></a>Birinci sanal makineyi oluşturma

[az vm create](/cli/azure/vm) ile bir VM oluşturun. SSH anahtarları varsayılan anahtar konumunda zaten mevcut değilse, komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. `--no-wait` seçeneği, sonraki adıma devam edebilmeniz için arka planda sanal makineyi oluşturur. Bu örnek, *myVm1*adlı bir sanal makine oluşturur:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>İkinci sanal makineyi oluşturma

`--no-wait`Önceki adımda bu seçeneği kullandıysanız, devam edebilir ve *myVm2*adlı ikinci VM 'yi oluşturabilirsiniz.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

### <a name="azure-cli-output-message"></a>Azure CLı çıkış iletisi

Sanal makinelerin oluşturulması birkaç dakika sürebilir. Azure VM 'Leri oluşturduktan sonra, Azure CLı şunun gibi bir çıktı döndürür:

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

**publicIpAddress** değerini not alın. Sonraki adımda İnternet 'ten VM 'ye bağlanmak için bu adresi kullanacaksınız.

## <a name="connect-to-a-vm-from-the-internet"></a>İnternet'ten bir sanal makineye bağlanma

Bu komutta, `<publicIpAddress>` *myVm2* sanal makinenizin genel IP adresiyle değiştirin:

```bash
ssh <publicIpAddress>
```

## <a name="communicate-between-vms"></a>Sanal makineler arasında iletişim

*MyVm2* ve *myVm1* VM 'ler arasındaki özel iletişimi doğrulamak için şu komutu girin:

```bash
ping myVm1 -c 4
```

*10.0.0.4*adresinden dört yanıt alacaksınız.

*myVm2* sanal makinesiyle SSH oturumundan çıkın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az Group Delete](/cli/azure/group) ' i kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, varsayılan bir sanal ağ ve iki sanal makine oluşturdunuz. İnternet 'ten bir sanal makineye bağlanırsınız ve iki VM arasında özel olarak iletişim kuracaksınız. Sanal ağ ayarları hakkında daha fazla bilgi için [Sanal ağı yönetme](manage-virtual-network.md) başlıklı konuya bakın.

Azure, VM 'Ler arasında sınırsız özel iletişime olanak sağlar. Varsayılan olarak, Azure yalnızca İnternet 'ten gelen Windows VM 'lerine gelen Uzak Masaüstü bağlantılarına izin verir. Farklı türlerde VM ağı iletişimleri yapılandırma hakkında daha fazla bilgi edinmek için [ağ trafiğini filtreleme](tutorial-filter-network-traffic.md) öğreticisine gidin.
