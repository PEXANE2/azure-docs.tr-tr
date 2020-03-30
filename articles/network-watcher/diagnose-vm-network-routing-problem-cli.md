---
title: VM ağ yönlendirme sorununu tanıla - Azure CLI
titleSuffix: Azure Network Watcher
description: Bu makalede, Azure Ağ İzleyicisi'nin bir sonraki atlama özelliğini kullanarak sanal makine ağı yönlendirme sorununu nasıl tanıladığınızı öğrenirsiniz.
services: network-watcher
documentationcenter: network-watcher
author: damendo
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: damendo
ms.custom: ''
ms.openlocfilehash: ae139ea7aca7c3896fcd7b0acf2bf6673490a2f4
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382911"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-cli"></a>Sanal makine ağı yönlendirme sorununu tanılama - Azure CLI

Bu makalede, sanal bir makine (VM) dağıtın ve ardından bir IP adresi ve URL'ye iletişimi denetlersiniz. Bir iletişim hatasının nedenini ve bu hatayı nasıl çözeceğinizi belirlersiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.28 veya sonraki sürümlerini çalıştırdığınız gerekir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). Azure CLI sürümünü doğruladıktan `az login` sonra, Azure ile bağlantı oluşturmak için çalıştırın. Bu makaledeki Azure CLI komutları Bash kabuğunda çalışacak şekilde biçimlendirilir.

## <a name="create-a-vm"></a>VM oluşturma

Bir sanal makine oluşturabilmeniz için sanal makineyi içerecek bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group#az-group-create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek, *eastus* konumda *myResourceGroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[az vm create](/cli/azure/vm#az-vm-create) ile bir VM oluşturun. SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. Aşağıdaki örnek *myVm*adlı bir VM oluşturur:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturulana ve Azure CLI çıktıyı döndürene kadar kalan adımlarla devam etmeyin.

## <a name="test-network-communication"></a>Ağ iletişimini test etme

Ağ İzleyicisi ile ağ iletişimini test etmek için önce test etmek istediğiniz VM bölgesinde bir ağ izleyicisine etkinleştirmeniz ve ardından iletişimi sınamak için Network Watcher'ın bir sonraki atlama özelliğini kullanmanız gerekir.

### <a name="enable-network-watcher"></a>Ağ izleyicisini etkinleştirme

Doğu ABD bölgesinde zaten etkin bir ağ izleyiciniz varsa, [sonraki atlamayı kullan'a](#use-next-hop)atlayın. Doğu ABD bölgesinde bir ağ izleyicisi oluşturmak için [az ağ izleyiciyapılandırma](/cli/azure/network/watcher#az-network-watcher-configure) komutunu kullanın:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-next-hop"></a>Sonraki atlamayı kullanma

Azure, varsayılan hedeflerin yollarını otomatik olarak oluşturur. Varsayılan yolları geçersiz kılmak için özel yollar oluşturabilirsiniz. Bazı durumlarda, özel yollar iletişimin başarısız olmasına neden olabilir. VM'den yönlendirmeyi sınamak için, trafiğin belirli bir adrese mukadder olduğu bir sonraki yönlendirme atlamasını belirlemek için [az ağ izleyicisi show-next-hop'ı](/cli/azure/network/watcher?view=azure-cli-latest#az-network-watcher-show-next-hop) kullanın.

Sanal makineden, www.bing.com adresinin IP adreslerinden birine giden iletişimi test etme:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 13.107.21.200 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Birkaç saniye sonra, çıkış **nextHopType** **Internet**olduğunu bildirir , ve **routeTableId** **Sistem Rotası**olduğunu . Bu sonuç, hedefe giden geçerli bir rota olduğunu bilmenizi sağlar.

Sanal makineden 172.31.0.100 adresine giden iletişimi test etme:

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 172.31.0.100 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVm \
  --nic myVmVMNic \
  --out table
```

Döndürülen **çıktı,** **None'un nextHopType**olduğunu ve **routeTableId'in** de **Sistem Rotası**olduğunu bildirir. Bu sonuç, hedefin geçerli bir sistem yolu olmasına rağmen trafiği hedefe yönlendiren bir sonraki atlama olmadığını size bildirir.

## <a name="view-details-of-a-route"></a>Bir yolun ayrıntılarını görüntüleme

Yönlendirmeyi daha fazla analiz etmek için, [az network nic show-effective-route-table](/cli/azure/network/nic#az-network-nic-show-effective-route-table) komutuyla ağ arabiriminin etkili yollarını gözden geçirin:

```azurecli-interactive
az network nic show-effective-route-table \
  --resource-group myResourceGroup \
  --name myVmVMNic
```

Döndürülen çıktıya aşağıdaki metin dahildir:

```
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
  },
  "addressPrefix": [
    "0.0.0.0/0"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "Internet",
  "source": "Default",
  "state": "Active"
},
```

Giden iletişimi `az network watcher show-next-hop` 13.107.21.200'e test etmek için komutu [kullandığınızda,](#use-next-hop)çıkışta başka bir yol adresi içermediğinden, 0.0.0.0/0** adresine giden yolu önceden **kullandı.** Varsayılan olarak, başka bir yolun adres ön ekinde belirtilmeyen tüm adresler İnternet'e yönlendirilir.

Ancak 172.31.0.100 giden iletişimi test etmek için `az network watcher show-next-hop` komutu kullandığınızda, sonuç bir sonraki atlama türü olmadığını size bildirdi. Döndürülen çıktıda aşağıdaki metni de görürsünüz:

```
{
  "additionalProperties": {
    "disableBgpRoutePropagation": false
      },
  "addressPrefix": [
    "172.16.0.0/12"
  ],
  "name": null,
  "nextHopIpAddress": [],
  "nextHopType": "None",
  "source": "Default",
  "state": "Active"
},
```

`az network watcher nic show-effective-route-table` Komuttan çıktıda görebileceğiniz gibi, 172.31.0.100 adresini içeren 172.16.0.0/12 önekinin varsayılan bir rotası olmasına rağmen, **nextHopType** **Yok'** tır. Azure, 172.16.0.0/12 için varsayılan bir yol oluşturur ancak bir neden olmadıkça sonraki atlama türünü belirtmez. Örneğin, sanal ağın adres alanına 172.16.0.0/12 adres aralığını eklediyseniz, Azure **sonraki HopType'ı** rota için **Sanal ağa** değiştirir. Bir denetim daha sonra **sonraki HopType**olarak **Sanal ağ** gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir VM oluşturdunuz ve VM'den ağ yönlendirmesi tanısı koydunuz. Azure’un birkaç varsayılan yol oluşturduğunu öğrendiniz ve iki farklı hedefin yolunu test ettiniz. [Azure'da yönlendirme](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve [özel yollar oluşturma](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route) hakkında daha fazla bilgi edinin.

Giden VM bağlantıları için, Ağ İzleyicisi'nin [bağlantı sorun giderme](network-watcher-connectivity-cli.md) özelliğini kullanarak VM ile bitiş noktası arasındaki gecikmeyi ve ağ trafiğini belirleyebilirsiniz. Ağ İzleyicisi bağlantı izleme özelliğini kullanarak, bir VM ile IP adresi veya URL gibi bir uç nokta arasındaki iletişimi zaman içinde izleyebilirsiniz. Nasıl yapılacağını öğrenmek için [bkz.](connection-monitor.md)
