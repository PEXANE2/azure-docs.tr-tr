---
title: Sanal Makine Ölçek Kümeleri için Azure disk şifrelemesini etkinleştir
description: Bu makale, sanal makine ölçek kümeleri için Microsoft Azure disk şifrelemeyi etkinleştirme hakkında yönergeler sağlar
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 10/10/2019
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 6b9805d66149a18216a200bc89a79b3e06106c9d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83195121"
---
# <a name="azure-disk-encryption-for-virtual-machine-scale-sets"></a>Sanal Makine Ölçek Kümeleri için Azure disk şifrelemesi

Azure disk şifrelemesi, sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifrelemesi sağlar ve böylece kurumsal güvenlik ve uyumluluk taahhütlerini karşılamak üzere verilerinizi korumanıza ve korumaya yardımcı olur. Daha fazla bilgi için bkz [. Azure disk şifrelemesi: Linux VM 'leri](../virtual-machines/linux/disk-encryption-overview.md) ve [Azure disk şifrelemesi: Windows VM 'leri](../virtual-machines/windows/disk-encryption-overview.md)  

Azure disk şifrelemesi, bu örneklerde Windows ve Linux sanal makine ölçek kümelerine de uygulanabilir:
- Yönetilen disklerle oluşturulan ölçek kümeleri. Yerel (veya yönetilmeyen) disk ölçek kümeleri için Azure disk şifrelemesi desteklenmez.
- Windows ölçek kümelerinde işletim sistemi ve veri birimleri.
- Linux ölçek kümelerinde veri birimleri. Linux ölçek kümeleri için işletim sistemi disk şifrelemesi desteklenmiyor.

Sanal Makine Ölçek Kümeleri için Azure disk şifrelemesi temellerini, [Azure CLI 'yi kullanarak bir sanal makine ölçek kümesi şifrelemesini](disk-encryption-cli.md) veya [Azure PowerShell öğreticileri kullanarak bir sanal makine ölçek kümelerini şifrelemeyi](disk-encryption-powershell.md) yalnızca birkaç dakika içinde öğrenebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Resource Manager kullanarak bir sanal makine ölçek kümelerini şifreleme](disk-encryption-azure-resource-manager.md)
- [Azure disk şifrelemesi için Anahtar Kasası oluşturma ve yapılandırma](disk-encryption-key-vault.md)
- [Sanal makine ölçek kümesi uzantısı sıralaması ile Azure disk şifrelemesini kullanma](disk-encryption-extension-sequencing.md)
