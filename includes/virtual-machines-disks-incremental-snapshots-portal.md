---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/12/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 64aa01995460837c820c90010d7c4e3f3d78e6a2
ms.sourcegitcommit: c29b7870f1d478cec6ada67afa0233d483db1181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79300238"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik
[!INCLUDE [virtual-machines-disks-incremental-snapshots-regions](virtual-machines-disks-incremental-snapshots-regions.md)]

### <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="portal"></a>Portal

Genel Azure portal Artımlı anlık görüntüler oluşturup yapılandırmak için şu bağlantıyı kullanmanız gerekir: [https://aka.ms/incrementalsnapshots](https://aka.ms/incrementalsnapshots). Artımlı anlık görüntü oluşturma genel Azure portal henüz kullanılamıyor.

1. [Azure Portal](https://aka.ms/incrementalsnapshots) , belirtilen bağlantıyla oturum açın ve anlık görüntü eklemek istediğiniz diske gidin.
1. Diskinizde **anlık görüntü oluştur** ' u seçin.

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-snapshot-button-incremental.png" alt-text=" ":::

1. Kullanmak istediğiniz kaynak grubunu seçin ve bir ad girin.
1. **Artımlı** ' i seçin ve **gözden geçir + oluştur** seçeneğini belirleyin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/incremental-snapshot-create-snapshot-blade.png" alt-text=" ":::

1. **Oluştur**’u seçin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-incremental-snapshot-validation.png" alt-text=" ":::

## <a name="next-steps"></a>Sonraki adımlar

.NET kullanarak Artımlı anlık görüntülerin fark yeteneğini gösteren örnek kodu görmek isterseniz, bkz. [Azure yönetilen diskler yedeklemelerini, artımlı anlık görüntülerin fark yeteneği ile başka bir bölgeye kopyalama](https://github.com/Azure-Samples/managed-disks-dotnet-backup-with-incremental-snapshots).
