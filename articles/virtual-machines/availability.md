---
title: Kullanılabilirlik seçenekleri
description: Azure 'da sanal makine çalıştırmaya yönelik kullanılabilirlik özellikleri hakkında bilgi edinin
author: cynthn
ms.author: cynthn
ms.service: virtual-machines
ms.topic: conceptual
ms.date: 05/10/2019
ms.openlocfilehash: d26e1edb53f963d591b1ee1fba58b87fd454e898
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87288563"
---
# <a name="availability-options-for-virtual-machines-in-azure"></a>Azure'da sanal makineler için kullanılabilirlik seçenekleri

Bu makalede, Azure sanal makinelerinin (VM 'Ler) kullanılabilirlik özelliklerine ilişkin bir genel bakış sunulmaktadır.

## <a name="high-availability"></a>Yüksek kullanılabilirlik

İş yükleri genellikle yüksek aktarım hızı, performans ve bir VM 'nin bir güncelleştirme ya da başka bir olay nedeniyle etkilenmemesi durumunda artıklık oluşturmak için farklı sanal makinelere yayılır. 

Azure 'un yüksek kullanılabilirlik elde etmek için sağladığı birkaç seçenek vardır. İlk olarak temel yapılar hakkında konuşalım. 

### <a name="availability-zones"></a>Kullanılabilirlik alanları

[Kullanılabilirlik alanları](../availability-zones/az-overview.md) , sanal makinelerinizdeki uygulamaların ve verilerin kullanılabilirliğini korumak için sahip olduğunuz denetim düzeyini genişletir. Bir kullanılabilirlik alanı, bir Azure bölgesi içinde fiziksel olarak ayrı bir bölgedir. Desteklenen Azure bölgesi başına üç Kullanılabilirlik Alanları vardır. 

Her Kullanılabilirlik Alanı farklı bir güç kaynağı, ağ ve soğutma sistemine sahiptir. Bölgelerde çoğaltılan VM 'Leri kullanma çözümlerinizi tasarlayarak, uygulamalarınızı ve verilerinizi bir veri merkezi kaybından koruyabilirsiniz. Bir bölge tehlikeye girerse, çoğaltılan uygulamalar ve veriler başka bir bölgede anında kullanılabilir. 

![Kullanılabilirlik alanları](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Bir [Windows](./windows/create-powershell-availability-zone.md) veya [Linux](./linux/create-cli-availability-zone.md) VM 'yi bir kullanılabilirlik alanına dağıtma hakkında daha fazla bilgi edinin.


### <a name="fault-domains"></a>Hata etki alanları

Hata etki alanı, ortak bir güç kaynağı ve ağ anahtarını paylaşan, şirket içi veri merkezindeki rafa benzer bir temel alınan donanım mantık grubudur. 

### <a name="update-domains"></a>Güncelleme etki alanları

Güncelleme etki alanı, bakımdan geçirilebilen ya da aynı anda yeniden başlatılabilen bir temel alınan donanım mantıksal grubudur. 

Bu yaklaşım, Azure platformu periyodik bakımdan geçirilirken uygulamanızın en az bir örneğinin her zaman çalışır durumda kalmasını sağlar. Yeniden başlatılan güncelleştirme etki alanlarının sırası bakım sırasında sırayla ilerlemeyebilir, ancak aynı anda yalnızca bir güncelleştirme etki alanı yeniden başlatılır.


## <a name="virtual-machines-scale-sets"></a>Sanal Makine Ölçek Kümeleri 

Azure sanal makine ölçek kümeleri, yük dengeli bir VM grubu oluşturmanızı ve yönetmenizi sağlar. Tanımlı bir zamanlamaya veya talebe yanıt olarak sanal makine örneği sayısı otomatik olarak artabilir ya da azalabilir. Ölçek Kümeleri, uygulamalarınız için yüksek kullanılabilirlik sağlar ve birçok VM 'yi merkezi olarak yönetmenize, yapılandırmanıza ve güncelleştirmenize olanak tanır. Yüksek oranda kullanılabilir bir uygulama sağlamak ve [% 99,95 Azure SLA 'sını](https://azure.microsoft.com/support/legal/sla/virtual-machines/)karşılamak için ölçek kümesi içinde iki veya daha fazla sanal makine oluşturulmasını öneririz. Ölçek kümesinin kendisi için herhangi bir maliyet yoktur, yalnızca oluşturduğunuz her sanal makine örneği için ödeme yaparsınız. Tek bir VM [Azure Premium SSD](https://docs.microsoft.com/azure/virtual-machines/windows/disks-types#premium-ssd)kullanırken, planlanmamış bakım olayları için Azure SLA 'sı geçerlidir. Ölçek kümesindeki sanal makineler, çok sayıda güncelleştirme etki alanına ve hata etki alanına dağıtılabilir ve bu da veri merkezi kesintileri ve planlı ya da plansız bakım olayları nedeniyle kullanılabilirlik ve esnekliği 'in kesintilerine en üst düzeye çıkarabilir. Ölçek kümesindeki sanal makineler aynı zamanda tek bir kullanılabilirlik bölgesine veya bölgeye göre dağıtılabilir. Kullanılabilirlik alanı dağıtım seçenekleri, düzenleme moduna göre farklılık gösterebilir.

**Hata etki alanları ve güncelleştirme etki alanları**

Sanal Makine Ölçek Kümeleri, hata etki alanlarını ve güncelleştirme etki alanlarını hizalayarak yüksek kullanılabilirlik için tasarlamayı basitleştirir. Ölçek kümesi için yalnızca hata etki alanı sayısı tanımlamanız gerekir. Ölçek kümelerinin kullanabildiği hata etki alanlarının sayısı bölgeye göre farklılık gösterebilir. Bkz. [Azure 'da sanal makinelerin kullanılabilirliğini yönetme](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability).


## <a name="availability-sets"></a>Kullanılabilirlik kümeleri
Kullanılabilirlik kümesi, Azure 'un, uygulamanızın artıklık ve kullanılabilirlik sağlamak üzere nasıl oluşturulduğunu anlamasına olanak tanıyan bir veri merkezi içindeki VM 'lerin mantıksal bir gruplandırmasıdır. Yüksek oranda kullanılabilir bir uygulama sağlamak ve [% 99,95 Azure SLA 'sını](https://azure.microsoft.com/support/legal/sla/virtual-machines/)karşılamak için bir kullanılabilirlik kümesi içinde iki veya daha fazla sanal makine oluşturulmasını öneririz. Kullanılabilirlik kümesinin kendisi için herhangi bir maliyet yoktur, yalnızca oluşturduğunuz her sanal makine örneği için ödeme yaparsınız. Tek bir VM [Azure Premium SSD](./windows/disks-types.md#premium-ssd)kullanırken, planlanmamış bakım olayları için Azure SLA 'sı geçerlidir.

Bir kullanılabilirlik kümesinde VM 'Ler bu hata etki alanlarına otomatik olarak dağıtılır. Bu yaklaşım, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkisini sınırlar.

[Azure Yönetilen Diskler](./windows/faq-for-disks.md)’i kullanan sanal makineler, yönetilen kullanılabilirlik kümesi kullanılırken yönetilen disk hata etki alanları ile hizalanır. Bu hizalama, bir VM'ye bağlı tüm yönetilen disklerin, aynı yönetilen disk hata etki alanı içinde olmasını sağlar. 

Yönetilen bir kullanılabilirlik kümesinde yalnızca, yönetilen disklere sahip VM’ler oluşturulabilir. Yönetilen disk hata etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki ya da üç yönetilen disk hata etki alanı). [Linux VM 'leri](./linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) veya [Windows VM 'leri](./windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set)için bu yönetilen disk hata etki alanları hakkında daha fazla bilgi edinebilirsiniz.

![Yönetilen kullanılabilirlik kümesi](./media/virtual-machines-common-manage-availability/md-fd-updated.png)


Bir kullanılabilirlik kümesindeki VM 'Ler Ayrıca güncelleştirme etki alanları arasında otomatik olarak dağıtılır. 

![Kullanılabilirlik kümeleri](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure ortamınızı oluşturmak için bu kullanılabilirlik ve yedeklilik özelliklerini kullanmaya başlayabilirsiniz. En iyi uygulama bilgileri için bkz. [Azure kullanılabilirlik en iyi uygulamaları](/azure/architecture/checklist/resiliency-per-service).

