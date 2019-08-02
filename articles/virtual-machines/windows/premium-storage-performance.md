---
title: "Azure Premium Depolama: Windows VM 'lerinde performans için tasarım | Microsoft Docs"
description: Azure Premium Depolama kullanarak yüksek performanslı uygulamalar tasarlayın. Premium Depolama, Azure sanal makinelerinde çalışan g/ç yoğunluklu iş yükleri için yüksek performanslı ve düşük gecikmeli disk desteği sunar.
author: roygara
ms.service: virtual-machines-windows
ms.topic: conceptual
ms.date: 06/27/2017
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 3979920cac8e3ef856b3211733f08c21afac3e26
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68693689"
---
[!INCLUDE [virtual-machines-common-premium-storage-introduction](../../../includes/virtual-machines-common-premium-storage-introduction.md)]

> [!NOTE]
> Bazen bir disk performans sorunu gibi görünen durum aslında ağ performans sorununa neden olur. Bu durumlarda, [ağ performansınızı](../../virtual-network/virtual-network-optimize-network-bandwidth.md)iyileştirebilmelisiniz.
>
> Diskinizin kıyaslanmaya bakıyorsanız, [bir diski sınama](disks-benchmarks.md)hakkındaki makalemize bakın.
>
> VM 'niz hızlandırılmış ağı destekliyorsa etkinleştirildiğinden emin olun. Etkinleştirilmemişse, hem [Windows](../../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms) hem de [Linux](../../virtual-network/create-vm-accelerated-networking-cli.md#enable-accelerated-networking-on-existing-vms)üzerinde zaten dağıtılmış VM 'lerde etkinleştirebilirsiniz.

Başlamadan önce Premium Storage 'a yeni başladıysanız, önce [IaaS VM 'leri için bir Azure disk türü seçin](disks-types.md) ve [depolama hesapları için Azure depolama ölçeklenebilirlik ve performans hedefleri](../../storage/common/storage-scalability-targets.md)makalesini okuyun.

[!INCLUDE [virtual-machines-common-premium-storage-performance.md](../../../includes/virtual-machines-common-premium-storage-performance.md)]
