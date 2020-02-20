---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 02/18/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 77fc6070010791bf96c944114929eba95842c9d4
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77471728"
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

Paylaşılan diskler gerçekleştirmeye ilgileniyorsanız, [önizlememiz için kaydolun](https://aka.ms/AzureSharedDiskPreviewSignUp).
