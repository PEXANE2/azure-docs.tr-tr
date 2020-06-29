---
title: CLı kullanarak Azure spot VM 'Leri dağıtma
description: Azure spot VM 'Leri kullanarak maliyetleri tasarruf etmek için CLı 'yı nasıl kullanacağınızı öğrenin.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 06/26/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: d6560f11d26200bdd9f39c4cbae643022872d362
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85506081"
---
# <a name="deploy-spot-vms-using-the-azure-cli"></a>Azure CLı kullanarak spot VM 'Leri dağıtma

[Azure spot VM 'lerin](spot-vms.md) kullanılması, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi her zaman yapması gerektiğinde, Azure altyapısı spot VM 'Leri çıkarır. Bu nedenle, spot VM 'Ler toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için mükemmeldir.

Nokta VM 'Leri için fiyatlandırma, bölge ve SKU temel alınarak değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması. 

VM için saat başına ödeme yapmak istediğiniz maksimum fiyatı ayarlama seçeneğiniz vardır. Bir spot VM 'nin en yüksek fiyatı, en fazla 5 ondalık basamak kullanılarak ABD Doları (USD) olarak ayarlanabilir. Örneğin, değer, `0.98765` saat başına $0,98765 ABD Doları olan en yüksek fiyat olacaktır. En yüksek fiyatı olacak şekilde ayarlarsanız `-1` , VM fiyata göre çıkarılmaz. Kapasite ve kota kullanılabilir olduğu sürece, sanal makine fiyatı, nokta için geçerli fiyat veya standart bir sanal makine fiyatı olacaktır. En yüksek fiyatı ayarlama hakkında daha fazla bilgi için bkz. [spot VM 'ler-fiyatlandırma](spot-vms.md#pricing).

Azure CLı kullanarak spot ile bir VM oluşturma işlemi [hızlı başlangıç makalesinde](/azure/virtual-machines/linux/quick-create-cli)ayrıntılıdır. Yalnızca '--Priority Spot ' parametresini ekleyin, öğesini serbest bırak `--eviction-policy` (varsayılan) veya `Delete` olarak ayarlayın ve en yüksek fiyat ya da belirtin `-1` . 


## <a name="install-azure-cli"></a>Azure CLI'yı yükleme

Spot VM 'Ler oluşturmak için Azure CLı sürüm 2.0.74 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için **az--Version** ' i çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

[Az Login](/cli/azure/reference-index#az-login)kullanarak Azure 'da oturum açın.

```azurecli
az login
```

## <a name="create-a-spot-vm"></a>Spot VM oluşturma

Bu örnek, fiyata göre çıkarımayacak bir Linux spot VM 'nin nasıl dağıtılacağını göstermektedir. Çıkarma İlkesi VM 'yi serbest bırakmak üzere ayarlanır, böylece daha sonra yeniden başlatılabilir. VM çıkartılan sanal makineyi ve temel diski silmek istiyorsanız, `--eviction-policy` olarak ayarlayın `Delete` .

```azurecli
az group create -n mySpotGroup -l eastus
az vm create \
    --resource-group mySpotGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1 \
    --eviction-policy Deallocate
```



VM oluşturulduktan sonra, kaynak grubundaki tüm VM 'Ler için maksimum fatura fiyatını görmek üzere sorgulama yapabilirsiniz.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

**Sonraki adımlar**

[Azure PowerShell](../windows/spot-powershell.md), [Portal](../windows/spot-portal.md)veya [şablon](spot-template.md)kullanarak bir spot VM de oluşturabilirsiniz.

Bir hatayla karşılaşırsanız bkz. [hata kodları](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
