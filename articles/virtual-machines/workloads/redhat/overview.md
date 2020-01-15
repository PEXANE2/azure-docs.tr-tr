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
ms.openlocfilehash: 8ca249a5f6c300a39548e4e16927d7a20acae1a8
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75942334"
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

### <a name="red-hat-gold-images-rhel-byos"></a>Red Hat Gold görüntüleri (`rhel-byos`)
Azure ayrıca Red Hat altın görüntülerini da sunar. Bu görüntüler, var olan Red Hat aboneliklerine sahip müşteriler için yararlı olabilir ve bunları Azure 'da kullanmak isteyebilir. Mevcut Red Hat aboneliklerinizi Azure 'da kullanabilmeniz için Red Hat bulut erişimi için etkinleştirmeniz gerekir. Red Hat abonelikleriniz bulut erişimi için etkinleştirildiğinde ve uygunluk gereksinimlerini karşılarken bu görüntülere erişim otomatik olarak verilir. Bu görüntülerin kullanılması, bir müşterinin, PAYG görüntülerini kullanarak tahakkuk ettirilen bir çift faturalandırmaya engel olmasını sağlar.
* [Azure ile bulut erişimi için Red Hat aboneliklerinizi nasıl etkinleştirebileceğinizi öğrenin](https://access.redhat.com/documentation/en-us/red_hat_subscription_management/1/html/red_hat_cloud_access_reference_guide/con-enable-subs)
* [Azure portal, CLı veya PowerShell cmdlet 'Inde Red Hat altın görüntülerini bulmayı öğrenin](./byos.md)

> [!NOTE]
> Çift faturalandırma üzerine göz önünde, bir kullanıcının RHEL abonelikleri için iki kez ödeme yaptığı iki faturalandırma yapılır. Bu genellikle bir müşterinin bir RHEL PAYG sanal makinesine yetkilendirme eklemek için abonelik Yöneticisi kullandığında oluşur. Örneğin, bir RHEL PAYG görüntüsündeki SAP paketlerine yönelik bir yetkilendirme eklemek için abonelik Yöneticisi 'ni kullanan bir müşteri, bir kez RHEL-bir kez ve SAP aboneliğinden bir kez olmak üzere iki kez faturalandırılacağından, dolaylı olarak çift faturalandırılır. Bu, KCG görüntü kullanıcıları tarafından gerçekleşmeyecektir.

## <a name="red-hat-update-infrastructure-rhui"></a>Red Hat güncelleştirme altyapısı (RHUı)
Azure, yalnızca PAYG RHEL sanal makineleri (VM 'Ler) için Red Hat güncelleştirme altyapısı sağlar. RHUı, Red Hat CDNs 'in bir yansıtmasıdır ancak yalnızca Azure PAYG RHEL VM 'lerinin erişimine açık olur. Dağıttığınız RHEL görüntüsüne bağlı olarak uygun paketlere erişebilirsiniz. Örneğin, bir RHEL for SAP görüntüsü, temel RHEL paketlerine ek olarak SAP paketlerine erişebilcektir.

### <a name="rhui-update-behavior"></a>RHUı güncelleştirme davranışı
RHUı 'e bağlı RHEL görüntüleri, varsayılan olarak, bir `yum update` çalıştırıldığında RHEL 'nin en son ikincil sürümüne güncelleştirme yapılır. Bu davranış, üzerinde `yum update` bir işlem çalıştırıldığında RHEL 7,4 VM 'sinin RHEL 7,7 ' ye yükseltibileceği anlamına gelir. Bu, RHUı tasarımdır. Ancak, normal RHEL depolarından [genişletilmiş güncelleştirme desteği (EUS) depolarına](./redhat-rhui.md#rhel-eus-and-version-locking-rhel-vms)geçiş yaparak bu yükseltme davranışı azaltılabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Azure 'Da RHEL görüntüleri](./redhat-images.md) hakkında daha fazla bilgi edinin
* [Red Hat güncelleştirme altyapısı](./redhat-rhui.md) hakkında daha fazla bilgi
* [Red Hat altın görüntü (`rhel-byos`) teklifi](./byos.md) hakkında daha fazla bilgi edinin
