---
title: Windows VM 'Leri için yönetilen diske genel bakış Azure Disk Depolama | Microsoft Docs
description: Azure Windows VM 'Leri kullanılırken depolama hesaplarını işleyen Azure yönetilen disklere genel bakış
author: roygara
ms.service: virtual-machines-windows
ms.topic: overview
ms.date: 04/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: f86937feb7061950084a8b5bdb532075c7f93869
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68693717"
---
# <a name="introduction-to-azure-managed-disks"></a>Azure yönetilen disklere giriş

Azure yönetilen disk, bir sanal sabit disktir (VHD). Bunu, şirket içi sunucuda bir fiziksel disk gibi düşünebilirsiniz, ancak sanallaştırılabilir. Azure yönetilen diskler, Azure 'daki bir rastgele GÇ depolama nesnesi olan sayfa Blobları olarak depolanır. Sayfa Blobları, blob kapsayıcıları ve Azure depolama hesapları üzerinde bir soyutlama olduğundan, yönetilen bir disk ' Managed ' çağrısı yaptık. Yönetilen disklerle, tüm yapmanız gereken diski temin etmek ve Azure 'un geri kalanını yapması gerekir.

Azure yönetilen diskleri iş yükleriniz ile kullanmayı seçtiğinizde, Azure bu diski sizin için oluşturur ve yönetir. Kullanılabilir disk türleri, Ultra katı hal sürücüleri (SSD) (Önizleme), Premium SSD, Standart SSD ve standart sabit disk sürücüleri (HDD). Her bir disk türü hakkında daha fazla bilgi için bkz. [IaaS VM 'leri için disk türü seçme](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [IaaS VM 'Leri için bir disk türü seçin](disks-types.md)