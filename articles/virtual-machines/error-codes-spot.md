---
title: Azure spot VM 'Ler ve ölçek kümeleri örnekleri için hata kodları
description: Spot VM 'Leri ve ölçek kümesi örneklerini kullanırken görebileceğiniz hata kodları hakkında bilgi edinin.
author: cynthn
manager: gwallace
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/20/2019
ms.author: cynthn
ms.openlocfilehash: 1863c510967dcf80044d768aee1a37643a068ce4
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77115861"
---
# <a name="preview-error-messages-for-spot-vms-and-scale-sets"></a>Önizleme: spot VM 'Ler ve ölçek kümeleri için hata iletileri


> [!IMPORTANT]
> Nokta VM 'Leri ve sanal makine ölçek kümeleri Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


Spot VM 'Leri ve ölçek kümelerini kullanırken alacağınız bazı olası hata kodları aşağıda verilmiştir.


| Anahtar | İleti | Açıklama |
|-----|---------|-------------|
| SkuNotAvailable | '\<Resource\>' kaynağı için istenen katman Şu anda '\<SubscriptionID\>' aboneliğinin '\<location\>' konumunda kullanılamıyor. Lütfen başka bir katman deneyin veya farklı bir konuma dağıtın. | Bu konumda VM veya ölçek kümesi örneğinizi oluşturmak için yeterli sayıda Azure nokta kapasitesi yok. |
| EvictionPolicyCanBeSetOnlyOnAzureSpotVirtualMachines  |  Çıkarma ilkesi yalnızca Azure spot sanal makinelerinde ayarlanabilir. | Bu VM bir spot VM değil, çıkarma ilkesini ayarlayamazsınız. |
| AzureSpotVMNotSupportedInAvailabilitySet  |  Azure spot sanal makinesi kullanılabilirlik kümesi 'nde desteklenmiyor. | Bir spot VM kullanmayı veya bir VM 'yi bir kullanılabilirlik kümesinde kullanmayı seçmeniz gerekir, her ikisini de seçemezsiniz. |
| AzureSpotFeatureNotEnabledForSubscription  |  Abonelik, Azure spot özelliği ile etkin değil. | Spot VM 'Leri destekleyen bir aboneliğiniz olması gerekir. |
| VMPriorityCannotBeApplied  |  Belirtilen '{0}' öncelik değeri, sanal makine oluşturulduğunda hiçbir öncelik belirtilmediğinden '{1}' sanal makinesine uygulanamıyor. | VM oluşturulduğunda önceliği belirtmeniz gerekir. |
| SpotPriceGreaterThanProvidedMaxPrice  |  '{1} USD ' sunulan en büyük fiyat, '{3}' Azure spot VM boyutu için geçerli '{2} USD ' nokta fiyatından düşük olduğundan '{0}' işlemi yapılamıyor. | En yüksek fiyat ' ı seçin. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) veya [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)için fiyatlandırma bilgileri.|
| Maxpricevaluegeçersizdir  |  Geçersiz en yüksek fiyat değeri. En yüksek fiyat için desteklenen değerler-1 veya sıfırdan büyük bir ondalık. -1 ' in Max Price değeri, Azure spot sanal makinesinin fiyat nedenleriyle çıkarılmayacağını gösterir. | Geçerli bir en yüksek fiyat girin. Daha fazla bilgi için bkz. [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) veya [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/)fiyatlandırması. |
| Maxpricechangenotallodilimforallocatedvms | '{0}' sanal makinesi şu anda ayrıldığınızda, en yüksek fiyat değişikliğine izin verilmez. Lütfen serbest bırakın ve yeniden deneyin. | En fazla fiyata değişiklik yapabilmeniz için VM 'yi serbest bırakın. |
| MaxPriceChangeNotAllowed | En fazla fiyat değişikliğine izin verilmez. | Bu VM için en yüksek fiyatı değiştiremezsiniz. |
| AzureSpotIsNotSupportedForThisAPIVersion  |  Azure spot, bu API sürümü için desteklenmiyor. | API sürümünün 2019-03-01 olması gerekir. |
| AzureSpotIsNotSupportedForThisVMSize  |  Azure spot, bu VM boyutu {0}için desteklenmiyor. | Başka bir VM boyutu seçin. Daha fazla bilgi için bkz. [sanal makineleri spot](./linux/spot-vms.md). |
| MaxPriceIsSupportedOnlyForAzureSpotVirtualMachines  |  En yüksek fiyat yalnızca Azure spot sanal makineler için desteklenir. | Daha fazla bilgi için bkz. [sanal makineleri spot](./linux/spot-vms.md). |
| MoveResourcesWithAzureSpotVMNotSupported  |  Kaynakları taşıma isteği bir Azure spot sanal makinesi içerir. Bu şu anda desteklenmiyor. Lütfen sanal makine kimlikleri için hata ayrıntılarını kontrol edin. | Spot VM 'Leri taşıyamazsınız. |
| MoveResourcesWithAzureSpotVmssNotSupported  |  Kaynakları taşıma isteği bir Azure spot sanal makine ölçek kümesi içerir. Bu şu anda desteklenmiyor. Lütfen sanal makine ölçek kümesi kimlikleri için hata ayrıntılarını kontrol edin. | Nokta ölçeği kümesi örneklerini taşıyamazsınız. |
| EphemeralOSDisksNotSupportedForSpotVMs | Kısa ömürlü işletim sistemi diskleri, spot VM 'Ler için desteklenmez. | Spot VM 'niz için düzenli bir işletim sistemi diski kullanmanız gerekir. |
| AzureSpotVMNotSupportedInVmssWithVMOrchestrationMode | Azure spot sanal makinesi, VM düzenleme modu ile sanal makine ölçek kümesinde desteklenmez. | Spot örnekleri kullanmak için Orchestration modunu sanal makine ölçek kümesine ayarlayın. |


**Sonraki adımlar** Daha fazla bilgi için bkz. [sanal makineleri spot](./linux/spot-vms.md).