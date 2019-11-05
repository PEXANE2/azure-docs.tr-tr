---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 10/24/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8bb607d0b3091de34c5c75b09c3b99257f6b2c2d
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73467745"
---
| Premium SSD Boyutlar | P1 | P2 | P3 | P4 | P6 | P10 | P15 | P20 | P30 | P40 | P50 | P60 | P70 | P80 |
|-------------------|----|----|----|----|----|-----|-----|-----|-----|-----|-----|------|------|------|
| GiB 'de disk boyutu | 4 | 8 | 16 | 32 | 64 | 128 | 256 | 512 | 1\.024 | 2\.048 | 4\.096 | 8,192 | 16.384 | 32.767 |
| Disk başına IOPS | 120 | 120 | 120 | 120 | 240 | 500 | 1\.100 | 2\.300 | 5\.000 | 7\.500 | 7\.500 | 16.000 | 18.000 | 20.000 |
| Disk başına aktarım hızı | 25 MiB/sn | 25 MiB/sn | 25 MiB/sn |En fazla 25 MIB/sn | 50 MIB/sn | 100 MIB/sn | 125 MIB/sn | 150 MIB/sn | 200 MIB/sn | 250 MIB/sn | 250 MIB/sn| 500 MIB/sn | 750 MIB/sn | 900 MIB/sn |
| Disk başına en fazla patlama ıOPS * * | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 | 3\.500 |
| Disk başına en fazla patlama performansı * * | 170 MIB/sn | 170 MIB/sn | 170 MIB/sn | 170 MIB/sn | 170 MIB/sn | 170 MIB/sn | 170 MIB/sn | 170 MIB/sn |
| En fazla patlama süresi * * | 30 dakika  | 30 dakika  | 30 dakika  | 30 dakika  | 30 dakika  | 30 dakika  | 30 dakika  | 30 dakika  |

\*, şu anda önizleme aşamasında olan bir disk boyutunu gösterir, bölgesel kullanılabilirlik bilgileri için bkz. [Premium diskler: yönetilen ve yönetilmeyen](https://docs.microsoft.com/azure/virtual-machines/linux/faq-for-disks#premium-disks-managed-and-unmanaged).

\** Şu anda önizleme aşamasında olan bir özelliği gösterir. daha fazla bilgi için bkz. [disk](https://docs.microsoft.com/azure/virtual-machines/linux/disk-bursting#regional-availability) patlaması.
