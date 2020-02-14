---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/13/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4c150d874d56e3de495b0682bee979d13304a01d
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77202222"
---
Önizleme aşamasında, paylaşılan diskler etkinleştirilmiş yönetilen diskler aşağıdaki sınırlamalara tabidir:

- Şu anda yalnızca Premium SSD 'Ler ile kullanılabilir.
- Şu anda yalnızca Orta Batı ABD bölgesinde desteklenir.
- Bir diski paylaşan tüm sanal makinelerin aynı [yakınlık yerleştirme gruplarında](../articles/virtual-machines/windows/proximity-placement-groups.md)dağıtılması gerekir.
- Yalnızca veri disklerinde etkinleştirilebilir, işletim sistemi diskleri için kullanılamaz.
- Yalnızca temel diskler bazı Windows Server yük devretme kümesi sürümleriyle kullanılabilir. Ayrıntılar için bkz. [Yük Devretme Kümelemesi Donanım gereksinimleri ve depolama seçenekleri](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements).
- `maxShares>1`ile Premium SSD 'Ler için ReadOnly konak önbelleğe alma kullanılamaz.
- Kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri, yalnızca 1 olarak ayarlanmış `FaultDomainCount` ile kullanılabilir.
- Azure Backup ve Azure Site Recovery desteği henüz kullanılamıyor.

Paylaşılan diskler gerçekleştirmeye ilgileniyorsanız, [önizlememiz için kaydolun](https://aka.ms/shareddisksignup).
