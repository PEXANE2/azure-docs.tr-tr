---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 06/03/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: ce964ac197fbff64bbb7cc36e8c2bf762f93663f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84337368"
---
Önizleme aşamasında, paylaşılan disklerin etkinleştirilmesi yalnızca disk türlerinin bir alt kümesi tarafından kullanılabilir. Şu anda yalnızca Ultra diskler ve Premium SSD 'Ler, Paylaşılan diskleri etkinleştirebilir. Paylaşılan diskleri olan her yönetilen disk, disk türüne göre düzenlenmiş aşağıdaki sınırlamalara tabidir:

### <a name="ultra-disks"></a>Ultra diskler

Ultra disklerin, paylaşılan disklerle ilgisi olmayan ayrı bir sınırlama listesi vardır. Ultra disk sınırlamaları için bkz. [Azure Ultra diskleri kullanma](../articles/virtual-machines/linux/disks-enable-ultra-ssd.md).

Ultra diskler paylaştırdığınızda, bunlar aşağıdaki ek sınırlamalara sahiptir:

- Şu anda Azure Resource Manager veya SDK desteğiyle sınırlı.
- Yalnızca temel diskler bazı Windows Server yük devretme kümesi sürümleriyle kullanılabilir. Ayrıntılar için bkz. [Yük Devretme Kümelemesi Donanım gereksinimleri ve depolama seçenekleri](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).

### <a name="premium-ssds"></a>Premium SSD’ler

- Şu anda yalnızca Orta Batı ABD bölgesinde desteklenir.
- Bir diski paylaşan tüm sanal makinelerin aynı [yakınlık yerleştirme gruplarında](../articles/virtual-machines/windows/proximity-placement-groups.md)dağıtılması gerekir.
- Yalnızca veri disklerinde etkinleştirilebilir, işletim sistemi diskleri için kullanılamaz.
- Yalnızca temel diskler bazı Windows Server yük devretme kümesi sürümleriyle kullanılabilir. Ayrıntılar için bkz. [Yük Devretme Kümelemesi Donanım gereksinimleri ve depolama seçenekleri](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- İle Premium SSD 'Ler için ReadOnly konak önbelleğe alma kullanılamaz `maxShares>1` .
- Kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri yalnızca `FaultDomainCount` 1 olarak ayarlanmış şekilde kullanılabilir.
- Azure Backup ve Azure Site Recovery desteği henüz kullanılamıyor.

Paylaşılan diskler gerçekleştirmeye ilgileniyorsanız, [önizlememiz için kaydolun](https://aka.ms/AzureSharedDiskPreviewSignUp).
