---
title: Yönetilen disk patlaması
description: Azure diskleri için disk alma ve Azure sanal makineleri için disk alma hakkında bilgi edinin
author: albecker1
ms.author: albecker
ms.date: 04/27/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 25aa9fc166e831acd2ed0389bbbe4d2dc7e04b19
ms.sourcegitcommit: 3abadafcff7f28a83a3462b7630ee3d1e3189a0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82594461"
---
# <a name="disk-bursting"></a>Disk genişletme
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Sanal makine düzeyinde patlama
VM düzeyinde burdıya desteği, bu desteklenen boyutlarda genel buluttaki tüm bölgelerde etkin: 
- [Lsv2 serisi](../lsv2-series.md)

Bu işlemi destekleyen sanal makineler için burdıya varsayılan olarak etkindir.

## <a name="disk-level-bursting"></a>Disk düzeyinde patlama
Ayrıca, tüm bölgelerde P20 ve daha küçük bir disk boyutu için [Premium SSD](disks-types.md#premium-ssd) 'larımız de mevcuttur. Disk patlaması, bunu destekleyen disk boyutlarının yeni dağıtımları üzerinde varsayılan olarak etkindir. Disk kullanımını destekliyorsa, mevcut disk boyutları aşağıdaki yöntemlerden birini kullanarak ani bir şekilde etkinleştirebilir: 
- **VM 'yi yeniden başlatma** 
- **Diski kullanımdan çıkarın ve yeniden bağlayın**


[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]
