---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/09/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 1e53dda2c6cb293a9204f344d152daa1937aa38b
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81008379"
---
Önizleme sırasında, paylaşılan diskleri etkinleştirmek yalnızca disk türlerinin bir alt kümesi tarafından kullanılabilir. Şu anda yalnızca ultra diskler ve premium SSD'ler paylaşılan diskleri etkinleştirebilir. Diskleri etkinleştirilen paylaşılan her yönetilen disk, disk türüne göre düzenlenen aşağıdaki sınırlamalara tabidir:

### <a name="ultra-disks"></a>Ultra diskler

Ultra disklerin paylaşılan disklerle ilgisi olmayan kendi sınırlama listesi vardır. Ultra disk sınırlamaları için [Azure ultra diskleri kullanma'ya](../articles/virtual-machines/linux/disks-enable-ultra-ssd.md)bakın.

Ultra diskleri paylaşırken aşağıdaki ek sınırlamalara sahiptirler:

- Şu anda sadece Batı ABD'de desteklenir.
- Şu anda Azure Kaynak Yöneticisi veya SDK desteği ile sınırlıdır.
- Windows Server Failover Cluster'ın bazı sürümlerinde yalnızca temel diskler kullanılabilir, ayrıntılar için [Failover kümeleme donanım gereksinimleri ve depolama seçeneklerine](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements)bakın.

### <a name="premium-ssds"></a>Premium SSD’ler

- Şu anda sadece Batı Orta ABD bölgesinde desteklenir.
- Bir diski paylaşan tüm sanal makineler aynı [yakınlık yerleşim gruplarında](../articles/virtual-machines/windows/proximity-placement-groups.md)dağıtılmalıdır.
- İşletim sistemi disklerinde değil, yalnızca veri disklerinde etkinleştirilebilir.
- Windows Server Failover Cluster'ın bazı sürümlerinde yalnızca temel diskler kullanılabilir, ayrıntılar için [Failover kümeleme donanım gereksinimleri ve depolama seçeneklerine](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements)bakın.
- ReadOnly ana bilgisayar önbelleğe alma ile `maxShares>1`premium SSD'ler için kullanılamaz.
- Kullanılabilirlik kümeleri ve sanal makine `FaultDomainCount` ölçek kümeleri yalnızca 1'e ayarlanmış olarak kullanılabilir.
- Azure Yedekleme ve Azure Site Kurtarma desteği henüz kullanılamıyor.

Paylaşılan diskleri denemek istiyorsanız [önizlememize kaydolun.](https://aka.ms/AzureSharedDiskPreviewSignUp)
