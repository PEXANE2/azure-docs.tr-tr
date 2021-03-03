---
title: CLı kullanarak Azure spot sanal makineleri dağıtma
description: Azure spot sanal makinelerini kullanarak maliyetleri tasarruf etmek için CLı 'yı nasıl kullanacağınızı öğrenin.
author: cynthn
ms.service: virtual-machines
ms.subservice: spot
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 06/26/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: f1ad1dff695755e88881773bdcbebc2da283b75d
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101669379"
---
# <a name="deploy-azure-spot-virtual-machines-using-the-azure-cli"></a>Azure CLı kullanarak Azure spot sanal makineleri dağıtma

[Azure spot sanal makinelerinin](../spot-vms.md) kullanılması, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi yeniden sağlaması gerektiğinde Azure altyapısı, Azure spot sanal makinelerini çıkarır. Bu nedenle, Azure spot sanal makineleri toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için mükemmeldir.

Azure spot sanal makineleri için fiyatlandırma, bölge ve SKU temel alınarak değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması. 

VM için saat başına ödeme yapmak istediğiniz maksimum fiyatı ayarlama seçeneğiniz vardır. En fazla 5 ondalık basamak kullanarak bir Azure spot sanal makinesi için maksimum fiyat ABD Doları (USD) olarak ayarlanabilir. Örneğin, değer, `0.98765` saat başına $0,98765 ABD Doları olan en yüksek fiyat olacaktır. En yüksek fiyatı olacak şekilde ayarlarsanız `-1` , VM fiyata göre çıkarılmaz. Kapasite ve kota kullanılabilir olduğu sürece, sanal makine için Fiyat, Azure spot sanal makinesi için geçerli fiyat veya standart bir VM 'nin fiyatı olacaktır. En yüksek fiyatı ayarlama hakkında daha fazla bilgi için bkz. [Azure spot sanal makineler-fiyatlandırma](../spot-vms.md#pricing).

Azure CLı kullanarak Azure spot sanal makinesi oluşturma işlemi [hızlı başlangıç makalesinde](./quick-create-cli.md)ayrıntılıdır. Yalnızca '--Priority Spot ' parametresini ekleyin, öğesini serbest bırak `--eviction-policy` (varsayılan) veya `Delete` olarak ayarlayın ve en yüksek fiyat ya da belirtin `-1` . 


## <a name="install-azure-cli"></a>Azure CLI'yı yükleme

Azure spot sanal makineleri oluşturmak için Azure CLı sürüm 2.0.74 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için **az--Version** ' i çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

[Az Login](/cli/azure/reference-index#az-login)kullanarak Azure 'da oturum açın.

```azurecli
az login
```

## <a name="create-an-azure-spot-virtual-machine"></a>Azure spot sanal makinesi oluşturma

Bu örnek, fiyata göre çıkarımayacak bir Linux Azure spot sanal makinesinin nasıl dağıtılacağını göstermektedir. Çıkarma İlkesi VM 'yi serbest bırakmak üzere ayarlanır, böylece daha sonra yeniden başlatılabilir. VM çıkartılan sanal makineyi ve temel diski silmek istiyorsanız, `--eviction-policy` olarak ayarlayın `Delete` .

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

## <a name="simulate-an-eviction"></a>Çıkargı benzetimi yap

Uygulamanızın ani bir çıkargı için ne kadar iyi yanıt olacağını test etmek üzere bir Azure spot sanal makinesi [çıkarması benzetimi](/rest/api/compute/virtualmachines/simulateeviction) yapabilirsiniz. 

Aşağıdaki bilgilerinizi ile değiştirin: 

- `subscriptionId`
- `resourceGroupName`
- `vmName`


```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/simulateEviction?api-version=2020-06-01
```

**Sonraki adımlar**

[Azure PowerShell](../windows/spot-powershell.md), [Portal](../spot-portal.md)veya [şablon](spot-template.md)kullanarak Azure spot sanal makinesi de oluşturabilirsiniz.

Azure spot sanal makinesi hakkında bilgi edinmek için [Azure perakende FIYATLARı API](/rest/api/cost-management/retail-prices/azure-retail-prices) 'sini kullanarak geçerli fiyatlandırma bilgilerini sorgulayın. `meterName`Ve `skuName` her ikisi de içerecektir `Spot` .

Bir hatayla karşılaşırsanız bkz. [hata kodları](../error-codes-spot.md).