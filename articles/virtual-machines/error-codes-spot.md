---
title: Azure Spot VM'leri ve ölçek kümeleri örnekleri için hata kodları
description: Spot VM'leri kullanırken ve örneklerini ölçeklendirirken görebileceğiniz hata kodları hakkında bilgi edinin.
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/20/2019
ms.author: cynthn
ms.openlocfilehash: 1863c510967dcf80044d768aee1a37643a068ce4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77115861"
---
# <a name="preview-error-messages-for-spot-vms-and-scale-sets"></a>Önizleme: Spot VM'ler ve ölçek kümeleri için hata iletileri


> [!IMPORTANT]
> Spot Sanal Ayarlar ve sanal makine ölçek kümeleri şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.


Burada, Spot VM'leri ve ölçek kümelerini kullanırken alabileceğiniz bazı olası hata kodları verebilirsiniz.


| Anahtar | İleti | Açıklama |
|-----|---------|-------------|
| SkuNotAvailable | Kaynak için istenen katman\<\>' kaynak ' şu\<anda abonelik\<için\>' ' ' için konum ' '\>adresinde kullanılamaz. Lütfen başka bir katman deneyin veya farklı bir konuma dağıtın. | Bu konumda VM veya ölçek seti örneğioluşturmak için yeterli Azure Spot kapasitesi yoktur. |
| TahliyePolitikasıCanBeSetOnlyOnAzureSpotVirtualMachines  |  Çıkarma ilkesi yalnızca Azure Spot Sanal Makineler'de ayarlanabilir. | Bu VM bir Spot VM değildir, bu nedenle tahliye ilkesini ayarlayamazsınız. |
| AzureSpotVMNotSupportedInAvailabilitySet  |  Azure Spot Sanal Makine Kullanılabilirlik Kümesi'nde desteklenmez. | Spot VM kullanmayı veya kullanılabilirlik kümesinde VM kullanmayı seçmeniz gerekir, ikisini birden seçemezsiniz. |
| AzureSpotFeatureNotEnabledForSubscription  |  Azure Spot özelliği ile abonelik etkinleştirildi. | Spot VM'leri destekleyen bir aboneliğe sahip olmanız gerekir. |
| VMPriorityuygulanamaz  |  Sanal Makine oluşturulduğunda öncelik belirtilmediği için belirtilen öncelik değeri ' ' ' '{0}Sanal Makine{1}' ye uygulanamaz. | VM oluşturulduğunda önceliği belirtmeniz gerekir. |
| SpotPriceGreaterThanProvidedMaxPrice  |  İşlem icra edilemediği için{0}'{1} ' sağlanan maksimum fiyat '{2} USD' mevcut spot fiyattan{3}daha düşük ' USD' için Azure Spot VM boyutu ' ' | Daha yüksek bir maksimum fiyat seçin. Daha fazla bilgi için [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) veya [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için fiyatlandırma bilgilerine bakın.|
| MaxPriceValueGeçersiz  |  Geçersiz maksimum fiyat değeri. Maksimum fiyat için desteklenen tek değerler -1 veya sıfırdan büyük ondalık değerlerdir. -1'in maksimum fiyat değeri, Azure Spot sanal makinesinin fiyat nedenleriyle tahliye edilmeyeceğini gösterir. | Geçerli bir maksimum fiyat girin. Daha fazla bilgi için [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) veya [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)fiyatlandırması için bkz. |
| MaxPriceChangeNotAllowedForAllocatedVMs | VM ' '{0}şu anda tahsis edildiğinde maksimum fiyat değişikliğine izin verilmez. Lütfen anlaşma ve tekrar deneyin. | Stop\Deallocate VM böylece maksimum fiyat değiştirebilirsiniz. |
| MaxPriceChangeNotallowed | Maksimum fiyat değişikliğine izin verilmez. | Bu VM için maksimum fiyatı değiştiremezsiniz. |
| AzureSpotIsNotSupportedForThisAPIVersion  |  Azure Spot bu API sürümü için desteklenmez. | API sürümü 2019-03-01 olması gerekir. |
| AzureSpotIsNotSupportedForThisVMSize  |  Azure Spot bu VM boyutu {0}için desteklenmez. | Başka bir VM boyutu seçin. Daha fazla bilgi için [Bkz. Spot Sanal Makineler.](./linux/spot-vms.md) |
| MaxPriceIsSupportedOnlyForAzureSpotVirtualMachines  |  Maksimum fiyat yalnızca Azure Spot Sanal Makineler için desteklenir. | Daha fazla bilgi için [Bkz. Spot Sanal Makineler.](./linux/spot-vms.md) |
| MoveResourcesWithAzureSpotVMNotSupported  |  Kaynakları Taşı isteği bir Azure Spot sanal makinesi içerir. Bu şu anda desteklenmiyor. Lütfen sanal makine kimlikleri için hata ayrıntılarını kontrol edin. | Spot VM'leri taşıyamazsınız. |
| MoveResourcesWithAzureSpotVmssNotSupported  |  Kaynakları Taşı isteği, bir Azure Spot sanal makine ölçeği kümesi içerir. Bu şu anda desteklenmiyor. Sanal makine ölçeği kümesi Id'ler için lütfen hata ayrıntılarını kontrol edin. | Spot ölçek kümesi örneklerini taşıyamazsınız. |
| EfemeralOSDisksNotSupportedForSpotVMs | Nokta VM'ler için geçici işletim sistemi diskleri desteklenmez. | Spot VM'iniz için normal bir işletim sistemi diski kullanıyor olmanız gerekir. |
| AzureSpotVMNotSupportedInVmssWithVMOrchestrationMode | Azure Spot Sanal Makine, VM Orkestrasyon moduile Sanal Makine Ölçeği Seti'nde desteklenmez. | Düzenleme modunu Spot örneklerini kullanmak için sanal makine ölçeğine ayarlayın. |


**Sonraki adımlar** Daha fazla bilgi için [spot Sanal Makineler'e](./linux/spot-vms.md)bakın.