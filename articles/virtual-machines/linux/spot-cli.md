---
title: Azure Spot VM'lerini dağıtmak için CLI'yi kullanın
description: Maliyetlerden tasarruf etmek için Azure Spot VM'lerini dağıtmak için CLI'yi nasıl kullanacağınızı öğrenin.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 03/25/2020
ms.author: cynthn
ms.openlocfilehash: 5b6a07bfbcf56f3ca78fa4991e7741a3d44c25b9
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80544349"
---
# <a name="deploy-spot-vms-using-the-azure-cli"></a>Azure CLI'yi kullanarak Spot VM'leri dağıtma

[Azure Spot VM'leri](spot-vms.md) kullanmak, önemli bir maliyet tasarrufu nda kullanılmayan kapasitemizden yararlanmanızı sağlar. Azure'un kapasiteye geri ihtiyacı olduğu herhangi bir zamanda, Azure altyapısı Spot VM'leri boşaltacaktır. Bu nedenle, Spot VM'ler toplu iş işleri, geliştirme/test ortamları, büyük bilgi işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilir iş yükleri için idealdir.

Spot VM'ler için fiyatlandırma, bölgeye ve SKU'ya göre değişkendir. Daha fazla bilgi için [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması için bkz. 

VM için saat başına ödemek istediğiniz maksimum fiyatı belirleme seçeneğiniz vardır. Spot VM'nin maksimum fiyatı, 5 ondalık basamak kullanılarak ABD doları (USD) olarak ayarlanabilir. Örneğin, değeri `0.98765`saatte 0,98765 USD maksimum fiyat olacaktır. Maksimum fiyatı olarak `-1`ayarlarsanız, VM fiyata göre tahliye olmaz. VM için fiyat Spot veya standart bir VM için fiyat olacak, hangi hiç daha az, sürece kapasite ve kota mevcuttur. Maksimum fiyatı ayarlama hakkında daha fazla bilgi için [Spot VM'ler - Fiyatlandırma'ya](spot-vms.md#pricing)bakın.

Azure CLI'yi kullanarak Spot ile VM oluşturma [işlemi, hızlı başlangıç makalesinde](/azure/virtual-machines/linux/quick-create-cli)ayrıntılı olarak belirtildiği gibi. Sadece '--öncelikli Spot' parametresini ekleyin ve `-1`maksimum fiyat veya .


## <a name="install-azure-cli"></a>Azure CLI'yı yükleme

Spot VM'leri oluşturmak için Azure CLI sürüm 2.0.74 veya sonraki sürümlerini çalıştırıyor olmanız gerekir. Sürümü bulmak için **az --sürümünü** çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 

Az girişi kullanarak [Azure'da](/cli/azure/reference-index#az-login)oturum açın.

```azurecli
az login
```

## <a name="create-a-spot-vm"></a>Spot VM Oluşturma

Bu örnek, fiyata göre tahliye edilmeyecek bir Linux Spot VM'nin nasıl dağıtılanacağını gösterir. 

```azurecli
az group create -n mySpotGroup -l eastus
az vm create \
    --resource-group mySpotGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --priority Spot \
    --max-price -1
```

VM oluşturulduktan sonra, kaynak grubundaki tüm VM'lerin maksimum fatura fiyatını görmek için sorgu layabilirsiniz.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

**Sonraki adımlar**

[Ayrıca Azure PowerShell](../windows/spot-powershell.md) veya [şablon](spot-template.md)kullanarak bir Spot VM oluşturabilirsiniz.

Bir hatayla karşılaşırsanız, [hata kodlarına](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)bakın.
