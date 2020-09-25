---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/22/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 11901e0b68fed9a6a83287d50896bf2afc51fa37
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91279578"
---
Paylaşılan disklerin etkinleştirilmesi yalnızca disk türlerinin bir alt kümesi tarafından kullanılabilir. Şu anda yalnızca Ultra diskler ve Premium SSD 'Ler, Paylaşılan diskleri etkinleştirebilir. Paylaşılan diskleri olan her yönetilen disk, disk türüne göre düzenlenmiş aşağıdaki sınırlamalara tabidir:

### <a name="ultra-disks"></a>Ultra diskler

Ultra disklerin, paylaşılan disklerle ilgisi olmayan ayrı bir sınırlama listesi vardır. Ultra disk sınırlamaları için bkz. [Azure Ultra diskleri kullanma](../articles/virtual-machines/disks-enable-ultra-ssd.md).

Ultra diskler paylaştırdığınızda, bunlar aşağıdaki ek sınırlamalara sahiptir:

- Şu anda Azure Resource Manager veya SDK desteğiyle sınırlı. 
- Yalnızca temel diskler bazı Windows Server yük devretme kümesi sürümleriyle kullanılabilir. Ayrıntılar için bkz. [Yük Devretme Kümelemesi Donanım gereksinimleri ve depolama seçenekleri](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).

Paylaşılan Ultra diskler, varsayılan olarak Ultra diskleri destekleyen tüm bölgelerde kullanılabilir ve bunları kullanmaya erişmek için kaydolmanız gerekmez.

### <a name="premium-ssds"></a>Premium SSD’ler

- Şu anda yalnızca [bölgelerin bir alt kümesinde](#regional-availability)desteklenir.
- Şu anda Azure Resource Manager veya SDK desteğiyle sınırlı. 
- Yalnızca veri disklerinde etkinleştirilebilir, işletim sistemi diskleri için kullanılamaz.
- İle Premium SSD 'Ler için **ReadOnly** konak önbelleğe alma kullanılamaz `maxShares>1` .
- Disk patlaması, ile Premium SSD 'Ler için kullanılamaz `maxShares>1` .
- Azure Paylaşılan disklerle kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri kullanırken, sanal makine hata etki alanı ile [depolama hatası etki alanı hizalaması](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability#use-managed-disks-for-vms-in-an-availability-set) , paylaşılan veri diski için zorlanmaz.
- [Yakınlık yerleşimi grupları (PPG)](../articles/virtual-machines/windows/proximity-placement-groups.md)kullanılırken, bir diski paylaşan tüm sanal makineler aynı PPG 'nin parçası olmalıdır.
- Yalnızca temel diskler bazı Windows Server yük devretme kümesi sürümleriyle kullanılabilir. Ayrıntılar için bkz. [Yük Devretme Kümelemesi Donanım gereksinimleri ve depolama seçenekleri](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- Azure Backup ve Azure Site Recovery desteği henüz kullanılamıyor.

#### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

Paylaşılan Premium SSD 'ler yalnızca aşağıdaki bölgelerde desteklenir:

- Doğu ABD
- Doğu ABD 2
- Batı ABD
- Batı ABD 2
- Orta Batı ABD
- Orta Güney ABD
- Orta Kuzey ABD
- Orta ABD
- West Europe
- Kuzey Avrupa
- Güney Kore - Orta
- Orta Kanada
- Doğu Kanada
- Doğu Japonya
- Batı Japonya
- Güneydoğu Asya
- Doğu Asya
- US Gov Virginia
- US Gov Arizona