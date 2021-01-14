---
title: CLı kullanarak Azure spot VM 'Leri dağıtma
description: Azure spot VM 'Leri kullanarak maliyetleri tasarruf etmek için CLı 'yı nasıl kullanacağınızı öğrenin.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 06/26/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 71e0f2e87fc71deef0bdc4dd48425cdc9dd99dc8
ms.sourcegitcommit: 2bd0a039be8126c969a795cea3b60ce8e4ce64fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2021
ms.locfileid: "98200779"
---
# <a name="deploy-spot-vms-using-the-azure-cli"></a>Azure CLı kullanarak spot VM 'Leri dağıtma

[Azure spot VM 'lerin](../spot-vms.md) kullanılması, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi her zaman yapması gerektiğinde, Azure altyapısı spot VM 'Leri çıkarır. Bu nedenle, spot VM 'Ler toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için mükemmeldir.

Nokta VM 'Leri için fiyatlandırma, bölge ve SKU temel alınarak değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması. 

VM için saat başına ödeme yapmak istediğiniz maksimum fiyatı ayarlama seçeneğiniz vardır. Bir spot VM 'nin en yüksek fiyatı, en fazla 5 ondalık basamak kullanılarak ABD Doları (USD) olarak ayarlanabilir. Örneğin, değer, `0.98765` saat başına $0,98765 ABD Doları olan en yüksek fiyat olacaktır. En yüksek fiyatı olacak şekilde ayarlarsanız `-1` , VM fiyata göre çıkarılmaz. Kapasite ve kota kullanılabilir olduğu sürece, sanal makine fiyatı, nokta için geçerli fiyat veya standart bir sanal makine fiyatı olacaktır. En yüksek fiyatı ayarlama hakkında daha fazla bilgi için bkz. [spot VM 'ler-fiyatlandırma](../spot-vms.md#pricing).

Azure CLı kullanarak spot ile bir VM oluşturma işlemi [hızlı başlangıç makalesinde](./quick-create-cli.md)ayrıntılıdır. Yalnızca '--Priority Spot ' parametresini ekleyin, öğesini serbest bırak `--eviction-policy` (varsayılan) veya `Delete` olarak ayarlayın ve en yüksek fiyat ya da belirtin `-1` . 


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

## <a name="simulate-an-eviction"></a>Çıkargı benzetimi yap

Uygulamanızın ani bir çıkarma için ne kadar iyi yanıt olacağını test etmek için bir spot VM 'nin [çıkarmasını benzedönüştürebilirsiniz](/rest/api/compute/virtualmachines/simulateeviction) . 

Aşağıdaki bilgilerinizi ile değiştirin: 

- `subscriptionId`
- `resourceGroupName`
- `vmName`


```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/simulateEviction?api-version=2020-06-01
```

**Sonraki adımlar**

[Azure PowerShell](../windows/spot-powershell.md), [Portal](../spot-portal.md)veya [şablon](spot-template.md)kullanarak bir spot VM de oluşturabilirsiniz.

Spot fiyatlandırması hakkında bilgi için [Azure perakende FIYATLARı API](/rest/api/cost-management/retail-prices/azure-retail-prices) 'sini kullanarak geçerli fiyatlandırma bilgilerini sorgulayın. `meterName`Ve `skuName` her ikisi de içerecektir `Spot` .

Bir hatayla karşılaşırsanız bkz. [hata kodları](../error-codes-spot.md).