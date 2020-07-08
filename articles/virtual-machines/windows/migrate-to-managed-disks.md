---
title: Azure VM 'lerini yönetilen disklere geçirme
description: Yönetilen diskleri kullanmak için depolama hesaplarında yönetilmeyen diskler kullanılarak oluşturulan Azure sanal makinelerini geçirin.
author: roygara
ms.service: virtual-machines-windows
ms.topic: how-to
ms.date: 05/30/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: d64ebb053ef01e375edd52ad0bf2c1f424f1b837
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84660825"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Azure VM 'Leri Azure 'da yönetilen disklere geçirme

Azure yönetilen diskler, depolama hesaplarını ayrı olarak yönetme gereksinimini ortadan kaldırarak depolama yönetiminizi basitleştirir.  Ayrıca, bir kullanılabilirlik kümesindeki VM 'lerin daha iyi güvenlerinden yararlanmak için mevcut Azure sanal makinelerinizi yönetilen disklere geçirebilirsiniz. Tek bir başarısızlık noktasını önlemek için bir kullanılabilirlik kümesindeki farklı sanal makinelerin disklerinin birbirinden yeterince yalıtılmış olmasını sağlar. Farklı sanal makinelerin disklerini, farklı depolama ölçek birimlerindeki (damgalar), donanım ve yazılım hatalarından kaynaklanan tek bir depolama ölçek birimi hatalarının etkisini sınırlayan bir kullanılabilirlik kümesine otomatik olarak koyar.
Gereksinimlerinize göre dört tür depolama seçeneği arasından seçim yapabilirsiniz. Kullanılabilir disk türleri hakkında bilgi edinmek için bkz. makalemiz [disk türü seçme](disks-types.md)

## <a name="migration-scenarios"></a>Geçiş senaryoları

Aşağıdaki senaryolarda yönetilen disklere geçiş yapabilirsiniz:

|Senaryo  |Makale  |
|---------|---------|
|Tek başına VM'leri ve bir kullanılabilirlik kümesindeki VM'leri yönetilen disklere dönüştürme     |[VM 'Leri yönetilen diskleri kullanacak şekilde dönüştürme](convert-unmanaged-to-managed-disks.md)         |
|Yönetilen disklerde tek bir VM 'yi klasik 'ten Kaynak Yöneticisi dönüştürme     |[Klasik bir VHD 'den VM oluşturma](create-vm-specialized-portal.md)         |
|Yönetilen disklerde, bir sanal ağdaki tüm VM 'Leri klasik 'ten Kaynak Yöneticisi Dönüştür     |[IaaS kaynaklarını klasik 'ten Kaynak Yöneticisi geçirin](migration-classic-resource-manager-ps.md) ve ardından [bir VM 'yi yönetilmeyen disklerden yönetilen disklere dönüştürün](convert-unmanaged-to-managed-disks.md)         |
|Yönetilen Premium disklere sahip VM 'lere standart yönetilmeyen disklerle sanal makineler yükseltme     | İlk olarak, [bir Windows sanal makinesini yönetilmeyen disklerden yönetilen disklere dönüştürün](convert-unmanaged-to-managed-disks.md). Ardından [yönetilen bir diskin depolama türünü güncelleştirin](convert-disk-storage.md).         |

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen diskler](managed-disks-overview.md) hakkında daha fazla bilgi edinin
- [Yönetilen diskler için fiyatlandırmayı](https://azure.microsoft.com/pricing/details/managed-disks/)gözden geçirin.
