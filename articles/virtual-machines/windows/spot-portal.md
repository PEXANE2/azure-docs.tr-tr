---
title: Azure Spot VM'leri dağıtmak için portalı kullanın
description: Maliyetlerden tasarruf etmek için Spot VM'leri dağıtmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.topic: article
ms.date: 02/11/2020
ms.author: cynthn
ms.openlocfilehash: 8845d0f9277384c1cc32b31b5ea5151cd0668c79
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77158987"
---
# <a name="preview-deploy-spot-vms-using-the-azure-portal"></a>Önizleme: Azure portalını kullanarak Spot VM'leri dağıtma

[Spot VM'leri](spot-vms.md) kullanmak, önemli bir maliyet tasarrufu nda kullanılmayan kapasitemizden yararlanmanızı sağlar. Azure'un kapasiteye geri ihtiyacı olduğu herhangi bir zamanda, Azure altyapısı Spot VM'leri boşaltacaktır. Bu nedenle, Spot VM'ler toplu iş işleri, geliştirme/test ortamları, büyük bilgi işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilir iş yükleri için idealdir.

Spot VM'ler için fiyatlandırma, bölgeye ve SKU'ya göre değişkendir. Daha fazla bilgi için [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması için bkz. Maksimum fiyatı ayarlama hakkında daha fazla bilgi için [Spot VM'ler - Fiyatlandırma'ya](spot-vms.md#pricing)bakın.

VM için saat başına ödemek istediğiniz maksimum fiyatı belirleme seçeneğiniz vardır. Spot VM'nin maksimum fiyatı, 5 ondalık basamak kullanılarak ABD doları (USD) olarak ayarlanabilir. Örneğin, değeri `0.05701`saat başına 0,05701 USD maksimum fiyat olacaktır. Maksimum fiyatı olarak `-1`ayarlarsanız, VM fiyata göre tahliye olmaz. VM için fiyat spot veya standart bir VM için fiyat olacak, hangi hiç daha az, sürece kapasite ve kota mevcuttur.

> [!IMPORTANT]
> Spot örnekleri şu anda genel önizlemede.
> Bu önizleme sürümü üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
>


## <a name="create-the-vm"></a>Sanal makine oluşturma

Spot VM kullanan bir VM oluşturma [işlemi, hızlı başlatmada](quick-create-portal.md)ayrıntılı olarak aynıdır. Bir VM dağıtırken, bir Azure spot örneğini kullanmayı seçebilirsiniz.


Temel **Bilgiler** sekmesinde, **Örnek ayrıntıları** bölümünde, **Hayır,** Azure spot örneğini kullanmak için varsayılan dır.

![Hayır seçmek için ekran yakalama, Azure spot örneği kullanmayın](media/spot-portal/no.png)

**Evet'i**seçerseniz, bölüm genişler ve [tahliye türünü ve tahliye politikanızı](spot-vms.md#eviction-policy)seçebilirsiniz. 

![Evet seçmek için ekran yakalama, Azure spot örneğini kullanma](media/spot-portal/yes.png)


## <a name="next-steps"></a>Sonraki adımlar

PowerShell'i kullanarak Spot [PowerShell](spot-powershell.md)VM'ler de oluşturabilirsiniz.