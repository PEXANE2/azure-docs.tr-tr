---
title: Azure Site Recovery 'de Azure rol tabanlı erişim denetimini yönetme
description: Bu makalede, Azure Site Recovery erişimini yönetmek için rol tabanlı erişim denetimi 'nin (RBAC) nasıl uygulanacağı açıklanır.
ms.service: site-recovery
ms.date: 04/08/2019
author: mayurigupta13
ms.topic: conceptual
ms.author: mayg
ms.openlocfilehash: 877545eb22331e724520102814ec8a00b2c205e5
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87370827"
---
# <a name="manage-site-recovery-access-with-role-based-access-control-rbac"></a>Rol tabanlı erişim denetimi (RBAC) ile Site Recovery erişimini yönetme

Azure rol tabanlı erişim denetimi (RBAC), Azure için ayrıntılı erişim yönetimine izin vermez. RBAC kullanarak, takımlarınızın içindeki sorumlulukları ayırabilirsiniz ve belirli işleri gerçekleştirmek için gerektiğinde kullanıcılara yalnızca belirli erişim izinleri verebilirsiniz.

Azure Site Recovery, Site Recovery yönetim işlemlerini denetlemek için 3 yerleşik rol sağlar. [Azure yerleşik rolleri](../role-based-access-control/built-in-roles.md) hakkında daha fazla bilgi edinin

* [Site Recovery Katkıda Bulunanı](../role-based-access-control/built-in-roles.md#site-recovery-contributor) - Bu rol, Kurtarma Hizmetleri kasasında Site Recovery işlemlerini yönetmek için gereken tüm izinlere sahiptir. Ancak bu role sahip kullanıcı, Kurtarma Hizmetleri kasasını oluşturamaz veya silemez ya da diğer kullanıcılara erişim hakkı atayamaz. Bu rol, büyük/küçük harf gibi durumlarda uygulamalar veya tüm kuruluşlar için olağanüstü durum kurtarmayı etkinleştirebilen ve yönetebilen olağanüstü durum kurtarma yöneticileri için idealdir.
* [Site Recovery Operatörü](../role-based-access-control/built-in-roles.md#site-recovery-operator) - Bu rol, Yük Devretme ve Yeniden Çalışma işlemlerini yürütme ve yönetme izinlerine sahiptir. Bu role sahip bir Kullanıcı çoğaltmayı etkinleştiremez veya devre dışı bırakamayabilir, kasa oluşturabilir veya silebilir, yeni altyapıyı kaydedebilir veya diğer kullanıcılara erişim hakları atayabilir. Bu rol, uygulama sahipleri ve BT yöneticileri tarafından bir DR detaya gitme gibi gerçek ya da sanal bir olağanüstü durum durumunda, sanal makinelerin veya uygulamaların yükünü önleyebilen bir olağanüstü durum kurtarma operatörü için en uygun seçenektir. Olağanüstü durum çözümlemesi sonrasında DR işletmeni, sanal makineleri yeniden koruyabilir ve yeniden gerçekleştirebilir.
* [Site Recovery Okuyucusu](../role-based-access-control/built-in-roles.md#site-recovery-reader) - Bu rol tüm Site Recovery yönetim işlemlerini görüntüleme iznine sahiptir. Bu rol, güncel koruma durumunu izleyebilen ve gerekirse destek biletleri veren BT izleme Yöneticisi için idealdir.

Daha da fazla denetim için kendi rollerinizi tanımlamak istiyorsanız bkz. Azure 'da [özel roller oluşturma](../role-based-access-control/custom-roles.md) .

## <a name="permissions-required-to-enable-replication-for-new-virtual-machines"></a>Yeni sanal makineler için çoğaltmayı etkinleştirmek için gereken izinler
Yeni bir sanal makine Azure Site Recovery kullanılarak Azure 'a çoğaltıldığında, kullanıcının Site Recovery için belirtilen Azure kaynaklarını kullanmak için gerekli izinlere sahip olduğundan emin olmak için ilişkili kullanıcının erişim düzeyleri onaylanır.

Yeni bir sanal makine için çoğaltmayı etkinleştirmek üzere bir Kullanıcı şunları içermelidir:
* Seçili kaynak grubunda sanal makine oluşturma izni
* Seçilen sanal ağda sanal makine oluşturma izni
* Seçilen depolama hesabına yazma izni

Bir kullanıcının yeni bir sanal makinenin çoğaltmasını tamamlaması için aşağıdaki izinlere ihtiyacı vardır.

> [!IMPORTANT]
>Kaynak dağıtımı için kullanılan dağıtım modeli (Kaynak Yöneticisi/klasik) başına ilgili izinlerin eklendiğinden emin olun.

> [!NOTE]
> Bir Azure VM için çoğaltmayı etkinleştirip Site Recovery güncelleştirmeleri yönetmesine izin vermek istiyorsanız, çoğaltmayı etkinleştirirken yeni bir Otomasyon hesabı oluşturmak isteyebilirsiniz ve bu durumda, kasala aynı abonelikte bir Otomasyon hesabı oluşturmak için izninizin olması gerekir.

| **Kaynak Türü** | **Dağıtım modeli** | **İzin** |
| --- | --- | --- |
| İşlem | Resource Manager | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/Read |
|  |  | Microsoft. COMPUTE/virtualMachines/okuma |
|  |  | Microsoft. COMPUTE/virtualMachines/Write |
|  |  | Microsoft. COMPUTE/virtualMachines/Delete |
|  | Klasik | Microsoft. ClassicCompute/domainNames/Read |
|  |  | Microsoft. ClassicCompute/domainNames/Write |
|  |  | Microsoft. ClassicCompute/domainNames/Delete |
|  |  | Microsoft. ClassicCompute/virtualMachines/okuma |
|  |  | Microsoft. ClassicCompute/virtualMachines/Write |
|  |  | Microsoft. ClassicCompute/virtualMachines/Delete |
| Ağ | Resource Manager | Microsoft. Network/NetworkInterfaces/Read |
|  |  | Microsoft. Network/NetworkInterfaces/Write |
|  |  | Microsoft. Network/NetworkInterfaces/Delete |
|  |  | Microsoft. Network/NetworkInterfaces/JOIN/Action |
|  |  | Microsoft. Network/virtualNetworks/Read |
|  |  | Microsoft. Network/virtualNetworks/alt ağlar/okuma |
|  |  | Microsoft. Network/virtualNetworks/alt ağlar/JOIN/Action |
|  | Klasik | Microsoft. ClassicNetwork/virtualNetworks/Read |
|  |  | Microsoft. ClassicNetwork/virtualNetworks/JOIN/Action |
| Depolama | Resource Manager | Microsoft. Storage/storageAccounts/Read |
|  |  | Microsoft. Storage/storageAccounts/ListKeys/Action |
|  | Klasik | Microsoft. ClassicStorage/storageAccounts/Read |
|  |  | Microsoft. ClassicStorage/storageAccounts/listKeys/Action |
| Kaynak Grubu | Resource Manager | Microsoft. resources/dağıtımlar/* |
|  |  | Microsoft. resources/abonelikler/resourceGroups/Read |

Sırasıyla Kaynak Yöneticisi ve klasik dağıtım modelleriyle ilgili ' sanal makine katılımcısı ' ve ' klasik sanal makine katılımcısı ' [yerleşik rollerini](../role-based-access-control/built-in-roles.md) kullanmayı göz önünde bulundurun.

## <a name="next-steps"></a>Sonraki adımlar
* [Rol tabanlı Access Control](../role-based-access-control/role-assignments-portal.md): Azure Portal RBAC ile çalışmaya başlayın.
* İle erişimin nasıl yönetileceğini öğrenin:
  * [PowerShell](../role-based-access-control/role-assignments-powershell.md)
  * [Azure CLI](../role-based-access-control/role-assignments-cli.md)
  * [REST API](../role-based-access-control/role-assignments-rest.md)
* [Rol tabanlı Access Control sorun giderme](../role-based-access-control/troubleshooting.md): yaygın sorunları gidermeye yönelik öneriler alın.
