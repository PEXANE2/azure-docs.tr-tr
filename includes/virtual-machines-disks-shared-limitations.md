---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/30/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 9aa8857ab10423f460874870a3663929e8e5a5c5
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91566966"
---
Paylaşılan disklerin etkinleştirilmesi yalnızca disk türlerinin bir alt kümesi tarafından kullanılabilir. Şu anda yalnızca Ultra diskler ve Premium SSD 'Ler, Paylaşılan diskleri etkinleştirebilir. Paylaşılan diskleri olan her yönetilen disk, disk türüne göre düzenlenmiş aşağıdaki sınırlamalara tabidir:

### <a name="ultra-disks"></a>Ultra diskler

Ultra disklerin, paylaşılan disklerle ilgisi olmayan ayrı bir sınırlama listesi vardır. Ultra disk sınırlamaları için bkz. [Azure Ultra diskleri kullanma](../articles/virtual-machines/disks-enable-ultra-ssd.md).

Ultra diskler paylaştırdığınızda, bunlar aşağıdaki ek sınırlamalara sahiptir:

- Şu anda Azure Resource Manager veya SDK desteğiyle sınırlı. 
- Yalnızca temel diskler bazı Windows Server yük devretme kümesi sürümleriyle kullanılabilir. Ayrıntılar için bkz. [Yük Devretme Kümelemesi Donanım gereksinimleri ve depolama seçenekleri](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).

Paylaşılan Ultra diskler, varsayılan olarak Ultra diskleri destekleyen tüm bölgelerde kullanılabilir ve bunları kullanmaya erişmek için kaydolmanız gerekmez.

### <a name="premium-ssds"></a>Premium SSD’ler

- Şu anda Azure Resource Manager veya SDK desteğiyle sınırlı. 
- Yalnızca veri disklerinde etkinleştirilebilir, işletim sistemi diskleri için kullanılamaz.
- İle Premium SSD 'Ler için **ReadOnly** konak önbelleğe alma kullanılamaz `maxShares>1` .
- Disk patlaması, ile Premium SSD 'Ler için kullanılamaz `maxShares>1` .
- Azure Paylaşılan disklerle kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri kullanırken, sanal makine hata etki alanı ile [depolama hatası etki alanı hizalaması](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set) , paylaşılan veri diski için zorlanmaz.
- [Yakınlık yerleşimi grupları (PPG)](../articles/virtual-machines/windows/proximity-placement-groups.md)kullanılırken, bir diski paylaşan tüm sanal makineler aynı PPG 'nin parçası olmalıdır.
- Yalnızca temel diskler bazı Windows Server yük devretme kümesi sürümleriyle kullanılabilir. Ayrıntılar için bkz. [Yük Devretme Kümelemesi Donanım gereksinimleri ve depolama seçenekleri](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- Azure Backup ve Azure Site Recovery desteği henüz kullanılamıyor.

#### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Paylaşılan Premium SSD 'ler, yönetilen disklerin kullanılabildiği tüm bölgelerde kullanılabilir.