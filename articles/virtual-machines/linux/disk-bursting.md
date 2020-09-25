---
title: Yönetilen disk patlaması
description: Linux üzerinde Azure diskleri ve Azure sanal makineleri için disk patlaması hakkında bilgi edinin.
author: albecker1
ms.author: albecker
ms.date: 09/22/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 2e5fc8bde2c79a355fb7963c9101c4b040f97fa7
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91275174"
---
# <a name="managed-disk-bursting"></a>Yönetilen disk patlaması
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Sanal makine düzeyinde patlama
VM düzeyinde burdıya desteği, bu desteklenen boyutlarda genel buluttaki tüm bölgelerde etkin: 
- [Lsv2 serisi](../lsv2-series.md)

VM düzeyi patlaması, aşağıdaki desteklenen boyutlarda de Orta Batı ABD de kullanılabilir:
- [Dsv3 serisi](../dv3-dsv3-series.md)
- [Esv3 serisi](../ev3-esv3-series.md)

Bu işlemi destekleyen sanal makineler için burdıya varsayılan olarak etkindir.

## <a name="disk-level-bursting"></a>Disk düzeyinde patlama
Ayrıca, Azure genel, kamu ve Çin bulutlarındaki tüm bölgelerde P20 ve daha küçük disk boyutları için [Premium SSD](disks-types.md#premium-ssd) 'larımız de kullanıma sunulmuştur. Disk patlaması, bunu destekleyen disk boyutlarının tüm yeni ve mevcut dağıtımları üzerinde varsayılan olarak etkindir. 

[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]
