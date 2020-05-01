---
title: 'Azure Premium Depolama: Linux sanal makinelerinde performans için tasarım | Microsoft Docs'
description: Azure Premium SSD ile yönetilen diskleri kullanarak yüksek performanslı uygulamalar tasarlayın. Premium Depolama, Azure sanal makinelerinde çalışan g/ç yoğunluklu iş yükleri için yüksek performanslı ve düşük gecikmeli disk desteği sunar.
author: roygara
ms.service: virtual-machines-linux
ms.topic: conceptual
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 9940ee4cfce9721ac65f2b3cf1469e180adfa098
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79267150"
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium Depolama: yüksek performans için tasarım
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> Bazen bir disk performans sorunu gibi görünen durum aslında ağ performans sorununa neden olur. Bu durumlarda, [ağ performansınızı](../../virtual-network/virtual-network-optimize-network-bandwidth.md)iyileştirebilmelisiniz.
>
> Diskinizin kıyaslanmaya bakıyorsanız, [bir diski sınama](disks-benchmarks.md)hakkındaki makalemize bakın.
>
> VM 'niz hızlandırılmış ağı destekliyorsa etkinleştirildiğinden emin olun. Etkinleştirilmemişse, hem [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) hem de [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms)üzerinde zaten dağıtılmış VM 'lerde etkinleştirebilirsiniz.

Başlamadan önce Premium Storage 'a yeni başladıysanız, ilk olarak [IaaS VM 'leri Için Azure disk türünü seçin](disks-types.md) ve [Premium sayfa BLOB depolama hesapları için ölçeklenebilirlik hedeflerini](../../storage/blobs/scalability-targets-premium-page-blobs.md)okuyun.


[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]

Diskinizin kıyaslanmaya bakıyorsanız, [bir diski sınama](disks-benchmarks.md)hakkındaki makalemize bakın.

Kullanılabilir disk türleri hakkında daha fazla bilgi edinin: [bir disk türü seçin](disks-types.md)  

SQL Server kullanıcılar için SQL Server performans En Iyi yöntemleri hakkında makaleleri okuyun:

* [Azure sanal makineler 'de SQL Server için En Iyi performans uygulamaları](../windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure Premium Depolama, Azure VM 'de SQL Server için en yüksek performansı sağlar](https://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)