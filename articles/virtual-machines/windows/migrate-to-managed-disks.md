---
title: Azure VM'leri Yönetilen Disklere Geçirin
description: Yönetilen Diskleri kullanmak için depolama hesaplarında yönetilmeyen diskler kullanılarak oluşturulan Azure sanal makinelerini geçirin.
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: e8f2753ac9062803a2d6252eca1829cb0b168f02
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77921360"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Azure VM'lerini Azure'da Yönetilen Disklere Geçirin

Azure Yönetilen Diskler, depolama hesaplarını ayrı ayrı yönetme gereksinimini ortadan kaldırarak depolama yönetiminizi kolaylaştırır.  Kullanılabilirlik Kümesi'ndeki VM'lerin daha iyi güvenilirliğinden yararlanmak için mevcut Azure VM'lerinizi Yönetilen Disklere de geçirebilirsiniz. Kullanılabilirlik Kümesi'ndeki farklı VM'lerin disklerinin, tek bir hata noktasını önlemek için birbirinden yeterince yalıtılmış olmasını sağlar. Donanım ve yazılım hatalarından kaynaklanan tek Depolama ölçeği birim hatalarının etkisini sınırlayan farklı Depolama ölçeği birimlerinde (damgalar) farklı VM'lerin disklerini otomatik olarak yerleştirir.
İhtiyaçlarınıza bağlı olarak, dört tür depolama seçeneği arasından seçim yapabilirsiniz. Kullanılabilir disk türleri hakkında bilgi edinmek için makalemize bakın [Bir disk türü seçin](disks-types.md)

## <a name="migration-scenarios"></a> Geçiş senaryoları

Aşağıdaki senaryolarda Yönetilen Disklere geçiş yapabilirsiniz:

|Senaryo  |Makale  |
|---------|---------|
|Tek başına VM'leri ve bir kullanılabilirlik kümesindeki VM'leri yönetilen disklere dönüştürme     |[Yönetilen diskleri kullanmak için VM'leri dönüştürme](convert-unmanaged-to-managed-disks.md)         |
|Yönetilen disklerde tek bir VM'yi klasikten Kaynak Yöneticisi'ne dönüştürme     |[Klasik bir VHD'den VM oluşturma](create-vm-specialized-portal.md)         |
|Yönetilen disklerde vNet'teki tüm VM'leri klasikten Kaynak Yöneticisi'ne dönüştürün     |[IaaS kaynaklarını klasikkaynak yöneticisine geçirin](migration-classic-resource-manager-ps.md) ve ardından [yönetilmeyen disklerden yönetilen disklere VM dönüştürme](convert-unmanaged-to-managed-disks.md)         |
|Yönetilen premium disklerle standart yönetilmeyen disklerle VM'leri VM'lere yükseltin     | İlk olarak, [yönetilen disklerden yönetilen disklere Windows sanal makinesini dönüştürün.](convert-unmanaged-to-managed-disks.md) Ardından [yönetilen bir diskin depolama türünü güncelleştirin.](convert-disk-storage.md)         |

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Yönetilen Diskler](managed-disks-overview.md) hakkında daha fazla bilgi edinin
- Yönetilen [Diskler için fiyatlandırmayı gözden geçirin.](https://azure.microsoft.com/pricing/details/managed-disks/)
