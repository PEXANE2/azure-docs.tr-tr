---
title: Yönetilen disk patlaması
description: Azure diskleri ve Azure sanal makineleri için disk yakalaması hakkında bilgi edinin.
author: albecker1
ms.author: albecker
ms.date: 09/22/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: dcdbf94e547581cb9ff885ac5896467abdf316ae
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99576202"
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
