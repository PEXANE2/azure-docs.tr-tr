---
title: Yönetilen disk patlaması
description: Azure diskleri için disk alma ve Azure sanal makineleri için disk alma hakkında bilgi edinin
author: albecker1
ms.author: albecker
ms.date: 04/27/2020
ms.topic: conceptual
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: baabad550f5e6b0ae39936fc182e6d9789d189d8
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83650930"
---
# <a name="disk-bursting"></a>Disk genişletme
[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting.md)]

## <a name="virtual-machine-level-bursting"></a>Sanal makine düzeyinde patlama
VM düzeyinde burdıya desteği, bu desteklenen boyutlarda genel buluttaki tüm bölgelerde etkin: 
- [Lsv2 serisi](../lsv2-series.md)

Bu işlemi destekleyen sanal makineler için burdıya varsayılan olarak etkindir.

## <a name="disk-level-bursting"></a>Disk düzeyinde patlama
Ayrıca, Azure genel, kamu ve Çin bulutlarındaki tüm bölgelerde P20 ve daha küçük disk boyutları için [Premium SSD](disks-types.md#premium-ssd) 'larımız de kullanıma sunulmuştur. Disk patlaması, bunu destekleyen disk boyutlarının yeni dağıtımları üzerinde varsayılan olarak etkindir. Disk kullanımını destekliyorsa, mevcut disk boyutları aşağıdaki yöntemlerden birini kullanarak ani bir şekilde etkinleştirebilir: 
- **VM 'yi yeniden başlatma** 
- **Diski kullanımdan çıkarın ve yeniden bağlayın**


[!INCLUDE [managed-disks-bursting](../../../includes/managed-disks-bursting-2.md)]
