---
title: Azure 'da sanal makine ölçek kümeleri için düzenleme modları hakkında daha fazla bilgi edinin
description: Azure 'daki sanal makine ölçek kümeleri için düzenleme modları hakkında daha fazla bilgi edinin.
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: management
ms.date: 10/23/2019
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 54515b347a95b9315ca9ba87568fb2104c3b2b45
ms.sourcegitcommit: c4ad4ba9c9aaed81dfab9ca2cc744930abd91298
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84737011"
---
# <a name="orchestration-modes-preview"></a>Düzenleme modları (Önizleme)

> [!CAUTION]
> Bu genel önizlemeye katılmış herkes için teşekkür ederiz. Topluluğumuza değerli geri bildirim toplayabildik. Bu önizleme, geri bildirimleri bütünleştirmek için artık yeni katılımcılar için **kapalıdır** . Bu alanı yeni bilgilerle güncelleştireceğiz.

Sanal Makine Ölçek Kümeleri, platform tarafından yönetilen sanal makinelerin mantıksal bir gruplandırmasını sağlar. Ölçek kümeleri ile, bir sanal makine yapılandırma modeli oluşturur, CPU veya bellek yüküne göre ek örnekler ekler veya kaldırır ve otomatik olarak en son işletim sistemi sürümüne yükseltilir. Geleneksel olarak, ölçek kümeleri, ölçek kümesi oluşturma sırasında sağlanmış bir VM yapılandırma modeli kullanarak sanal makineler oluşturmanıza izin verir ve ölçek kümesi yalnızca yapılandırma modeline göre örtük olarak oluşturulan sanal makineleri yönetebilir.

Ölçek kümesi düzenleme modu (Önizleme) ile artık ölçek kümesinin, bir ölçek kümesi yapılandırma modeli dışında oluşturulan sanal makineleri veya yapılandırma modeline göre örtük olarak oluşturulan sanal makine örneklerini belirleyip düzenleyemeyeceğini seçebilirsiniz. Ölçek kümesi düzenleme modu Ayrıca, bu VM 'Leri hata etki alanlarına dağıtarak ve Kullanılabilirlik Alanları VM altyapınızı yüksek kullanılabilirlik için tasarlamanıza yardımcı olur.


Sanal Makine Ölçek Kümeleri 2 farklı düzenleme modunu destekleyecektir:

- ScaleSetVM – ölçek kümesine eklenen sanal makine örnekleri, ölçek kümesi yapılandırma modelini temel alır. Sanal makine örnek yaşam döngüsü oluşturma, güncelleştirme, silme-ölçek kümesi tarafından yönetilir.
- VM (sanal makineler) – ölçek kümesi dışında oluşturulan sanal makineler, ölçek kümesine açıkça eklenebilir. 
 

> [!IMPORTANT]
> Düzenleme modu, ölçek kümesi oluşturduğunuzda tanımlanır ve daha sonra değiştirilemez veya güncelleştirilemez. 
> 
> Sanal makine ölçek kümelerinin bu özelliği şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. 
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="orchestration-modes"></a>Düzenleme modları

|                             | "orchestrationMode": "VM" (VirtualMachine) | "orchestrationMode": "ScaleSetVM" (VirtualMachineScaleSetVM) |
|-----------------------------|--------------------------------------------|--------------------------------------------------------------|
| VM yapılandırma modeli      | Yok                                       | Gerekli |
| Ölçek kümesine yeni VM ekleniyor  | VM oluşturulduğunda, sanal makineler ölçek kümesine açıkça eklenir. | VM 'Ler örtük olarak oluşturulur ve VM yapılandırma modeli, örnek sayısı ve otomatik ölçeklendirme kuralları temelinde ölçek kümesine eklenir | |
| VM Silme                   | VM 'Lerin tek tek silinmesi gerekir, ölçek kümesi içinde herhangi bir VM varsa silinmeyecektir. | VM 'Ler tek tek silinebilir, ölçek kümesini silmek tüm sanal makine örneklerini siler.  |
| VM 'Leri iliştirme/ayır           | Desteklenmiyor                              | Desteklenmiyor |
| Örnek yaşam döngüsü (silme yoluyla oluşturma) | Sanal makineler ve yapıtlar (diskler ve NIC 'Ler gibi) bağımsız olarak yönetilebilir. | Örnekler ve yapıtlar (diskler ve NIC 'ler gibi), bunları oluşturan ölçek kümesi örneklerine örtülü olarak uygulanır. Ölçek kümesi dışında ayrı olarak ayrılamadığı veya yönetilemez |
| Hata etki alanları               | , Hata etki alanlarını tanımlayabilir. 2 veya 3 kullanılabilirlik alanı için bölgesel destek ve 5 ' i temel alır. | 1 ile 5 arasında bir hata etki alanı tanımlayabilir |
| Güncelleme etki alanları              | Güncelleştirme etki alanları hata etki alanlarına otomatik olarak eşlenir | Güncelleştirme etki alanları hata etki alanlarına otomatik olarak eşlenir |
| Kullanılabilirlik Alanları          | Tek bir kullanılabilirlik bölgesindeki bölgesel dağıtımı veya VM 'Leri destekler | Bölgesel dağıtımı veya birden çok Kullanılabilirlik Alanları destekler; Bölge Dengeleme stratejisini tanımlayabilir |
| Ayarının                   | Desteklenmiyor                              | Destekleniyor |
| İşletim sistemi yükseltmesi                  | Desteklenmiyor                              | Destekleniyor |
| Model güncelleştirmeleri               | Desteklenmiyor                              | Destekleniyor |
| Örnek denetimi            | Tam VM denetimi. VM 'Ler, Azure VM yönetimi yeteneklerini (Azure Ilkesi, Azure Backup ve Azure Site Recovery gibi) tam olarak destekleyen tam URI 'ye sahiptir | VM 'Ler, ölçek kümesinin bağımlı kaynaklarıdır. Örneklere yalnızca ölçek kümesi üzerinden yönetim için erişilebilir. |
| Örnek modeli              | Microsoft. COMPUTE/VirtualMachines model tanımı. | Microsoft. COMPUTE/VirtualMachineScaleSets/VirtualMachines model tanımı. |
| Kapasite                    | Boş bir ölçek kümesi oluşturulabilir; ölçek kümesine 200 adede kadar VM eklenebilir | Ölçek Kümeleri, örnek sayısı 0-1000 ile tanımlanabilir |
| Taşı                        | Destekleniyor                                  | Destekleniyor |
| Tek yerleşim grubu = = false | Desteklenmiyor                          | Destekleniyor |


## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [kullanılabilirlik seçeneklerine genel bakış](availability.md).
