---
title: Azure spot VM 'Leri dağıtmak için portalı kullanma
description: Maliyetlerden tasarruf etmek için spot VM 'Leri dağıtmak üzere Azure PowerShell nasıl kullanacağınızı öğrenin.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/11/2020
ms.author: cynthn
ms.openlocfilehash: 8845d0f9277384c1cc32b31b5ea5151cd0668c79
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77158987"
---
# <a name="preview-deploy-spot-vms-using-the-azure-portal"></a>Önizleme: Azure portal kullanarak spot VM 'Leri dağıtma

[Spot VM 'lerin](spot-vms.md) kullanılması, önemli bir maliyet tasarruflarından kullanılmamış kapasitemizin avantajlarından yararlanmanızı sağlar. Azure 'un kapasiteyi her zaman yapması gerektiğinde, Azure altyapısı spot VM 'Leri çıkarır. Bu nedenle, spot VM 'Ler toplu işleme işleri, geliştirme/test ortamları, büyük işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilen iş yükleri için mükemmeldir.

Nokta VM 'Leri için fiyatlandırma, bölge ve SKU temel alınarak değişkendir. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması. En yüksek fiyatı ayarlama hakkında daha fazla bilgi için bkz. [spot VM 'ler-fiyatlandırma](spot-vms.md#pricing).

VM için saat başına ödeme yapmak istediğiniz maksimum fiyatı ayarlama seçeneğiniz vardır. Bir spot VM 'nin en yüksek fiyatı, en fazla 5 ondalık basamak kullanılarak ABD Doları (USD) olarak ayarlanabilir. Örneğin değer `0.05701`, saat başına $0,05701 ABD Doları olan en yüksek fiyat olacaktır. En büyük fiyatı `-1`olarak ayarlarsanız, VM fiyata göre çıkarılmaz. Kapasite ve kota kullanılabilir olduğu sürece, sanal makine fiyatı, nokta için geçerli fiyat veya standart bir sanal makine fiyatı olacaktır.

> [!IMPORTANT]
> Nokta örnekleri şu anda genel önizlemededir.
> Bu önizleme sürümü üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>


## <a name="create-the-vm"></a>Sanal makine oluşturma

Spot VM 'Ler kullanan bir VM oluşturma işlemi [hızlı](quick-create-portal.md)başlangıçta ayrıntılıdır. Bir sanal makine dağıttığınızda, bir Azure spot örneği kullanmayı tercih edebilirsiniz.


**Temel bilgiler** sekmesinde, **örnek ayrıntıları** bölümünde, **Hayır** , Azure spot örneği kullanımı için varsayılan değer değildir.

![Hayır seçme için ekran yakalama, Azure spot örneği kullanma](media/spot-portal/no.png)

**Evet**' i seçtiğinizde, Bölüm genişletilir ve [çıkarma türü ve çıkarma ilkenizi](spot-vms.md#eviction-policy)seçebilirsiniz. 

![Ekran yakalama Evet seçme, Azure spot örneği kullanma](media/spot-portal/yes.png)


## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, [PowerShell](spot-powershell.md)kullanarak spot VM 'ler oluşturabilirsiniz.