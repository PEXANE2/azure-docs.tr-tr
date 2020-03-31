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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77471728"
---
Önizleme sırasında, diskleri etkinleştirilen yönetilen diskler aşağıdaki sınırlamalara tabidir:

- Şu anda sadece premium SSD'ler ile kullanılabilir.
- Şu anda sadece Batı Orta ABD bölgesinde desteklenir.
- Bir diski paylaşan tüm sanal makineler aynı [yakınlık yerleşim gruplarında](../articles/virtual-machines/windows/proximity-placement-groups.md)dağıtılmalıdır.
- İşletim sistemi disklerinde değil, yalnızca veri disklerinde etkinleştirilebilir.
- Windows Server Failover Cluster'ın bazı sürümlerinde yalnızca temel diskler kullanılabilir, ayrıntılar için [Failover kümeleme donanım gereksinimleri ve depolama seçeneklerine](https://docs.microsoft.com/windows-server/failover-clustering/clustering-requirements)bakın.
- ReadOnly ana bilgisayar önbelleğe alma ile `maxShares>1`premium SSD'ler için kullanılamaz.
- Kullanılabilirlik kümeleri ve sanal makine `FaultDomainCount` ölçek kümeleri yalnızca 1'e ayarlanmış olarak kullanılabilir.
- Azure Yedekleme ve Azure Site Kurtarma desteği henüz kullanılamıyor.

Paylaşılan diskleri denemek istiyorsanız [önizlememize kaydolun.](https://aka.ms/AzureSharedDiskPreviewSignUp)
