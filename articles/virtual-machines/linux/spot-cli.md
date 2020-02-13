---
title: CLı kullanarak Azure spot VM 'Leri dağıtma (Önizleme)
description: Azure spot VM 'Leri kullanarak maliyetleri tasarruf etmek için CLı 'yı nasıl kullanacağınızı öğrenin.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2020
ms.author: cynthn
ms.openlocfilehash: 110e935671ab1d640b2ff3dc26c203b262e999fe
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77163101"
---
# <a name="preview-deploy-spot-vms-using-the-azure-cli"></a>Önizleme: Azure CLı kullanarak spot VM 'Leri dağıtma

[Azure spot VM 'lerin](spot-vms.md) kullanılması, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi her zaman yapması gerektiğinde, Azure altyapısı spot VM 'Leri çıkarır. Bu nedenle, spot VM 'Ler toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için mükemmeldir.

Nokta VM 'Leri için fiyatlandırma, bölge ve SKU temel alınarak değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması. 

VM için saat başına ödeme yapmak istediğiniz maksimum fiyatı ayarlama seçeneğiniz vardır. Bir spot VM 'nin en yüksek fiyatı, en fazla 5 ondalık basamak kullanılarak ABD Doları (USD) olarak ayarlanabilir. Örneğin değer `0.98765`, saat başına $0,98765 ABD Doları olan en yüksek fiyat olacaktır. En büyük fiyatı `-1`olarak ayarlarsanız, VM fiyata göre çıkarılmaz. Kapasite ve kota kullanılabilir olduğu sürece, sanal makine fiyatı, nokta için geçerli fiyat veya standart bir sanal makine fiyatı olacaktır. En yüksek fiyatı ayarlama hakkında daha fazla bilgi için bkz. [spot VM 'ler-fiyatlandırma](spot-vms.md#pricing).

Azure CLı kullanarak spot ile bir VM oluşturma işlemi [hızlı başlangıç makalesinde](/azure/virtual-machines/linux/quick-create-cli)ayrıntılıdır. '--Priority Spot ' parametresini eklemeniz ve en yüksek fiyat veya `-1`sağlamanız yeterlidir.

> [!IMPORTANT]
> Nokta örnekleri şu anda genel önizlemededir.
> Bu önizleme sürümü üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>



## <a name="install-azure-cli"></a>Azure CLI'yı yükleme

Spot VM 'Ler oluşturmak için Azure CLı sürüm 2.0.74 veya üstünü çalıştırıyor olmanız gerekir. Sürümü bulmak için **az --version** komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yi yükleme](/cli/azure/install-azure-cli). 

[Az Login](/cli/azure/reference-index#az-login)kullanarak Azure 'da oturum açın.

```azurecli
az login
```

## <a name="create-a-spot-vm"></a>Spot VM oluşturma

Bu örnek, fiyata göre çıkarımayacak bir Linux spot VM 'nin nasıl dağıtılacağını göstermektedir. 

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

VM oluşturulduktan sonra, kaynak grubundaki tüm VM 'Ler için maksimum fatura fiyatını görmek üzere sorgulama yapabilirsiniz.

```azurecli
az vm list \
   -g mySpotGroup \
   --query '[].{Name:name, MaxPrice:billingProfile.maxPrice}' \
   --output table
```

**Sonraki adımlar**

Ayrıca, [Azure PowerShell](../windows/spot-powershell.md) veya [şablon](spot-template.md)kullanarak bir spot VM oluşturabilirsiniz.

Bir hatayla karşılaşırsanız bkz. [hata kodları](../error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
