---
title: Sanal Makine Ölçek Kümeleri için Azure disk şifrelemesini etkinleştir
description: Bu makale, sanal makine ölçek kümeleri için Microsoft Azure disk şifrelemeyi etkinleştirme hakkında yönergeler sağlar
author: msmbaldwin
ms.service: virtual-machine-scale-sets
ms.topic: article
ms.author: mbaldwin
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: 69197b9c0d2a60e1833d03a3f12802221a8307e2
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72530842"
---
# <a name="azure-disk-encryption-for-virtual-machine-scale-sets"></a>Sanal Makine Ölçek Kümeleri için Azure disk şifrelemesi

Azure disk şifrelemesi, sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifrelemesi sağlar ve böylece kurumsal güvenlik ve uyumluluk taahhütlerini karşılamak üzere verilerinizi korumanıza ve korumaya yardımcı olur. Daha fazla bilgi için bkz [. Azure disk şifrelemesi: Linux VM 'leri](../virtual-machines/linux/disk-encryption-overview.md) ve [Azure disk şifrelemesi: Windows VM 'leri](../virtual-machines/windows/disk-encryption-overview.md)  

Azure disk şifrelemesi, bu örneklerde Windows ve Linux sanal makine ölçek kümelerine de uygulanabilir:
- Yönetilen disklerle oluşturulan ölçek kümeleri. Yerel (veya yönetilmeyen) disk ölçek kümeleri için Azure disk şifrelemesi desteklenmez.
- Windows ölçek kümelerinde işletim sistemi ve veri birimleri.
- Linux ölçek kümelerinde veri birimleri. Linux ölçek kümeleri için işletim sistemi disk şifrelemesi desteklenmiyor.

Sanal Makine Ölçek Kümeleri için Azure disk şifrelemesi temellerini, [Azure CLI 'yi kullanarak bir sanal makine ölçek kümesi şifrelemesini](disk-encryption-cli.md) veya [Azure CLI öğreticilerini kullanarak bir sanal makine ölçek kümesini şifrelemeyi](disk-encryption-powershell.md) yalnızca birkaç dakika içinde öğrenebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Resource Manager kullanarak bir sanal makine ölçek kümelerini şifreleme](disk-encryption-azure-resource-manager.md)
- [Azure disk şifrelemesi için Anahtar Kasası oluşturma ve yapılandırma](disk-encryption-key-vault.md)
- [Sanal makine ölçek kümesi uzantısı sıralaması ile Azure disk şifrelemesini kullanma](disk-encryption-extension-sequencing.md)