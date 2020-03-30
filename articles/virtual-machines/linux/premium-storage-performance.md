---
title: "Azure Premium Depolama: Linux VM'lerde performans için tasarım | Microsoft Dokümanlar"
description: Azure premium SSD yönetilen diskleri kullanarak yüksek performanslı uygulamalar tasarla. Premium Depolama, Azure Sanal Makinelerde çalışan Yoğun I/İş yoğun iş yükleri için yüksek performanslı, düşük gecikmeli disk desteği sunar.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 9940ee4cfce9721ac65f2b3cf1469e180adfa098
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79267150"
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure premium depolama: yüksek performans için tasarım
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> Bazen, bir disk performansı sorunu gibi görünen şey aslında bir ağ darboğazdır. Bu gibi durumlarda, [ağ performansınızı](../../virtual-network/virtual-network-optimize-network-bandwidth.md)en iyi duruma getirmelisiniz.
>
> Diskinizi kıyaslamak istiyorsanız, [bir diski Kıyaslama](disks-benchmarks.md)hakkındaki makalemize bakın.
>
> VM'niz hızlandırılmış ağ iletişimini destekliyorsa, etkinleştirildiğinden emin olmalısınız. Etkin değilse, hem [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) hem de [Linux'ta](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms)zaten dağıtılan VM'lerde etkinleştirebilirsiniz.

Başlamadan önce, Premium Depolama'da yeniyseniz, premium sayfa blob depolama hesapları [için önce IaaS VM'ler ve](disks-types.md) [Ölçeklenebilirlik hedefleri için](../../storage/blobs/scalability-targets-premium-page-blobs.md)Azure disk türü seçin'i okuyun.


[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]

Diskinizi kıyaslamak istiyorsanız, [bir diski Kıyaslama](disks-benchmarks.md)hakkındaki makalemize bakın.

Kullanılabilir disk türleri hakkında daha fazla bilgi edinin: [Disk türü seçin](disks-types.md)  

SQL Server kullanıcıları için, SQL Server için En İyi Performans Uygulamaları hakkındaki makaleleri okuyun:

* [Azure Sanal Makinelerde SQL Server için En İyi Performans Uygulamaları](../windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure Premium Depolama, Azure VM'deki SQL Server için en yüksek performansı sağlar](https://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)