---
title: Yönetilen disk patlaması
description: Azure diskleri ve Azure sanal makineleri için disk yakalaması hakkında bilgi edinin.
author: albecker1
ms.author: albecker
ms.date: 01/27/2021
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 1cedac5814d1c547a28e9b1c894f416af5a924b5
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100585081"
---
# <a name="managed-disk-bursting"></a>Yönetilen disk patlaması
[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Sanal makine düzeyinde patlama
VM düzeyi patlaması, üzerinde desteklendikleri tüm bölgelerde aşağıdaki VM serisinde etkinleştirilir:
- [Lsv2 serisi](lsv2-series.md)
- [Dsv3 serisi](dv3-dsv3-series.md)
- [Esv3 serisi](ev3-esv3-series.md)

Bu işlemi destekleyen sanal makineler için burdıya varsayılan olarak etkindir.

## <a name="disk-level-bursting"></a>Disk düzeyinde patlama
Ayrıca, Azure genel, kamu ve Çin bulutlarındaki tüm bölgelerde P20 ve daha küçük disk boyutları için [Premium SSD](disks-types.md#premium-ssd) 'larımız de kullanıma sunulmuştur. Disk patlaması, bunu destekleyen disk boyutlarının tüm yeni ve mevcut dağıtımları üzerinde varsayılan olarak etkindir. 

[!INCLUDE [managed-disks-bursting](../../includes/managed-disks-bursting-2.md)]

## <a name="next-steps"></a>Sonraki adımlar

Elde eden kaynaklarınız hakkında fikir edinme hakkında bilgi edinmek için bkz. [disk alma ölçümleri](disks-metrics.md).
