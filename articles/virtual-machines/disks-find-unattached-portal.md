---
title: Eklenmemiş Azure disklerini tanımla-Azure portal
description: Azure portal kullanarak, eklenmemiş Azure yönetilen ve yönetilmeyen (VHD/sayfa blob 'ları) disklerini bulma.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 06/01/2020
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: ad01155259ea03806f56c6e75b315ff85b606220
ms.sourcegitcommit: d118ad4fb2b66c759b70d4d8a18e6368760da3ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84300438"
---
# <a name="find-and-delete-unattached-azure-managed-and-unmanaged-disks---azure-portal"></a>Eklenmemiş Azure yönetilen ve yönetilmeyen diskleri bulma ve silme-Azure portal

Azure 'da bir sanal makineyi (VM) sildiğinizde, varsayılan olarak, VM 'ye bağlı olan tüm diskler silinmez. Bu, VM 'lerin istenmeden silinmesinden dolayı veri kaybını önlemeye yardımcı olur. Bir VM silindikten sonra, eklenmemiş diskler için ödeme yapmaya devam edersiniz. Bu makalede, Azure portal kullanarak eklenmemiş disklerin nasıl bulunacağı ve silineceği ve gereksiz maliyetler azaltılacağı gösterilmektedir.

## <a name="managed-disks-find-and-delete-unattached-disks"></a>Yönetilen diskler: eklenmemiş diskleri bulma ve silme

Ekli yönetilen disklere sahipseniz ve bu verilere artık ihtiyacınız yoksa, aşağıdaki süreç Azure portal nasıl bulacağınızı açıklar:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. **Diskleri**arayın ve seçin.

    **Diskler** dikey penceresinde, tüm disklerinizin bir listesini görürsünüz. **-** **Sahip** sütununda "" olan herhangi bir disk, eklenmemiş bir disktir.

    [![](media/disks-find-unattached-portal/managed-disk-unattached-owner.png "Screenshot of the managed disks blade, if a disk has - in the Owner column, it is an unattached disk")](media/disks-find-unattached-portal/managed-disk-owner-unattached.png#lightbox)

1. Silmek istediğiniz eklenmemiş diski seçin, bu, diskin dikey penceresini açar.
1. Diskin dikey penceresinde disk durumunun eklenmemiş olduğunu onaylayıp **Sil**' i seçin.

    :::image type="content" source="media/disks-find-unattached-portal/delete-managed-disk-unattached.png" alt-text="Tek bir yönetilen diskler dikey penceresinin ekran görüntüsü. Bu dikey pencere, bağlanılıyorsa disk durumunda bağlı değil olarak gösterilir. Bu diski, verileri daha uzun süre korumanız gerekmiyorsa silebilirsiniz":::

## <a name="unmanaged-disks-find-and-delete-unattached-disks"></a>Yönetilmeyen diskler: eklenmemiş diskleri bulma ve silme

Yönetilmeyen diskler, [Azure depolama hesaplarında](../storage/common/storage-account-overview.md) [sayfa BLOBLARı](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-page-blobs) olarak depolanan VHD dosyalarıdır.

Bir VM 'ye bağlı olmayan ve bu disklere artık gerek duyulmayan yönetilmeyen diskler varsa ve bunları silmek istiyorsanız, aşağıdaki süreç Azure portal bunun nasıl yapılacağını açıklar:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Diskleri arayın ve seçin **(klasik)**.

    Tüm yönetilmeyen disklerinizin bir listesini görürsünüz. Ekli sütununda "" olan tüm diskler **-** , **Attached to** açılmamış bir disktir.

    :::image type="content" source="media/disks-find-unattached-portal/unmanaged-disk-unattached-attached-to.png" alt-text="Yönetilmeyen diskler dikey penceresinin ekran görüntüsü. Bu dikey penceredeki, ekli sütununda bulunan diskler eklenmemiş.":::

1. Silmek istediğiniz eklenmemiş diski seçin. Bu, diskin dikey penceresini getirir.

1. Belgenin dikey penceresinde, ekli olduğundan emin olun, ancak **bağlı** olmaya devam eder **-** .

    :::image type="content" source="media/disks-find-unattached-portal/unmanaged-disk-unattached-select-blade.png" alt-text="Tek bir yönetilmeyen disk dikey penceresinin ekran görüntüsü. Bağlanılıyorsa değere iliştirilmiş olacaktır. Artık bu disk verilerine ihtiyacınız yoksa silebilirsiniz.":::

1. **Sil**’i seçin.

    :::image type="content" source="media/disks-find-unattached-portal/delete-unmanaged-disk-unattached.png" alt-text="Tek bir yönetilmeyen disk dikey penceresinin ekran görüntüsü, silme vurgulaması.":::

## <a name="next-steps"></a>Sonraki adımlar

Eklenmemiş depolama hesaplarını bulmanın ve silmenin otomatik bir yolunu isterseniz, bkz. [CLI](linux/find-unattached-disks.md) veya [PowerShell](windows/find-unattached-disks.md) makalelerimiz.

Daha fazla bilgi için bkz. [bir depolama hesabını silme](../storage/common/storage-account-create.md#delete-a-storage-account) ve [PowerShell kullanarak yalnız bırakılmış diskleri tanımla](https://blogs.technet.microsoft.com/ukplatforms/2018/02/21/azure-cost-optimisation-series-identify-orphaned-disks-using-powershell/)