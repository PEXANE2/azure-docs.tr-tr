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
ms.openlocfilehash: 8f2e2081ee31e2ce7b21677bd261ed7eda705334
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79486071"
---
[!INCLUDE [virtual-machines-disks-incremental-snapshots-description](virtual-machines-disks-incremental-snapshots-description.md)]

## <a name="regional-availability"></a>Bölgesel kullanılabilirlik
[!INCLUDE [virtual-machines-disks-incremental-snapshots-regions](virtual-machines-disks-incremental-snapshots-regions.md)]

### <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-incremental-snapshots-restrictions](virtual-machines-disks-incremental-snapshots-restrictions.md)]

## <a name="portal"></a>Portal

Genel Azure portalında artımlı anlık görüntüler oluşturmak ve yapılandırmak için [https://aka.ms/incrementalsnapshots](https://aka.ms/incrementalsnapshots)aşağıdaki bağlantıyı kullanmanız gerekir: . Artımlı anlık görüntü oluşturma henüz genel Azure portalında kullanıma sunulmadı.

1. Sağlanan bağlantıyla [Azure portalında](https://aka.ms/incrementalsnapshots) oturum açın ve anlık görüntü almak istediğiniz diske gidin.
1. Diskinizde **Anlık Görüntü Oluştur'u** seçin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-snapshot-button-incremental.png" alt-text="Ekran görüntüsü. Diskinizin bıçağı, **+Anlık görüntü oluştur** vurgulanır, çünkü seçmeniz gereken budur.":::

1. Kullanmak istediğiniz kaynak grubunu seçin ve bir ad girin.
1. **Artımlı'yı** seçin ve **Gözden Geçir + Oluştur'u** seçin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/incremental-snapshot-create-snapshot-blade.png" alt-text="Ekran görüntüsü. Bir anlık görüntü bıçağı oluşturun, adı doldurun ve artımlı seçin, ardından anlık fotoğrafınızı oluşturun.":::

1. **Oluştur**’u seçin

    :::image type="content" source="media/virtual-machines-disks-incremental-snapshots-portal/create-incremental-snapshot-validation.png" alt-text="Ekran görüntüsü. Anlık görüntünüz için doğrulama sayfası, seçimlerinizi onaylayın ve ardından anlık görüntü oluşturun.":::

## <a name="next-steps"></a>Sonraki adımlar

.NET'i kullanarak artımlı anlık görüntünün diferansiyel yeteneğini gösteren örnek kodu görmek istiyorsanız, [Azure Yönetilen Diskleri yedeklemelerini artımlı anlık görüntü fark özelliğine sahip başka bir bölgeye](https://github.com/Azure-Samples/managed-disks-dotnet-backup-with-incremental-snapshots)kopyalayın'a bakın.
