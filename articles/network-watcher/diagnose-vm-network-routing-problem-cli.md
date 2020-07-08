---
title: VM ağı yönlendirme sorununu tanılama-Azure CLı
titleSuffix: Azure Network Watcher
description: Bu makalede, Azure ağ Izleyicisi 'nin sonraki atlama özelliğini kullanarak bir sanal makine ağ yönlendirme sorununu tanılamayı öğreneceksiniz.
services: network-watcher
documentationcenter: network-watcher
author: damendo
editor: ''
tags: azure-resource-manager
Customer intent: I need to diagnose virtual machine (VM) network routing problem that prevents communication to different destinations.
ms.assetid: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: network-watcher
ms.workload: infrastructure
ms.date: 04/20/2018
ms.author: damendo
ms.custom: ''
ms.openlocfilehash: 889db5cdcb1807b859339eaf326e3cec7ea64b84
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84738830"
---
# <a name="diagnose-a-virtual-machine-network-routing-problem---azure-cli"></a>Bir sanal makine ağ yönlendirme sorununu tanılama-Azure CLı

Bu makalede bir sanal makineyi (VM) dağıtırsınız ve ardından bir IP adresi ile URL 'ye iletişimleri kontrol edersiniz. Bir iletişim hatasının nedenini ve bu hatayı nasıl çözeceğinizi belirlersiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Azure CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.0.28 veya üstünü çalıştırıyor olmanızı gerektirir. Yüklü sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). Azure CLı sürümünü doğruladıktan sonra, `az login` Azure ile bağlantı oluşturmak için öğesini çalıştırın. Bu makaledeki Azure CLı komutları Bash kabuğunda çalışacak şekilde biçimlendirilir.

## <a name="create-a-vm"></a>VM oluşturma

Bir sanal makine oluşturabilmeniz için sanal makineyi içerecek bir kaynak grubu oluşturmanız gerekir. [az group create](/cli/azure/group#az-group-create) ile bir kaynak grubu oluşturun. Aşağıdaki örnek *eastus* konumunda *myresourcegroup* adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[az vm create](/cli/azure/vm#az-vm-create) ile bir VM oluşturun. SSH anahtarları, varsayılan anahtar konumunda zaten mevcut değilse komut bunları oluşturur. Belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın. Aşağıdaki örnek, *myvm*adlı bir sanal makine oluşturur:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Sanal makinenin oluşturulması birkaç dakika sürer. VM oluşturuluncaya ve Azure CLı tarafından çıkış döndürülünceye kadar kalan adımlara devam etmeyin.

## <a name="test-network-communication"></a>Ağ iletişimini test etme

Ağ iletişimini ağ Izleyicisi ile test etmek için, önce test etmek istediğiniz VM 'nin bulunduğu bölgede bir ağ izleyicisi etkinleştirmeniz ve ardından iletişim sınaması için ağ Izleyicisi 'nin sonraki atlama özelliğini kullanmanız gerekir.

### <a name="enable-network-watcher"></a>Ağ izleyicisini etkinleştirme

Doğu ABD bölgesinde zaten etkinleştirilmiş bir ağ izleyicisi varsa, [sonraki atlamayı kullan](#use-next-hop)' a atlayın. Doğu ABD bölgesinde bir Ağ İzleyicisi oluşturmak için [az Network izleyici configure](/cli/azure/network/watcher#az-network-watcher-configure) komutunu kullanın:

```azurecli-interactive
az network watcher configure \
  --resource-group NetworkWatcherRG \
  --locations eastus \
  --enabled
```

### <a name="use-next-hop"></a>Sonraki atlamayı kullanma

Azure, varsayılan hedeflerin yollarını otomatik olarak oluşturur. Varsayılan yolları geçersiz kılmak için özel yollar oluşturabilirsiniz. Bazı durumlarda, özel yollar iletişimin başarısız olmasına neden olabilir. Bir VM 'den yönlendirmeyi test etmek için [az Network izleyici Show-Next-Hop](/cli/azure/network/watcher?view=azure-cli-latest#az-network-watcher-show-next-hop) ' i kullanarak trafik belirli bir adrese yönlendirilse sonraki yönlendirme atağını saptayın.

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

Birkaç saniye sonra çıktı, **Nexthoptype** 'un **Internet**olduğunu ve **routetableıd** 'in **sistem yolu**olduğunu bildirir. Bu sonuç, hedefe geçerli bir yol olduğunu bilmenizi sağlar.

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

Döndürülen çıktı, **hiçbir** bir **nexthoptype**olmadığı ve **Routetableıd** 'in de **sistem rotası**olduğunu size bildirir. Bu sonuç, hedefin geçerli bir sistem yolu olmasına rağmen trafiği hedefe yönlendiren bir sonraki atlama olmadığını size bildirir.

## <a name="view-details-of-a-route"></a>Bir yolun ayrıntılarını görüntüleme

Yönlendirmeyi daha fazla analiz etmek için [az Network Nic Show-etkin-Route-Table](/cli/azure/network/nic#az-network-nic-show-effective-route-table) komutunu kullanarak ağ arabirimine yönelik geçerli yolları gözden geçirin:

```azurecli-interactive
az network nic show-effective-route-table \
  --resource-group myResourceGroup \
  --name myVmVMNic
```

Döndürülen çıktıda aşağıdaki metin bulunur:

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

Bir `az network watcher show-next-hop` [sonraki atlamada](#use-next-hop)13.107.21.200 ile giden iletişimi test etmek için komutunu kullandığınızda, çıkışdaki başka bir yol adresi içerdiğinden, adresi adrese yönlendirmek Için **addresspredüzeltmesini** 0.0.0.0/0 * * ile yönlendirme kullanılmıştır. Varsayılan olarak, başka bir yolun adres ön ekinde belirtilmeyen tüm adresler İnternet'e yönlendirilir.

`az network watcher show-next-hop`Ancak 172.31.0.100 ile giden iletişimi test etmek için komutunu kullandığınızda, sonuç bir sonraki atlama türü olmadığını bildirdi. Döndürülen çıktıda aşağıdaki metni de görürsünüz:

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

Komutun çıktısında görebileceğiniz gibi `az network watcher nic show-effective-route-table` , 172.31.0.100 adresini de içeren 172.16.0.0/12 ön ekine varsayılan bir yol vardır ancak **Nexthoptype** **none**olur. Azure, 172.16.0.0/12 için varsayılan bir yol oluşturur ancak bir neden olmadıkça sonraki atlama türünü belirtmez. Örneğin, 172.16.0.0/12 adres aralığını sanal ağın adres alanına eklediyseniz Azure, **Nexthoptype** 'ı yol için **sanal ağ** olarak değiştirir. Ardından bir denetim, **sanal ağı** **nexthoptype**olarak gösterir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, [az group delete](/cli/azure/group#az-group-delete) komutunu kullanarak kaynak grubunu ve içerdiği tüm kaynakları kaldırabilirsiniz:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir VM oluşturdunuz ve VM 'den ağ yönlendirmesi tanılandı. Azure’un birkaç varsayılan yol oluşturduğunu öğrendiniz ve iki farklı hedefin yolunu test ettiniz. [Azure'da yönlendirme](../virtual-network/virtual-networks-udr-overview.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json) ve [özel yollar oluşturma](../virtual-network/manage-route-table.md?toc=%2fazure%2fnetwork-watcher%2ftoc.json#create-a-route) hakkında daha fazla bilgi edinin.

Giden VM bağlantıları için, ağ izleyicisinin [bağlantısını sorun giderme](network-watcher-connectivity-cli.md) ÖZELLIĞINI kullanarak VM ile bir uç nokta arasında gecikme süresi ve reddedilen ağ trafiği de belirleyebilirsiniz. Ağ Izleyicisi Bağlantı İzleyicisi özelliğini kullanarak bir sanal makine ile IP adresi veya URL gibi bir uç nokta arasındaki iletişimi izleyebilirsiniz. Nasıl yapılacağını öğrenmek için bkz. [ağ bağlantısını izleme](connection-monitor.md).
