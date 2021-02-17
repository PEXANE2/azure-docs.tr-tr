---
title: NP serisi-Azure sanal makineleri
description: NP serisi VM 'Ler için belirtim.
author: vikancha-MSFT
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/09/2021
ms.author: vikancha
ms.openlocfilehash: 0b73697097e13f26a83eeaa72837fc00c31ef826
ms.sourcegitcommit: de98cb7b98eaab1b92aa6a378436d9d513494404
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/17/2021
ms.locfileid: "100557192"
---
# <a name="np-series-preview"></a>NP serisi (Önizleme)

NP serisi sanal makineler, makine öğrenimi çıkarımı, video dönüştürme kodu ve veritabanı arama & analizi dahil olmak üzere iş yüklerini hızlandırma için [Xilinx U250 ](https://www.xilinx.com/products/boards-and-kits/alveo/u250.html) fpgile desteklenir. NP serisi VM 'Ler, 3,2 GHz 'nin tüm çekirdek Turbo saat hızına sahip Intel Xeon 8171M (ufuk Gölü) CPU 'Ları tarafından da desteklenir.


[Premium Depolama](premium-storage-performance.md): desteklenir<br>
[Premium depolama önbelleği](premium-storage-performance.md): desteklenir<br>
[Dinamik geçiş](maintenance-and-updates.md): desteklenmiyor<br>
[Güncelleştirmeleri koruyan bellek](maintenance-and-updates.md): desteklenmiyor<br>
VM oluşturma desteği: 1. nesil<br>
[Hızlandırılmış ağ](../virtual-network/create-vm-accelerated-networking-cli.md): desteklenir<br>
[Kısa ömürlü işletim sistemi diskleri](ephemeral-os-disks.md): desteklenmez <br>
<br>

| Boyut | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | FPGA | FPGA belleği: GiB | Maksimum veri diskleri | En fazla NIC/beklenen ağ bant genişliği (MBps) | 
|---|---|---|---|---|---|---|---|
| Standard_NP10s | 10 | 168 | 736  | 1 | 64  | 8 | 1 / 7500 | 
| Standard_NP20s | 20 | 336 | 1474 | 2 | 128 | 16 | 2 / 15000 | 
| Standard_NP40s | 40 | 672 | 2948 | 4 | 256 | 32 | 4 / 30000 | 



[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler
Desteklenen işletim sistemlerinin tam listesini almak için [Xilinx çalışma zamanı (XRT) sürüm notlarını](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2020_2/ug1451-xrt-release-notes.pdf) ziyaret edin.

Önizleme programı sırasında Microsoft Azure mühendislik takımları, sürücü yüklemesi için özel yönergeleri paylaşır.

## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
