---
title: Azure Spot VM'leri dağıtmak için portalı kullanın
description: Maliyetlerden tasarruf etmek için Spot VM'leri dağıtmak için Azure PowerShell'i nasıl kullanacağınızı öğrenin.
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 03/25/2020
ms.author: cynthn
ms.openlocfilehash: 045cec080b9b1b8f2e4cb589b053c421897db5be
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80547378"
---
# <a name="deploy-spot-vms-using-the-azure-portal"></a>Azure portalını kullanarak Spot VM'leri dağıtma

[Spot VM'leri](spot-vms.md) kullanmak, önemli bir maliyet tasarrufu nda kullanılmayan kapasitemizden yararlanmanızı sağlar. Azure'un kapasiteye geri ihtiyacı olduğu herhangi bir zamanda, Azure altyapısı Spot VM'leri boşaltacaktır. Bu nedenle, Spot VM'ler toplu iş işleri, geliştirme/test ortamları, büyük bilgi işlem iş yükleri ve daha fazlası gibi kesintileri işleyebilir iş yükleri için idealdir.

Spot VM'ler için fiyatlandırma, bölgeye ve SKU'ya göre değişkendir. Daha fazla bilgi için [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) ve [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için VM fiyatlandırması için bkz. Maksimum fiyatı ayarlama hakkında daha fazla bilgi için [Spot VM'ler - Fiyatlandırma'ya](spot-vms.md#pricing)bakın.

VM için saat başına ödemek istediğiniz maksimum fiyatı belirleme seçeneğiniz vardır. Spot VM'nin maksimum fiyatı, 5 ondalık basamak kullanılarak ABD doları (USD) olarak ayarlanabilir. Örneğin, değeri `0.05701`saat başına 0,05701 USD maksimum fiyat olacaktır. Maksimum fiyatı olarak `-1`ayarlarsanız, VM fiyata göre tahliye olmaz. VM için fiyat spot veya standart bir VM için fiyat olacak, hangi hiç daha az, sürece kapasite ve kota mevcuttur.


## <a name="create-the-vm"></a>Sanal makine oluşturma

Spot VM kullanan bir VM oluşturma [işlemi, hızlı başlatmada](quick-create-portal.md)ayrıntılı olarak aynıdır. Bir VM dağıtırken, bir Azure spot örneğini kullanmayı seçebilirsiniz.


Temel **Bilgiler** sekmesinde, **Örnek ayrıntıları** bölümünde, **Hayır,** Azure spot örneğini kullanmak için varsayılan dır.

![Hayır seçmek için ekran yakalama, Azure spot örneği kullanmayın](media/spot-portal/no.png)

**Evet'i**seçerseniz, bölüm genişler ve [tahliye türünü ve tahliye politikanızı](spot-vms.md#eviction-policy)seçebilirsiniz. 

![Evet seçmek için ekran yakalama, Azure spot örneğini kullanma](media/spot-portal/yes.png)


## <a name="next-steps"></a>Sonraki adımlar

PowerShell'i kullanarak Spot [PowerShell](spot-powershell.md)VM'ler de oluşturabilirsiniz.