---
title: Premium sayfa blob depolama hesapları için ölçeklenebilirlik hedefleri
titleSuffix: Azure Storage
description: Premium performans sayfası blob depolama hesabı okuma/yazma işlemleri için optimize edi. Bu tür depolama hesabı, Azure sanal makinesi için yönetilmeyen bir diski geri izler.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: e134c69f5d602cb4369e9410e3e2b9d3478b11a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76756258"
---
# <a name="scalability-and-performance-targets-for-premium-page-blob-storage-accounts"></a>Premium sayfa blob depolama hesapları için ölçeklenebilirlik ve performans hedefleri

[!INCLUDE [storage-scalability-intro-include](../../../includes/storage-scalability-intro-include.md)]

## <a name="scale-targets-for-premium-page-blob-accounts"></a>Premium sayfa blob hesapları için ölçek hedefleri

Bir premium performans sayfası blob depolama hesabı okuma/yazma işlemleri için optimize edidir. Bu tür depolama hesabı, Azure sanal makinesi için yönetilmeyen bir diski geri izler.

> [!NOTE]
> Microsoft, mümkünse Azure sanal makineleri (VM) ile yönetilen diskler kullanmanızı önerir. Yönetilen diskler hakkında daha fazla bilgi [için Windows VM'leri için Azure Disk Depolama genel bakışı'na](../../virtual-machines/windows/managed-disks-overview.md)bakın.

Premium sayfa blob depolama hesapları aşağıdaki ölçeklenebilirlik hedefleri vardır:

| Toplam hesap kapasitesi                            | Yerel olarak yedekli bir depolama hesabı için toplam bant genişliği                     |
| ------------------------------------------------- | --------------------------------------------------------------------------- |
| Disk kapasitesi: 4 TB (bireysel disk)/ 35 TB (tüm disklerin kümülatif toplamı) <br>Anlık görüntü kapasitesi: 10 TB | Gelen<sup>1</sup> + giden<sup>2</sup> için saniyede 50 gigadir'e kadar |

<sup>1</sup> Depolama hesabına gönderilen tüm veriler (istekler)

<sup>2</sup> Depolama hesabından alınan tüm veriler (yanıtlar)

Premium sayfa blob hesabı, premium performans için yapılandırılan genel amaçlı bir hesaptır. Genel amaçlı v2 depolama hesapları önerilir.

Yönetilmeyen diskler için premium sayfa blob depolama hesapları kullanıyorsanız ve uygulamanız tek bir depolama hesabının ölçeklenebilirlik hedeflerini aşıyorsa, Microsoft yönetilen disklere geçiş yapmanızı önerir. Yönetilen diskler hakkında daha fazla bilgi için [Windows VM'ler için Azure Disk Depolama'ya genel bakış](../../virtual-machines/windows/managed-disks-overview.md) veya Linux [VM'leri için Azure Disk Depolama'ya genel bakış'a](../../virtual-machines/linux/managed-disks-overview.md)bakın.

Yönetilen disklere geçiremiyorsanız, birden çok depolama hesabı kullanmak ve verilerinizi bu depolama hesapları arasında bölmek için uygulamanızı oluşturun. Örneğin, birden çok VM'ye 51-TB disk eklemek istiyorsanız, bunları iki depolama hesabına dağıtın. 35 TB, tek bir premium depolama hesabının sınırıdır. Tek bir premium performans depolama hesabının hiçbir zaman 35 TB'den fazla sağlanan diske sahip olmadığından emin olun.

## <a name="see-also"></a>Ayrıca bkz.

- [Standart depolama hesapları için ölçeklenebilirlik ve performans hedefleri](../common/scalability-targets-standard-account.md)
- [Premium blok blob depolama hesapları için ölçeklenebilirlik hedefleri](../blobs/scalability-targets-premium-block-blobs.md)
- [Azure abonelik sınırları ve kotaları](../../azure-resource-manager/management/azure-subscription-service-limits.md)
