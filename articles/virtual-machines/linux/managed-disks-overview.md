---
title: Linux VM 'Leri için Azure Disk Depolama genel bakış
description: Linux VM 'lerini kullanırken depolama hesaplarını işleyen Azure yönetilen disklere genel bakış
author: roygara
ms.service: virtual-machines-linux
ms.topic: overview
ms.date: 12/02/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 2b25da174399b092fe821a46b235d7a2bd14572b
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355905"
---
# <a name="introduction-to-azure-managed-disks"></a>Azure yönetilen disklere giriş

Azure yönetilen diskler, Azure tarafından yönetilen ve Azure sanal makinelerle kullanılan blok düzeyinde depolama birimleridir. Yönetilen diskler, şirket içi sunucuda bulunan ancak sanallaştırılmış bir fiziksel disk gibidir. Yönetilen diskler ile, tüm yapmanız gerekir disk boyutunu, disk türünü ve diski temin etmek için gereklidir. Diski sağladığınızda Azure, kalanı işler.

Kullanılabilir disk türleri Ultra disklerdir, Premium katı hal sürücüleri (SSD), standart SSD 'Ler ve standart sabit disk sürücüleri (HDD). Her bir disk türü hakkında daha fazla bilgi için bkz. [IaaS VM 'leri için disk türü seçme](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [IaaS VM 'Leri için bir disk türü seçin](disks-types.md)