---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/02/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: de0cf260207747f4acb02a377819a13de8b8ba22
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628409"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik
[!INCLUDE [virtual-machines-disks-incremental-snapshots-regions](virtual-machines-disks-incremental-snapshots-regions.md)]

### <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="portal"></a>Portal


1. [Azure portalında](https://portal.azure.com/) oturum açın ve anlık görüntü almak istediğiniz diske gidin.
1. Diskinizde **Anlık Görüntü Oluştur'u** seçin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-snapshot-button-incremental.png" alt-text="Ekran görüntüsü. Diskinizin bıçağı, **+Anlık görüntü oluştur** vurgulanır, çünkü seçmeniz gereken budur.":::

1. Kullanmak istediğiniz kaynak grubunu seçin ve bir ad girin.
1. **Artımlı'yı** seçin ve **Gözden Geçir + Oluştur'u** seçin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/incremental-snapshot-create-snapshot-blade.png" alt-text="Ekran görüntüsü. Bir anlık görüntü bıçağı oluşturun, adı doldurun ve artımlı seçin, ardından anlık fotoğrafınızı oluşturun.":::

1. **Oluştur**’u seçin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-incremental-snapshot-validation.png" alt-text="Ekran görüntüsü. Anlık görüntünüz için doğrulama sayfası, seçimlerinizi onaylayın ve ardından anlık görüntü oluşturun.":::

## <a name="next-steps"></a>Sonraki adımlar

.NET'i kullanarak artımlı anlık görüntünün diferansiyel yeteneğini gösteren örnek kodu görmek istiyorsanız, [Azure Yönetilen Diskleri yedeklemelerini artımlı anlık görüntü fark özelliğine sahip başka bir bölgeye](https://github.com/Azure-Samples/managed-disks-dotnet-backup-with-incremental-snapshots)kopyalayın'a bakın.
