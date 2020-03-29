---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 11/04/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 4860dcac666f790fed199536338e50a967113c20
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76748937"
---
Bu makalede, Azure sanal makinelerin (VM) kullanılabilirlik özelliklerine genel bir bakış sağlanmaktadır.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

İş yükleri genellikle yüksek iş, performans kazanmak ve bir VM'nin bir güncelleştirme veya başka bir olay nedeniyle etkilenmesi durumunda fazlalık oluşturmak için farklı sanal makinelere yayılır. 

Azure'un Yüksek Kullanılabilirlik elde etmek için sağladığı birkaç seçenek vardır. Önce temel yapılardan bahsedelim. 

### <a name="availability-zones"></a>Kullanılabilirlik alanları

[Kullanılabilirlik bölgeleri,](../articles/availability-zones/az-overview.md) Sanal M'lerinizdeki uygulamaların ve verilerin kullanılabilirliğini korumak için gereken denetim düzeyini genişletir. Kullanılabilirlik Bölgesi, Azure bölgesinde fiziksel olarak ayrı bir bölgedir. Desteklenen Azure bölgesi başına üç Kullanılabilirlik Bölgesi vardır. 

Her Kullanılabilirlik Alanı farklı bir güç kaynağı, ağ ve soğutma sistemine sahiptir. Çözümlerinizi, yinelenen VM'leri bölgeler halinde kullanacak şekilde tasarlayarak, uygulamalarınızı ve verilerinizi bir veri merkezi nin kaybına karşı koruyabilirsiniz. Bir bölge tehlikeye girerse, çoğaltılan uygulamalar ve veriler anında başka bir bölgede kullanılabilir. 

![Kullanılabilirlik alanları](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Kullanılabilirlik Bölgesinde [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) veya [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) VM dağıtma hakkında daha fazla bilgi edinin.


### <a name="fault-domains"></a>Hata etki alanları

Hata etki alanı, ortak bir güç kaynağı ve ağ anahtarını paylaşan, şirket içi veri merkezindeki rafa benzer bir temel alınan donanım mantık grubudur. 

### <a name="update-domains"></a>Güncelleme etki alanları

Güncelleme etki alanı, bakımdan geçirilebilen ya da aynı anda yeniden başlatılabilen bir temel alınan donanım mantıksal grubudur. 

Bu yaklaşım, Azure platformu periyodik bakımdan geçirilirken uygulamanızın en az bir örneğinin her zaman çalışır durumda kalmasını sağlar. Yeniden başlatılan güncelleştirme etki alanlarının sırası bakım sırasında sırayla devam edemeyebilir, ancak aynı anda yalnızca bir güncelleştirme etki alanı yeniden başlatılır.


## <a name="virtual-machines-scale-sets"></a>Sanal Makineler Ölçek Setleri 

Azure sanal makine ölçek kümeleri, bir grup yük dengeli VM'si oluşturmanıza ve yönetmenize izin tanır. Tanımlı bir zamanlamaya veya talebe yanıt olarak sanal makine örneği sayısı otomatik olarak artabilir ya da azalabilir. Ölçek kümeleri uygulamalarınızda yüksek kullanılabilirlik sağlar ve birçok VM'yi merkezi olarak yönetmenize, yapılandırmanıza ve güncelleştirmenize olanak sağlar. Yüksek oranda kullanılabilir bir uygulama sağlamak ve [%99,95 Azure SLA'sını](https://azure.microsoft.com/support/legal/sla/virtual-machines/)karşılamak için bir ölçek kümesi içinde iki veya daha fazla VM oluşturulmasını önerdik. Ölçek kümesinin kendisi için herhangi bir maliyet yoktur, yalnızca oluşturduğunuz her VM örneği için ödeme. Tek bir VM [Azure premium SSD'leri](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd)kullanıyorsa, Azure SLA planlanmamış bakım etkinlikleri için geçerlidir. Bir ölçek kümesindeki sanal makineler, veri merkezi kesintileri ve planlanmış veya planlanmamış bakım olayları nedeniyle kullanılabilirliği ve kesintilere karşı dayanıklılığı en üst düzeye çıkarmak için birden çok güncelleştirme etki alanı ve hata etki alanında dağıtılabilir. Bir ölçek kümesindeki sanal makineler, tek bir Kullanılabilirlik bölgesine veya bölgesel olarak da dağıtılabilir. Kullanılabilirlik bölgesi dağıtım seçenekleri, düzenleme moduna bağlı olarak farklılık gösterebilir.

### <a name="preview-orchestration-mode-preview"></a>Önizleme: Düzenleme modu Önizleme
Sanal makineler ölçek kümeleri, düzenleme modunu belirtmenize olanak sağlar.  Sanal makine ölçeği ayarlı orkestrasyon modu (önizleme) ile, artık ölçek kümesinin bir ölçek kümesi yapılandırma modelinin dışında açıkça oluşturulan sanal makineleri mi yoksa örtülü olarak oluşturulan sanal makine örneklerini mi düzenlemesi gerektiğini seçebilirsiniz yapılandırma modeline göre. VM düzenleme modelinin açıkça tanımlanmış Sanal Makineleri bir bölgede veya kullanılabilirlik bölgesinde bir araya getiremenize olanak tanıyan düzenleme modunu seçin. Kullanılabilirlik Bölgesinde dağıtılan sanal makineler VM'lere bölge yalıtımı sağlar, kullanılabilirlik bölgesi sınırına bağlıdırlar ve bölgedeki diğer kullanılabilirlik bölgesinde oluşabilecek hatalara maruz kalırlar. 

|   | "orchestrationMode": "VM" (VirtualMachine)| "orchestrationMode": "ScaleSetVM" (VirtualMachineScaleSetVM) |
|----|----|----|
| VM yapılandırma modeli| Yok. VirtualMachineProfile ölçek kümesi modelinde tanımsız. | Gereklidir. VirtualMachineProfile ölçek kümesi modelinde doldurulur. |
| Ölçek Kümesine Yeni VM Ekleme| VM oluşturulduğunda VM'ler ölçek kümesine açıkça eklenir. | VM'ler örtülü olarak oluşturulur ve VM yapılandırma modeli, örnek sayısı ve Otomatik Ölçeklendirme kurallarına dayalı ölçek kümesine eklenir. |
| Kullanılabilirlik Alanları| Tek bir Kullanılabilirlik Bölgesinde bölgesel dağıtımı veya VM'leri destekler| Bölgesel dağıtımı veya birden çok Kullanılabilirlik Bölgesini destekler; Bölge dengeleme stratejisini tanımlayabilir |
| Hata etki alanları| Hata etki alanları sayısını tanımlayabilir. Bölgesel desteğe dayalı 2 veya 3 ve Kullanılabilirlik bölgesi için 5. Atanan VM hata etki alanı, anlaşma nın yerini bulma ve yeniden başlatma dahil olmak üzere VM yaşam döngüsüyle devam edecektir. | Bölge dışı dağıtımlar için 1, 2 veya 3 hata etki alanı ve Kullanılabilirlik bölgesi dağıtımları için 5 tanımlayabilir. Atanan VM hata etki alanı VM yaşam döngüsü ile devam etmez, sanal makineler tahsis sırasında bir hata etki alanı atanır. |
| Güncelleme etki alanları| Yok. Güncelleştirme etki alanları otomatik olarak hata etki adlarına eşlenir| Yok. Güncelleştirme etki alanları otomatik olarak hata etki adlarına eşlenir |

**Etki alanlarını hata ve güncelleştirme etki alanları**

Sanal makine ölçeği kümeleri, hata etki alanlarını hizalayarak ve etki alanlarını güncelleştirerek yüksek kullanılabilirlik için tasarımı basitleştirir. Yalnızca ölçek kümesi için hata etki alanları sayısını tanımlamanız gerekir. Ölçek kümeleri için kullanılabilen hata etki alanlarının sayısı bölgeye göre değişebilir. Bkz. [Azure'daki sanal makinelerin kullanılabilirliğini yönetin.](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability)


## <a name="availability-sets"></a>Kullanılabilirlik kümeleri
Kullanılabilirlik kümesi, Azure'un artıklık ve kullanılabilirlik sağlamak için uygulamanızın nasıl oluşturulup oluşturulmasını anlamasını sağlayan bir veri merkezindeki Sanal M'lerin mantıksal bir gruplandırmasIdır. Yüksek oranda kullanılabilir bir uygulama sağlamak ve [%99,95 Azure SLA'sını](https://azure.microsoft.com/support/legal/sla/virtual-machines/)karşılamak için kullanılabilirlik kümesi içinde iki veya daha fazla VM oluşturulmasını önerdik. Kullanılabilirlik Kümesi'nin kendisi için herhangi bir maliyet yoktur, yalnızca oluşturduğunuz her VM örneği için ödeme yapılır. Tek bir VM [Azure premium SSD'leri](../articles/virtual-machines/windows/disks-types.md#premium-ssd)kullanıyorsa, Azure SLA planlanmamış bakım etkinlikleri için geçerlidir.

Kullanılabilirlik kümesinde, VM'ler bu hata etki alanlarında otomatik olarak dağıtılır. Bu yaklaşım, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkisini sınırlar.

[Azure Yönetilen Diskler](../articles/virtual-machines/windows/faq-for-disks.md)’i kullanan sanal makineler, yönetilen kullanılabilirlik kümesi kullanılırken yönetilen disk hata etki alanları ile hizalanır. Bu hizalama, bir VM'ye bağlı tüm yönetilen disklerin, aynı yönetilen disk hata etki alanı içinde olmasını sağlar. 

Yönetilen bir kullanılabilirlik kümesinde yalnızca, yönetilen disklere sahip VM’ler oluşturulabilir. Yönetilen disk hata etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki ya da üç yönetilen disk hata etki alanı). [Linux VM'ler](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) veya Windows [VM'ler](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set)için yönetilen disk hatası etki alanları hakkında daha fazla bilgi edinebilirsiniz.

![Yönetilen kullanılabilirlik kümesi](./media/virtual-machines-common-manage-availability/md-fd-updated.png)


Kullanılabilirlik kümesi içindeki VM'ler de güncelleştirme etki alanlarında otomatik olarak dağıtılır. 

![Kullanılabilirlik kümeleri](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure ortamınızı oluşturmak için bu kullanılabilirlik ve yedeklilik özelliklerini kullanmaya başlayabilirsiniz. En iyi uygulama bilgileri için bkz. [Azure kullanılabilirlik en iyi uygulamaları](/azure/architecture/checklist/resiliency-per-service).

