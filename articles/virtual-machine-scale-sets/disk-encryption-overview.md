---
title: Sanal Makine Ölçek Kümeleri için Azure Disk Şifrelemesini Etkinleştir
description: Bu makalede, Sanal Makine Ölçek Kümeleri için Microsoft Azure Disk Şifrelemesi etkinleştirme hakkında yönergeler
author: msmbaldwin
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.author: mbaldwin
ms.date: 10/10/2019
ms.custom: seodec18
ms.openlocfilehash: 36eb83d99af299260ebbbd74fe56d69921ba389b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278981"
---
# <a name="azure-disk-encryption-for-virtual-machine-scale-sets"></a>Sanal Makine Ölçek Kümeleri için Azure Disk Şifrelemesi

Azure Disk Şifreleme, sanal makinelerinizin işletim sistemi ve veri diskleri için birim şifreleme sağlayarak verilerinizin kuruluş güvenliği ve uyumluluk taahhütlerini karşılamak için korunmasına ve korunmasına yardımcı olur. Daha fazla bilgi için Azure [Disk Şifrelemesi: Linux VM'leri](../virtual-machines/linux/disk-encryption-overview.md) ve [Azure Disk Şifrelemesi: Windows VM'leri](../virtual-machines/windows/disk-encryption-overview.md)  

Azure Disk Şifreleme, bu gibi durumlarda Windows ve Linux sanal makine ölçek kümelerine de uygulanabilir:
- Yönetilen disklerle oluşturulan ölçek kümeleri. Azure Disk şifrelemesi yerel (veya yönetilmeyen) disk ölçeği kümeleri için desteklenmez.
- Windows ölçeğindeki işletim sistemi ve veri birimleri kümeler.
- Linux ölçeğindeki veri hacimleri kümeler. İşletim sistemi disk şifrelemesi şu anda Linux ölçek kümeleri için desteklenmez.

[Azure CLI'yi kullanarak sanal makine ölçeği kümelerini şifreleme](disk-encryption-cli.md) veya Azure PowerShell öğreticilerini [kullanarak sanal makine ölçeği kümelerini şifreleme](disk-encryption-powershell.md) ile sanal makine ölçeği kümeleri için Azure Disk Şifrelemesinin temellerini birkaç dakika içinde öğrenebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Kaynak Yöneticisi'ni kullanarak sanal makine ölçeği kümelerini şifreleme](disk-encryption-azure-resource-manager.md)
- [Azure Disk Şifreleme için önemli bir kasa oluşturma ve yapılandırma](disk-encryption-key-vault.md)
- [Sanal makine ölçeği kümesi uzantısı sıralaması ile Azure Disk Şifreleme'yi kullanma](disk-encryption-extension-sequencing.md)
