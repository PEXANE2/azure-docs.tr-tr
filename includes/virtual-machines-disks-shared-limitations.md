---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/30/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 76e52758bf88dde9cdc68dad974f0b65fd55140f
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96025647"
---
Paylaşılan disklerin etkinleştirilmesi yalnızca disk türlerinin bir alt kümesi tarafından kullanılabilir. Şu anda yalnızca Ultra diskler ve Premium SSD 'Ler, Paylaşılan diskleri etkinleştirebilir. Paylaşılan diskleri olan her yönetilen disk, disk türüne göre düzenlenmiş aşağıdaki sınırlamalara tabidir:

### <a name="ultra-disks"></a>Ultra diskler

Ultra disklerin, paylaşılan disklerle ilgisi olmayan ayrı bir sınırlama listesi vardır. Ultra disk sınırlamaları için bkz. [Azure Ultra diskleri kullanma](../articles/virtual-machines/disks-enable-ultra-ssd.md).

Ultra diskler paylaştırdığınızda, bunlar aşağıdaki ek sınırlamalara sahiptir:

- Şu anda Azure Resource Manager veya SDK desteğiyle sınırlı. 
- Yalnızca temel diskler bazı Windows Server yük devretme kümesi sürümleriyle kullanılabilir. Ayrıntılar için bkz. [Yük Devretme Kümelemesi Donanım gereksinimleri ve depolama seçenekleri](/windows-server/failover-clustering/clustering-requirements).

Paylaşılan Ultra diskler, varsayılan olarak Ultra diskleri destekleyen tüm bölgelerde kullanılabilir ve bunları kullanmaya erişmek için kaydolmanız gerekmez.

### <a name="premium-ssds"></a>Premium SSD’ler

- Şu anda Azure Resource Manager veya SDK desteğiyle sınırlı. 
- Yalnızca veri disklerinde etkinleştirilebilir, işletim sistemi diskleri için kullanılamaz.
- İle Premium SSD 'Ler için **ReadOnly** konak önbelleğe alma kullanılamaz `maxShares>1` .
- Disk patlaması, ile Premium SSD 'Ler için kullanılamaz `maxShares>1` .
- Azure Paylaşılan disklerle kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri kullanırken, sanal makine hata etki alanı ile [depolama hatası etki alanı hizalaması](../articles/virtual-machines/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) , paylaşılan veri diski için zorlanmaz.
- [Yakınlık yerleşimi grupları (PPG)](../articles/virtual-machines/windows/proximity-placement-groups.md)kullanılırken, bir diski paylaşan tüm sanal makineler aynı PPG 'nin parçası olmalıdır.
- Yalnızca temel diskler bazı Windows Server yük devretme kümesi sürümleriyle kullanılabilir. Ayrıntılar için bkz. [Yük Devretme Kümelemesi Donanım gereksinimleri ve depolama seçenekleri](/windows-server/failover-clustering/clustering-requirements).
- Azure Backup ve Azure Site Recovery desteği henüz kullanılamıyor.

#### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Paylaşılan Premium SSD 'ler, yönetilen disklerin kullanılabildiği tüm bölgelerde kullanılabilir.