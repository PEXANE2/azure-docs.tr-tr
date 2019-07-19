---
title: include dosyası
description: include dosyası
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 05/10/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 1582f61befb9b6d71adbfda9482175d67874ffb9
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67850281"
---
# <a name="availability-options-for-virtual-machines-in-azure"></a>Azure 'da sanal makineler için kullanılabilirlik seçenekleri
Bu makalede, Azure sanal makinelerinin (VM 'Ler) kullanılabilirlik özelliklerine ilişkin bir genel bakış sunulmaktadır.


## <a name="availability-sets"></a>Kullanılabilirlik kümeleri
Kullanılabilirlik kümesi, Azure 'un, uygulamanızın artıklık ve kullanılabilirlik sağlamak üzere nasıl oluşturulduğunu anlamasına olanak tanıyan bir veri merkezi içindeki VM 'lerin mantıksal bir gruplandırmasıdır. Yüksek oranda kullanılabilir bir uygulama sağlamak ve [% 99,95 Azure SLA 'sını](https://azure.microsoft.com/support/legal/sla/virtual-machines/)karşılamak için bir kullanılabilirlik kümesi içinde iki veya daha fazla sanal makine oluşturulmasını öneririz. Kullanılabilirlik kümesinin kendisi için herhangi bir maliyet yoktur, yalnızca oluşturduğunuz her sanal makine örneği için ödeme yaparsınız. Tek bir VM [Azure Premium SSD](../articles/virtual-machines/windows/disks-types.md#premium-ssd)kullanırken, planlanmamış bakım olayları için Azure SLA 'sı geçerlidir.

Kullanılabilirlik kümesi, donanım hatalarına karşı koruyan ve güncelleştirmelerin güvenli bir şekilde (hata etki alanları (FDs) ve güncelleştirme etki alanlarının (UDs) güvenli bir şekilde uygulanmasına izin veren iki ek grupdan oluşur. [Linux VM](../articles/virtual-machines/linux/manage-availability.md) veya [Windows VM](../articles/virtual-machines/windows/manage-availability.md) kullanılabilirliğini yönetme hakkında daha fazla bilgi alabilirsiniz.

### <a name="fault-domains"></a>Hata etki alanları
Hata etki alanı, ortak bir güç kaynağı ve ağ anahtarını paylaşan, şirket içi veri merkezindeki rafa benzer bir temel alınan donanım mantık grubudur. Bir kullanılabilirlik kümesinde VM’ler oluşturduğunuzda, Azure platformu VM’lerinizi bu hata etki alanlarına otomatik olarak dağıtır. Bu yaklaşım, olası fiziksel donanım hatalarının, ağ kesintilerinin veya güç kesintilerinin etkisini sınırlar.

### <a name="update-domains"></a>Güncelleme etki alanları
Güncelleme etki alanı, bakımdan geçirilebilen ya da aynı anda yeniden başlatılabilen bir temel alınan donanım mantıksal grubudur. Bir kullanılabilirlik kümesinde VM’ler oluşturduğunuzda, Azure platformu VM’lerinizi bu güncelleme etki alanlarına otomatik olarak dağıtır. Bu yaklaşım, Azure platformu periyodik bakımdan geçirilirken uygulamanızın en az bir örneğinin her zaman çalışır durumda kalmasını sağlar. Yeniden başlatılmakta olan güncelleme etki alanlarının sırası, planlanan bakım sırasında sıralı olarak uygulanmayabilir, ancak aynı anda yalnızca bir güncelleme etki alanı yeniden başlatılır.

![Kullanılabilirlik kümeleri](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains"></a>Yönetilen disk hata etki alanları
[Azure Yönetilen Diskler](../articles/virtual-machines/windows/faq-for-disks.md)’i kullanan sanal makineler, yönetilen kullanılabilirlik kümesi kullanılırken yönetilen disk hata etki alanları ile hizalanır. Bu hizalama, bir VM'ye bağlı tüm yönetilen disklerin, aynı yönetilen disk hata etki alanı içinde olmasını sağlar. Yönetilen bir kullanılabilirlik kümesinde yalnızca, yönetilen disklere sahip VM’ler oluşturulabilir. Yönetilen disk hata etki alanlarının sayısı bölgeye göre farklılık gösterir (bölge başına iki ya da üç yönetilen disk hata etki alanı). [Linux VM 'leri](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) veya [Windows VM 'leri](../articles/virtual-machines/windows/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set)için bu yönetilen disk hata etki alanları hakkında daha fazla bilgi edinebilirsiniz.

![Yönetilen kullanılabilirlik kümesi](./media/virtual-machines-common-manage-availability/md-fd-updated.png)

## <a name="availability-zones"></a>Kullanılabilirlik alanları

Kullanılabilirlik [alanları](../articles/availability-zones/az-overview.md), kullanılabilirlik kümelerine alternatif olarak, VM 'larınızdaki uygulamaların ve verilerin kullanılabilirliğini korumak için sahip olduğunuz denetim düzeyini genişletir. Kullanılabilirlik Alanı, bir Azure bölgesinin içinde fiziksel olarak ayrılmış bir alandır. Desteklenen Azure bölgesi başına üç Kullanılabilirlik Alanları vardır. Her Kullanılabilirlik Alanı farklı bir güç kaynağı, ağ ve soğutma sistemine sahiptir. Bölgelerde çoğaltılan VM 'Leri kullanma çözümlerinizi tasarlayarak, uygulamalarınızı ve verilerinizi bir veri merkezi kaybından koruyabilirsiniz. Bir bölge tehlikeye girerse, çoğaltılan uygulamalar ve veriler başka bir bölgede anında kullanılabilir. 

![Kullanılabilirlik alanları](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

Bir [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) veya [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) VM 'yi bir kullanılabilirlik alanına dağıtma hakkında daha fazla bilgi edinin.


## <a name="next-steps"></a>Sonraki adımlar
Azure ortamınızı oluşturmak için bu kullanılabilirlik ve yedeklilik özelliklerini kullanmaya başlayabilirsiniz. En iyi uygulama bilgileri için bkz. [Azure kullanılabilirlik en iyi uygulamaları](../articles/best-practices-availability-checklist.md).

