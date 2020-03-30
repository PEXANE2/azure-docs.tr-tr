---
title: Azure Site Kurtarma'da Azure rol tabanlı erişim denetimini yönetme
description: Bu makalede, Azure Site Kurtarma erişimini yönetmek için rol tabanlı erişim denetiminin (RBAC) nasıl uygulanacağı açıklanmaktadır.
ms.service: site-recovery
ms.date: 04/08/2019
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: ce389f9281b02662f87353f00c9bca92cdf86937
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257582"
---
# <a name="manage-site-recovery-access-with-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) ile Site Kurtarma erişimini yönetme

Azure rol tabanlı erişim denetimi (RBAC), Azure için ince taneli erişim yönetimi sağlar. RBAC'ı kullanarak, ekibinizin içindeki sorumlulukları ayırabilir ve belirli işleri gerçekleştirmek için gerektiğinde kullanıcılara yalnızca belirli erişim izinleri verebilirsiniz.

Azure Site Kurtarma, Site Kurtarma yönetim işlemlerini denetlemek için 3 yerleşik rol sağlar. [Azure RBAC yerleşik rolleri](../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi edinin

* [Site Recovery Katkıda Bulunanı](../role-based-access-control/built-in-roles.md#site-recovery-contributor) - Bu rol, Kurtarma Hizmetleri kasasında Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir. Ancak bu role sahip kullanıcı, Kurtarma Hizmetleri kasasını oluşturamaz veya silemez ya da diğer kullanıcılara erişim hakkı atayamaz. Bu rol, durum böyle olabileceği gibi, uygulamalar veya tüm kuruluşlar için olağanüstü durum kurtarmayı etkinleştirebilen ve yönetebilen olağanüstü durum kurtarma yöneticileri için en uygun roldür.
* [Site Recovery Operatörü](../role-based-access-control/built-in-roles.md#site-recovery-operator) - Bu rol, Yük Devretme ve Yeniden Çalışma işlemlerini yürütme ve yönetme izinlerine sahiptir. Bu role sahip bir kullanıcı çoğaltmayı etkinleştiremez veya devre dışı kaldı, kasa oluşturamaz veya silemez, yeni altyapı kaydedemez veya diğer kullanıcılara erişim hakları atayamaz. Bu rol, uygulama sahipleri ve BT yöneticileri tarafından DR matkabı gibi gerçek veya simüle edilmiş bir felaket durumunda talimat verildiğinde sanal makineler veya uygulamalar üzerinde başarısız olan bir olağanüstü durum kurtarma operatörü için en uygun uyrur. Felaketin çözümü nden sonra, DR operatörü sanal makineleri yeniden koruyabilir ve geri alabilir.
* [Site Recovery Okuyucusu](../role-based-access-control/built-in-roles.md#site-recovery-reader) - Bu rol tüm Site Recovery yönetim işlemlerini görüntüleme iznine sahiptir. Bu rol, mevcut koruma durumunu izleyip gerekirse destek biletlerini yükseltebilen bir BT izleme yöneticisi için en uygun roldür.

Daha fazla denetim için kendi rollerinizi tanımlamak istiyorsanız, Azure'da Özel rollerin nasıl [oluşturabildiğini](../role-based-access-control/custom-roles.md) öğrenin.

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Yeni sanal makineler için çoğaltmayı etkinleştirmek için gereken izinler
Azure Site Kurtarma kullanılarak yeni bir Sanal Makine Azure'da çoğaltıldığında, kullanıcının Site Kurtarma'ya sağlanan Azure kaynaklarını kullanmak için gerekli izinlere sahip olduğundan emin olmak için ilişkili kullanıcının erişim düzeyleri doğrulanır.

Yeni bir sanal makine için çoğaltmayı etkinleştirmek için, bir kullanıcının şunları yapması gerekir:
* Seçili kaynak grubunda sanal makine oluşturma izni
* Seçili sanal ağda sanal makine oluşturma izni
* Seçili Depolama hesabına yazma izni

Bir kullanıcı, yeni bir sanal makinenin çoğaltılmasını tamamlamak için aşağıdaki izinlere ihtiyaç duyar.

> [!IMPORTANT]
>Kaynak dağıtımı için kullanılan dağıtım modeline (Kaynak Yöneticisi/ Klasik) ilgili izinlerin eklenmiştir.

> [!NOTE]
> Bir Azure VM için çoğaltmayı etkinleştiriyorsanız ve Site Kurtarma'nın güncelleştirmeleri yönetmesine izin vermek istiyorsanız, çoğaltmayı etkinleştirirken yeni bir Otomasyon hesabı da oluşturmak isteyebilirsiniz ve bu durumda aynı anda bir otomasyon hesabı oluşturmak için izin almanız gerekebilir kasa olarak da abonelik.

| **Kaynak Türü** | **Dağıtım Modeli** | **İzin** |
| --- | --- | --- |
| İşlem | Resource Manager | Microsoft.Compute/availabilitySets/read |
|  |  | Microsoft.Compute/virtualMachines/read |
|  |  | Microsoft.Compute/virtualMachines/write |
|  |  | Microsoft.Compute/virtualMachines/delete |
|  | Klasik | Microsoft.ClassicCompute/domainNames/read |
|  |  | Microsoft.ClassicCompute/domainNames/write |
|  |  | Microsoft.ClassicCompute/domainNames/delete |
|  |  | Microsoft.ClassicCompute/virtualMachines/read |
|  |  | Microsoft.ClassicCompute/virtualMachines/write |
|  |  | Microsoft.ClassicCompute/virtualMachines/delete |
| Ağ | Resource Manager | Microsoft.Network/networkInterfaces/read |
|  |  | Microsoft.Network/networkInterfaces/write |
|  |  | Microsoft.Network/networkInterfaces/delete |
|  |  | Microsoft.Network/networkInterfaces/join/action |
|  |  | Microsoft.Network/virtualNetworks/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/read |
|  |  | Microsoft.Network/virtualNetworks/subnets/join/action |
|  | Klasik | Microsoft.ClassicNetwork/virtualNetworks/oku |
|  |  | Microsoft.ClassicNetwork/virtualNetworks/join/action |
| Depolama | Resource Manager | Microsoft.Storage/storageAccounts/read |
|  |  | Microsoft.Storage/storageAccounts/listkeys/action |
|  | Klasik | Microsoft.ClassicStorage/storageAccounts/read |
|  |  | Microsoft.ClassicStorage/storageAccounts/listKeys/action |
| Kaynak Grubu | Resource Manager | Microsoft.Resources/deployments/* |
|  |  | Microsoft.Resources/subscriptions/resourceGroups/read |

Kaynak Yöneticisi ve Klasik dağıtım modelleri için sırasıyla 'Sanal Makine Katılımcısı' ve 'Klasik Sanal Makine Katılımcısı' [yerleşik rollerini](../role-based-access-control/built-in-roles.md) kullanmayı düşünün.

## <a name="next-steps"></a>Sonraki adımlar
* [Role-Based Access Control](../role-based-access-control/role-assignments-portal.md): Azure portalında RBAC ile başlayın.
* Şularla erişimi nasıl yöneteceklerini öğrenin:
  * [Powershell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Rol Tabanlı Erişim Denetimi sorun giderme](../role-based-access-control/troubleshooting.md): Sık karşılaşılan sorunları gidermek için öneriler alın.
