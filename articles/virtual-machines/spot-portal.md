---
title: Azure spot sanal makineleri dağıtmak için portalı kullanma
description: Maliyetlerden tasarruf etmek için spot sanal makineleri dağıtmak üzere Azure PowerShell nasıl kullanılır.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 09/14/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 28f21cad2387712fe638067600087ba6adbbfa42
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100554932"
---
# <a name="deploy-azure-spot-virtual-machines-using-the-azure-portal"></a>Azure portal kullanarak Azure spot sanal makineleri dağıtma

[Azure spot sanal makinelerinin](spot-vms.md) kullanılması, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi yeniden sağlaması gerektiğinde Azure altyapısı, Azure spot sanal makinelerini çıkarır. Bu nedenle, Azure spot sanal makineleri toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için mükemmeldir.

Azure spot sanal makineleri için fiyatlandırma, bölge ve SKU temel alınarak değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması. En yüksek fiyatı ayarlama hakkında daha fazla bilgi için bkz. [Azure spot sanal makineler-fiyatlandırma](spot-vms.md#pricing).

VM için saat başına ödeme yapmak istediğiniz maksimum fiyatı ayarlama seçeneğiniz vardır. Azure spot sanal makinesi için en fazla fiyat, en fazla 5 ondalık basamak kullanılarak ABD Doları (USD) olarak ayarlanabilir. Örneğin, değer, `0.05701` saat başına $0,05701 ABD Doları olan en yüksek fiyat olacaktır. En yüksek fiyatı olacak şekilde ayarlarsanız `-1` , VM fiyata göre çıkarılmaz. Kapasite ve kota kullanılabilir olduğu sürece, sanal makine fiyatı, nokta için geçerli fiyat veya standart bir sanal makine fiyatı olacaktır.

VM çıkarıldığında, VM 'yi ve temel diski silme ya da VM 'yi serbest bırakma seçeneğiniz vardır ve bu sayede daha sonra yeniden başlatılabilir.


## <a name="create-the-vm"></a>Sanal makineyi oluşturma

Bir sanal makine dağıttığınızda, bir Azure spot örneği kullanmayı tercih edebilirsiniz.


**Temel bilgiler** sekmesinde, **örnek ayrıntıları** bölümünde, **Hayır** , Azure spot örneği kullanımı için varsayılan değer değildir.

![Hayır seçme için ekran yakalama, Azure spot örneği kullanma](./media/spot-portal/no.png)

**Evet**' i seçerseniz, Bölüm genişletilir ve [çıkarma türü ve çıkarma ilkenizi](spot-vms.md#eviction-policy)seçebilirsiniz. 

![Ekran yakalama Evet seçme, Azure spot örneği kullanma](./media/spot-portal/yes.png)

Ayrıca, fiyatlandırma **geçmişini görüntüle ' ye ve yakındaki bölgelerdeki fiyatları karşılaştırarak** fiyatlandırma ve çıkarma oranlarını diğer benzer bölgelerle de karşılaştırabilirsiniz.

Bu örnekte, Kanada Orta bölgesi daha ucuz ve Doğu ABD bölgesinden daha düşük bir çıkarma hızına sahiptir.

:::image type="content" source="./media/spot-portal/pricing.png" alt-text="Fiyatlandırma ve çıkarma tarifelerinin farkı ile bölge seçeneklerinin ekran görüntüsü.":::

Bölgeyi sizin için en iyi şekilde çalıştıran seçimi seçerek ve ardından **Tamam**' ı seçerek değiştirebilirsiniz.

## <a name="simulate-an-eviction"></a>Çıkargı benzetimi yap

Uygulamanızın ani bir çıkarmasına ne kadar iyi yanıt vereceğini test etmek için bir Azure spot sanal makinesinin [çıkarmasını taklit](/rest/api/compute/virtualmachines/simulateeviction) edebilirsiniz. 

Aşağıdaki bilgilerinizi ile değiştirin: 

- `subscriptionId`
- `resourceGroupName`
- `vmName`


```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/simulateEviction?api-version=2020-06-01
```

## <a name="next-steps"></a>Sonraki adımlar

Ayrıca [PowerShell](./windows/spot-powershell.md), [CLI](./linux/spot-cli.md)veya [şablon](./linux/spot-template.md)kullanarak Azure spot sanal makineleri oluşturabilirsiniz.
