---
title: Azure'da sanal makine ölçeği kümeleri için düzenleme modları hakkında daha fazla bilgi edinin
description: Azure'da sanal makine ölçeği kümeleri için düzenleme modları hakkında daha fazla bilgi edinin.
author: shandilvarun
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.topic: conceptual
ms.date: 10/23/2019
ms.author: vashan
ms.openlocfilehash: 4a0be30f181921461ad0bacea6f18ce439d22353
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76279057"
---
# <a name="orchestration-mode-preview"></a>Düzenleme modu (önizleme)

Sanal makine ölçek kümeleri, platform tarafından yönetilen sanal makinelerin mantıksal bir gruplandırması sağlar. Ölçek kümeleri ile sanal bir makine yapılandırma modeli oluşturur, CPU veya bellek yüküne dayalı ek örnekleri otomatik olarak ekler veya kaldırır ve otomatik olarak en son işletim sistemi sürümüne yükseltebilirsiniz. Geleneksel olarak, ölçek kümeleri ölçek kümesi oluşturma sırasında sağlanan bir VM yapılandırma modelini kullanarak sanal makineler oluşturmanıza olanak sağlar ve ölçek kümesi yalnızca yapılandırma modeline dayalı olarak örtülü olarak oluşturulan sanal makineleri yönetebilir.

Ölçek kümesi düzenleme modu (önizleme) ile, artık ölçek kümesinin bir ölçek kümesi yapılandırma modelinin dışında açıkça oluşturulan sanal makineleri mi yoksa yapılandırma modeli. Ölçek seti orkestrasyon modu, bu VM'leri hata etki alanlarında ve Kullanılabilirlik Bölgelerinde dağıtarak VM altyapınızı yüksek kullanılabilirlik için tasarlamanıza da yardımcı olur.


Sanal makine ölçek kümeleri 2 farklı orkestrasyon modunu destekler:

- ScaleSetVM – Ölçek kümesine eklenen sanal makine örnekleri ölçek kümesi yapılandırma modelini temel alınmaktadır. Sanal makine örneği yaşam döngüsü - oluşturma, güncelleştirme, silme - ölçek kümesi tarafından yönetilir.
- VM (sanal makineler) – Ölçek kümesi dışında oluşturulan sanal makineler ölçek kümesine açıkça eklenebilir. 
 

> [!IMPORTANT]
> Ölçek kümesini oluşturduğunuzda düzenleme modu tanımlanır ve daha sonra değiştirilemez veya güncelleştirilemez. 
> 
> Sanal makine ölçek kümelerinin bu özelliği şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. 
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.


## <a name="orchestration-modes"></a>Düzenleme modları

|                             | "orchestrationMode": "VM" (VirtualMachine) | "orchestrationMode": "ScaleSetVM" (VirtualMachineScaleSetVM) |
|-----------------------------|--------------------------------------------|--------------------------------------------------------------|
| VM yapılandırma modeli      | None                                       | Gerekli |
| Ölçek Kümesine Yeni VM Ekleme  | VM oluşturulduğunda VM'ler ölçek kümesine açıkça eklenir. | VM'ler örtülü olarak oluşturulur ve VM yapılandırma modeli, örnek sayısı ve Otomatik Ölçeklendirme kurallarına göre ölçek kümesine eklenir | |
| VM Silme                   | VM'ler tek tek silinmeli, ölçek kümesi içinde VM varsa silinmez. | VM'ler tek tek silinebilir, ölçek kümesi silinerek tüm VM örnekleri silinir.  |
| VM'leri ekleme/ayırma           | Desteklenmiyor                              | Desteklenmiyor |
| Örnek Yaşam Döngüsü (Silme yoluyla Oluşturma) | VM'ler ve yapıları (diskler ve NIC'ler gibi) bağımsız olarak yönetilebilir. | Örnekler ve bunların yapıları (diskler ve NIC'ler gibi) bunları oluşturan ölçek kümesi örneklerine örtülüdür. Bunlar ölçek kümesi dışında ayrı ayrı ayrılamaz veya yönetilemez |
| Hata etki alanları               | Hata etki alanlarını tanımlayabilir. Bölgesel desteğe dayalı 2 veya 3 ve Kullanılabilirlik bölgesi için 5. | 1'den 5'e kadar olan hata etki alanlarını tanımlayabilir |
| Güncelleme etki alanları              | Güncelleştirme etki alanları otomatik olarak hata etki adlarına eşlenir | Güncelleştirme etki alanları otomatik olarak hata etki adlarına eşlenir |
| Kullanılabilirlik Alanları          | Tek bir Kullanılabilirlik Bölgesinde bölgesel dağıtımı veya VM'leri destekler | Bölgesel dağıtımı veya birden çok Kullanılabilirlik Bölgesini destekler; Bölge dengeleme stratejisini tanımlayabilir |
| Otomatik Ölçeklendirme                   | Desteklenmiyor                              | Destekleniyor |
| İşletim sistemi yükseltmesi                  | Desteklenmiyor                              | Destekleniyor |
| Model güncellemeleri               | Desteklenmiyor                              | Destekleniyor |
| Örnek denetimi            | Tam VM Kontrolü. VM'ler, tüm Azure VM yönetim özelliklerini (Azure İlkesi, Azure Yedeklemesi ve Azure Site Kurtarma gibi) destekleyen tam nitelikli URI'ye sahiptir | VM'ler ölçek kümesinin bağımlı kaynaklarıdır. Örneklere yalnızca ölçek kümesi üzerinden yönetim için erişilebilir. |
| Örnek Modeli              | Microsoft.Compute/VirtualMachines model tanımı. | Microsoft.Compute/VirtualMachineScaleSets/VirtualMachines model tanımı. |
| Kapasite                    | Boş bir ölçek kümesi oluşturulabilir; ölçek kümesine 200 VM'ye kadar eklenebilir | Ölçek kümeleri bir örnek sayısı 0 - 1000 ile tanımlanabilir |
| Taşı                        | Destekleniyor                                  | Destekleniyor |
| Tek yerleşim grubu == yanlış | Desteklenmiyor                          | Destekleniyor |


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [kullanılabilirlik seçeneklerine Genel Bakış](availability.md)bölümüne bakın.
