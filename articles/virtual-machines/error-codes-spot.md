---
title: Azure spot VM 'Ler ve ölçek kümeleri örnekleri için hata kodları
description: Spot VM 'Leri ve ölçek kümesi örneklerini kullanırken görebileceğiniz hata kodları hakkında bilgi edinin.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: troubleshooting
ms.date: 03/25/2020
ms.author: cynthn
ms.openlocfilehash: e5e621cc3763cfa7fe28790baf2f5d9866c8d618
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87069798"
---
# <a name="error-messages-for-spot-vms-and-scale-sets"></a>Spot VM 'Ler ve ölçek kümeleri için hata iletileri

Spot VM 'Leri ve ölçek kümelerini kullanırken alacağınız bazı olası hata kodları aşağıda verilmiştir.


| Anahtar | İleti | Açıklama |
|-----|---------|-------------|
| SkuNotAvailable | ' ' Kaynağı için istenen katman \<resource\> Şu anda ' ' \<location\> aboneliği için ' ' konumunda kullanılamıyor \<subscriptionID\> . Lütfen başka bir katman deneyin veya farklı bir konuma dağıtın. | Bu konumda VM veya ölçek kümesi örneğinizi oluşturmak için yeterli sayıda Azure nokta kapasitesi yok. |
| EvictionPolicyCanBeSetOnlyOnAzureSpotVirtualMachines  |  Çıkarma ilkesi yalnızca Azure spot sanal makinelerinde ayarlanabilir. | Bu VM bir spot VM değil, çıkarma ilkesini ayarlayamazsınız. |
| AzureSpotVMNotSupportedInAvailabilitySet  |  Azure spot sanal makinesi kullanılabilirlik kümesi 'nde desteklenmiyor. | Bir spot VM kullanmayı veya bir VM 'yi bir kullanılabilirlik kümesinde kullanmayı seçmeniz gerekir, her ikisini de seçemezsiniz. |
| AzureSpotFeatureNotEnabledForSubscription  |  Abonelik, Azure spot özelliği ile etkin değil. | Spot VM 'Leri destekleyen bir abonelik kullanın. |
| VMPriorityCannotBeApplied  |  Belirtilen ' ' öncelik değeri, {0} {1} sanal makine oluşturulduğunda hiçbir öncelik belirtilmediğinden ' ' sanal makinesine uygulanamıyor. | VM oluşturulduğunda önceliği belirtin. |
| SpotPriceGreaterThanProvidedMaxPrice  |  {0}Belirtilen en büyük fiyat ' {1} USD ', ' {2} ' Azure spot VM boyutu için geçerli olan ' USD ' nokta fiyatından düşük olduğundan ' ' işlemi gerçekleştirilemiyor {3} . | En yüksek fiyat ' ı seçin. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) veya [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için fiyatlandırma bilgileri.|
| Maxpricevaluegeçersizdir  |  Geçersiz en yüksek fiyat değeri. En yüksek fiyat için desteklenen değerler-1 veya sıfırdan büyük bir ondalık. -1 ' in Max Price değeri, Azure spot sanal makinesinin fiyat nedenleriyle çıkarılmayacağını gösterir. | Geçerli bir en yüksek fiyat girin. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) veya [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)fiyatlandırması. |
| Maxpricechangenotallodilimforallocatedvms | ' ' Sanal makinesi {0} Şu anda ayrıldığınızda en yüksek fiyat değişikliğine izin verilmez. Lütfen serbest bırakın ve yeniden deneyin. | En fazla fiyata değişiklik yapabilmeniz için VM 'yi serbest bırakın. |
| MaxPriceChangeNotAllowed | En fazla fiyat değişikliğine izin verilmez. | Bu VM için en yüksek fiyatı değiştiremezsiniz. |
| AzureSpotIsNotSupportedForThisAPIVersion  |  Azure spot, bu API sürümü için desteklenmiyor. | API sürümünün 2019-03-01 olması gerekir. |
| AzureSpotIsNotSupportedForThisVMSize  |  Azure spot, bu VM boyutu için desteklenmiyor {0} . | Başka bir VM boyutu seçin. Daha fazla bilgi için bkz. [sanal makineleri spot](./linux/spot-vms.md). |
| MaxPriceIsSupportedOnlyForAzureSpotVirtualMachines  |  En yüksek fiyat yalnızca Azure spot sanal makineler için desteklenir. | Daha fazla bilgi için bkz. [sanal makineleri spot](./linux/spot-vms.md). |
| MoveResourcesWithAzureSpotVMNotSupported  |  Kaynakları taşıma isteği bir Azure spot sanal makinesi içerir. Bu şu anda desteklenmiyor. Lütfen sanal makine kimlikleri için hata ayrıntılarını kontrol edin. | Spot VM 'Leri taşıyamazsınız. |
| MoveResourcesWithAzureSpotVmssNotSupported  |  Kaynakları taşıma isteği bir Azure spot sanal makine ölçek kümesi içerir. Bu şu anda desteklenmiyor. Lütfen sanal makine ölçek kümesi kimlikleri için hata ayrıntılarını kontrol edin. | Nokta ölçeği kümesi örneklerini taşıyamazsınız. |
| AzureSpotVMNotSupportedInVmssWithVMOrchestrationMode | Azure spot sanal makinesi, VM düzenleme modu ile sanal makine ölçek kümesinde desteklenmez. | Spot örnekleri kullanmak için Orchestration modunu sanal makine ölçek kümesine ayarlayın. |


**Sonraki adımlar** Daha fazla bilgi için bkz. [sanal makineleri spot](./linux/spot-vms.md).