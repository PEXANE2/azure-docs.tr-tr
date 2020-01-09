---
title: Azure 'da Red hat iş yükleri genel bakış | Microsoft Docs
description: Azure 'da kullanılabilen Red Hat ürün teklifleri hakkında bilgi edinin
services: virtual-machines-linux
author: asinn826
manager: borisb2015
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.topic: overview
ms.date: 12/18/2019
ms.author: alsin
ms.openlocfilehash: df787d4102752bdf61e30bc00d0d08307ac12319
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75473167"
---
# <a name="red-hat-workloads-on-azure"></a>Azure 'da Red hat iş yükleri
Red hat iş yükleri, Azure 'daki çeşitli tekliflerle desteklenir. Red Hat Enterprise Linux (RHEL) görüntüleri, Red Hat güncelleştirme altyapısı (RHUı) gibi RHEL iş yüklerinin çekirdeğleridir.

## <a name="red-hat-enterprise-linux-rhel-images"></a>Red Hat Enterprise Linux (RHEL) görüntüleri
Azure, Azure 'da çok sayıda RHEL görüntüsü sunmaktadır. Bu görüntüler iki farklı lisans modeli aracılığıyla kullanılabilir hale getirilir: Kullandıkça Öde (PAYG) ve kendi aboneliğinizi getir (BYOS). Azure 'daki yeni RHEL görüntüleri, yeni RHEL sürümleri yayımlandığında ve bunların yaşam döngülerine göre güncelleştirilerek yayımlandığında yayımlanır.

### <a name="pay-as-you-go-payg-images"></a>Kullandıkça Öde (PAYG) görüntüleri
Azure, çeşitli RHEL PAYG görüntüleri sunmaktadır. Bu görüntüler RHEL için uygun şekilde bulunur ve bir güncelleştirme kaynağına eklenir (Red Hat güncelleştirme altyapısı). Bu görüntüler, RHEL yetkilendirme ve güncelleştirmelerine yönelik bir Premium ücret ödemenizi sağlayacak. RHEL PAYG görüntü çeşitleri şunları içerir:
* Standart RHEL
* RHEL for SAP
* Yüksek kullanılabilirlik ve güncelleştirme hizmetleri olan SAP için RHEL.

Uygun sayıda abonelik için ayrı olarak ödeme yapma konusunda endişelenmek istemiyorsanız, PAYG görüntülerini kullanmak isteyebilirsiniz.

### <a name="bring-your-own-subscription-byos-images"></a>Kendi aboneliğinizi getir (BYOS) görüntüleri
Azure ayrıca RHEL BYOS görüntüleri de sunmaktadır. Bu görüntüler, var olan Red Hat aboneliklerine sahip müşteriler için yararlı olabilir ve bunları Azure 'da kullanmak isteyebilir. Azure 'da kullanabilmeniz için mevcut aboneliklerinizi bulut erişim aboneliklerine dönüştürmeniz gerekir. Bu görüntülere erişim yalnızca Red Hat yeterli sayıda bulut erişim aboneliğine sahip olduğunuzu doğruladığında verilir. Bu görüntülerin kullanılması, bir müşterinin, PAYG görüntülerini kullanarak tahakkuk ettirilen bir çift faturalandırmaya engel olmasını sağlar. [Burada](https://aka.ms/rhel-byos)BYOS görüntülerine erişim isteyebilirsiniz.

> [!NOTE]
> Çift faturalandırma üzerine göz önünde, bir kullanıcının RHEL abonelikleri için iki kez ödeme yaptığı iki faturalandırma yapılır. Bu genellikle bir müşterinin bir RHEL PAYG sanal makinesine yetkilendirme eklemek için abonelik Yöneticisi kullandığında oluşur. Örneğin, bir RHEL PAYG görüntüsündeki SAP paketlerine yönelik bir yetkilendirme eklemek için abonelik Yöneticisi 'ni kullanan bir müşteri, bir kez RHEL-bir kez ve SAP aboneliğinden bir kez olmak üzere iki kez faturalandırılacağından, dolaylı olarak çift faturalandırılır. Bu, KCG görüntü kullanıcıları tarafından gerçekleşmeyecektir.

## <a name="red-hat-update-infrastructure-rhui"></a>Red Hat güncelleştirme altyapısı (RHUı)
Azure, yalnızca PAYG RHEL sanal makineleri (VM 'Ler) için Red Hat güncelleştirme altyapısı sağlar. RHUı, Red Hat CDNs 'in bir yansıtmasıdır ancak yalnızca Azure PAYG RHEL VM 'lerinin erişimine açık olur. Dağıttığınız RHEL görüntüsüne bağlı olarak uygun paketlere erişebilirsiniz. Örneğin, bir RHEL for SAP görüntüsü, temel RHEL paketlerine ek olarak SAP paketlerine erişebilcektir.

### <a name="rhui-update-behavior"></a>RHUı güncelleştirme davranışı
RHUı 'e bağlı RHEL görüntüleri, varsayılan olarak, bir `yum update` çalıştırıldığında RHEL 'nin en son ikincil sürümüne güncelleştirme yapılır. Bu davranış, üzerinde `yum update` bir işlem çalıştırıldığında RHEL 7,4 VM 'sinin RHEL 7,7 ' ye yükseltibileceği anlamına gelir. Bu, RHUı tasarımdır. Ancak, normal RHEL depolarından [genişletilmiş güncelleştirme desteği (EUS) depolarına](./redhat-rhui.md#rhel-eus-and-version-locking-rhel-vms)geçiş yaparak bu yükseltme davranışı azaltılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure 'Da RHEL görüntüleri](./redhat-images.md) hakkında daha fazla bilgi edinin
* [Red Hat güncelleştirme altyapısı](./redhat-rhui.md) hakkında daha fazla bilgi
* [RHEL BYOS teklifi](./redhat-byos.md) hakkında daha fazla bilgi edinin