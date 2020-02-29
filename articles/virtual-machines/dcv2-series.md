---
title: DC-Series-Azure sanal makineleri
description: DC Serisi VM 'Ler için Özellikler.
services: virtual-machines
author: joelpelley
ms.service: virtual-machines
ms.topic: article
ms.date: 02/20/2020
ms.author: lahugh
ms.openlocfilehash: 41b08a921b4cc71275c216e86e89e49b59fc950a
ms.sourcegitcommit: 1f738a94b16f61e5dad0b29c98a6d355f724a2c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2020
ms.locfileid: "78163752"
---
# <a name="preview-dcv2-series"></a>Önizleme: DCv2-Series


DCv2 serisi, genel bulutta işlendiği sırada verilerinizin ve kodunuzun gizliliğini ve bütünlüğünü korumaya yardımcı olabilir. Bu makineler, SGX teknolojisini içeren en son Intel XEON E-2288G Işlemcisi tarafından desteklenir. Intel Turbo Boost teknolojisiyle bu makineler, 5.0 GHz 'ye kadar sürebilir. DCv2 serisi örnekler, müşterilerin, kullanıldığı sırada kod ve verilerini korumak için güvenli şifreleme tabanlı uygulamalar oluşturmasına imkan tanır.

Örnek kullanım örnekleri arasında gizli çok taraflı veri paylaşımı, sahtekarlık algılama, para olumsuz korunma, blok zinciri, gizli kullanım analizi, zekası Analizi ve gizli makine öğrenimi bulunur.

Premium Depolama: desteklenen *

Premium depolama önbelleği: desteklenir *

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

Standard_DC8_v2 hariç *



| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | En fazla NIC/beklenen ağ bant genişliği (MBps) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|-------------------------------------------|----------------------------------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2000/16 (21)                                                            | 1600/24                                   | 2                                            |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4000/32 (43)                                                            | 3200/48                                   | 2                                            |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8000/64 (86)                                                            | 6400/96                                   | 2                                            |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16000/128 (172)                                                         | 12800/192                                 | 2                                            |

- DCv2 serisi VM 'Ler [2. nesil sanal makineler](./linux/generation-2.md#creating-a-generation-2-vm) ve yalnızca `Gen2` görüntülerini destekler.




## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
