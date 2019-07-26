---
title: Linux VM 'Ler için yönetilen disk Azure Disk Depolama genel bakış | Microsoft Docs
description: Linux VM 'lerini kullanırken depolama hesaplarını işleyen Azure yönetilen disklere genel bakış
services: virtual-machines-linux,storage
author: roygara
ms.service: virtual-machines-linux
ms.tgt_pltfrm: vm-linux
ms.topic: overview
ms.date: 04/22/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 18dc1bd2eea232d0c2eb73d496dd4bd9d2d5016e
ms.sourcegitcommit: 57a7d4f67635212f5bf0c56e58fd87c8ec366f2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2019
ms.locfileid: "64707812"
---
# <a name="introduction-to-azure-managed-disks"></a>Azure yönetilen disklere giriş

Azure yönetilen disk, bir sanal sabit disktir (VHD). Bunu, şirket içi sunucuda bir fiziksel disk gibi düşünebilirsiniz, ancak sanallaştırılabilir. Azure yönetilen diskler, Azure 'daki bir rastgele GÇ depolama nesnesi olan sayfa Blobları olarak depolanır. Sayfa Blobları, blob kapsayıcıları ve Azure depolama hesapları üzerinde bir soyutlama olduğundan, yönetilen bir disk ' Managed ' çağrısı yaptık. Yönetilen disklerle, tüm yapmanız gereken diski temin etmek ve Azure 'un geri kalanını yapması gerekir.

Azure yönetilen diskleri iş yükleriniz ile kullanmayı seçtiğinizde, Azure bu diski sizin için oluşturur ve yönetir. Kullanılabilir disk türleri Ultra diskler (Önizleme), Premium katı hal sürücüleri (SSD), standart SSD ve standart sabit disk sürücüleri (HDD). Her bir disk türü hakkında daha fazla bilgi için bkz. [IaaS VM 'leri için disk türü seçme](disks-types.md).

[!INCLUDE [virtual-machines-managed-disks-overview.md](../../../includes/virtual-machines-managed-disks-overview.md)]

> [!div class="nextstepaction"]
> [IaaS VM 'Leri için bir disk türü seçin](disks-types.md)
