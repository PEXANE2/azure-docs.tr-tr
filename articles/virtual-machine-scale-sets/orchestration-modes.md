---
title: Azure 'da sanal makine ölçek kümeleri için düzenleme modları hakkında daha fazla bilgi edinin
description: Azure 'daki sanal makine ölçek kümeleri için düzenleme modları hakkında daha fazla bilgi edinin.
author: shandilvarun
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: vashan
ms.openlocfilehash: 4a0be30f181921461ad0bacea6f18ce439d22353
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2020
ms.locfileid: "76279057"
---
# <a name="orchestration-mode-preview"></a>Düzenleme modu (Önizleme)

Sanal Makine Ölçek Kümeleri, platform tarafından yönetilen sanal makinelerin mantıksal bir gruplandırmasını sağlar. Ölçek kümeleri ile, bir sanal makine yapılandırma modeli oluşturur, CPU veya bellek yüküne göre ek örnekler ekler veya kaldırır ve otomatik olarak en son işletim sistemi sürümüne yükseltilir. Geleneksel olarak, ölçek kümeleri, ölçek kümesi oluşturma sırasında sağlanmış bir VM yapılandırma modeli kullanarak sanal makineler oluşturmanıza izin verir ve ölçek kümesi yalnızca yapılandırma modeline göre örtük olarak oluşturulan sanal makineleri yönetebilir.

Ölçek kümesi düzenleme modu (Önizleme) ile artık ölçek kümesinin, bir ölçek kümesi yapılandırma modelinin dışında bir şekilde oluşturulan sanal makineleri veya sanal makine örneklerinin yapılandırma modeli. Ölçek kümesi düzenleme modu Ayrıca, bu VM 'Leri hata etki alanlarına dağıtarak ve Kullanılabilirlik Alanları VM altyapınızı yüksek kullanılabilirlik için tasarlamanıza yardımcı olur.


Sanal Makine Ölçek Kümeleri 2 farklı düzenleme modunu destekleyecektir:

- ScaleSetVM – ölçek kümesine eklenen sanal makine örnekleri, ölçek kümesi yapılandırma modelini temel alır. Sanal makine örnek yaşam döngüsü oluşturma, güncelleştirme, silme-ölçek kümesi tarafından yönetilir.
- VM (sanal makineler) – ölçek kümesi dışında oluşturulan sanal makineler, ölçek kümesine açıkça eklenebilir. 
 

> [!IMPORTANT]
> Düzenleme modu, ölçek kümesi oluşturduğunuzda tanımlanır ve daha sonra değiştirilemez veya güncelleştirilemez. 
> 
> Sanal makine ölçek kümelerinin bu özelliği şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. 
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="orchestration-modes"></a>Düzenleme modları

|                             | "orchestrationMode": "VM" (VirtualMachine) | "orchestrationMode": "ScaleSetVM" (VirtualMachineScaleSetVM) |
|-----------------------------|--------------------------------------------|--------------------------------------------------------------|
| VM yapılandırma modeli      | Hiçbiri                                       | Gereklidir |
| Ölçek kümesine yeni VM ekleniyor  | VM oluşturulduğunda, sanal makineler ölçek kümesine açıkça eklenir. | VM 'Ler örtük olarak oluşturulur ve VM yapılandırma modeli, örnek sayısı ve otomatik ölçeklendirme kuralları temelinde ölçek kümesine eklenir | |
| VM silme                   | VM 'Lerin tek tek silinmesi gerekir, ölçek kümesi içinde herhangi bir VM varsa silinmeyecektir. | VM 'Ler tek tek silinebilir, ölçek kümesini silmek tüm sanal makine örneklerini siler.  |
| VM 'Leri iliştirme/ayır           | Desteklenmiyor                              | Desteklenmiyor |
| Örnek yaşam döngüsü (silme yoluyla oluşturma) | Sanal makineler ve yapıtlar (diskler ve NIC 'Ler gibi) bağımsız olarak yönetilebilir. | Örnekler ve yapıtlar (diskler ve NIC 'ler gibi), bunları oluşturan ölçek kümesi örneklerine örtülü olarak uygulanır. Ölçek kümesi dışında ayrı olarak ayrılamadığı veya yönetilemez |
| Hata etki alanları               | , Hata etki alanlarını tanımlayabilir. 2 veya 3 kullanılabilirlik alanı için bölgesel destek ve 5 ' i temel alır. | 1 ile 5 arasında bir hata etki alanı tanımlayabilir |
| Güncelleme etki alanları              | Güncelleştirme etki alanları hata etki alanlarına otomatik olarak eşlenir | Güncelleştirme etki alanları hata etki alanlarına otomatik olarak eşlenir |
| Kullanılabilirlik Alanları          | Tek bir kullanılabilirlik bölgesindeki bölgesel dağıtımı veya VM 'Leri destekler | Bölgesel dağıtımı veya birden çok Kullanılabilirlik Alanları destekler; Bölge Dengeleme stratejisini tanımlayabilir |
| Otomatik Ölçeklendirme                   | Desteklenmiyor                              | Desteklenen |
| İşletim sistemi yükseltmesi                  | Desteklenmiyor                              | Desteklenen |
| Model güncelleştirmeleri               | Desteklenmiyor                              | Desteklenen |
| Örnek denetimi            | Tam VM denetimi. VM 'Ler, Azure VM yönetimi yeteneklerini (Azure Ilkesi, Azure Backup ve Azure Site Recovery gibi) tam olarak destekleyen tam URI 'ye sahiptir | VM 'Ler, ölçek kümesinin bağımlı kaynaklarıdır. Örneklere yalnızca ölçek kümesi üzerinden yönetim için erişilebilir. |
| Örnek modeli              | Microsoft. COMPUTE/VirtualMachines model tanımı. | Microsoft. COMPUTE/VirtualMachineScaleSets/VirtualMachines model tanımı. |
| Kapasite                    | Boş bir ölçek kümesi oluşturulabilir; ölçek kümesine 200 adede kadar VM eklenebilir | Ölçek Kümeleri, örnek sayısı 0-1000 ile tanımlanabilir |
| Taşı                        | Desteklenen                                  | Desteklenen |
| Tek yerleşim grubu = = false | Desteklenmiyor                          | Desteklenen |


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [kullanılabilirlik seçeneklerine genel bakış](availability.md).
