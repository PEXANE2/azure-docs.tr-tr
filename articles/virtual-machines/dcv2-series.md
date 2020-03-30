---
title: DC serisi - Azure Sanal Makineler
description: DC serisi VM'ler için teknik özellikler.
services: virtual-machines
author: susaxen
ms.service: virtual-machines
ms.topic: article
ms.date: 02/20/2020
ms.author: lahugh
ms.openlocfilehash: 7834c8a32d4d85fc354bac209e13f19f3b8315fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80256938"
---
# <a name="preview-dcsv2-series"></a>Önizleme: DCsv2 serisi


DCsv2 serisi, genel bulutta işlenirken verilerinizin ve kodlarınızın gizliliğini ve bütünlüğünü korumaya yardımcı olabilir. Bu makineler, SGX teknolojisine sahip en son nesil Intel XEON E-2288G İşlemci tarafından desteklenen bir cihazdır. Intel Turbo Boost Teknolojisi ile bu makineler 5.0GHz'e kadar çıkabiliyor. DCsv2 serisi örnekleri, müşterilerin kullanımdayken kodlarını ve verilerini korumak için güvenli enklav tabanlı uygulamalar oluşturmasına olanak tanır.

Örnek kullanım durumları arasında gizli çok taraflı veri paylaşımı, dolandırıcılık algılama, kara para aklamayla mücadele, blockchain, gizli kullanım analitiği, istihbarat analizi ve gizli makine öğrenimi sayılabilir.

Premium Depolama: Desteklenen*

Premium Depolama önbelleğe alma: Desteklenen*

Canlı Geçiş: Desteklenmiyor

Bellek Koruma Güncelleştirmeleri: Desteklenmiyor

*Standard_DC8_v2 hariç



| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | Maksimum önbelleğe alınmamış disk aktarım hızı: IOPS-MB/sn | Max NIC ' ler / Beklenen ağ bant genişliği (MBps) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|-------------------------------------------|----------------------------------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2000/16 (21)                                                            | 1600/24                                   | 2                                            |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4000/32 (43)                                                            | 3200/48                                   | 2                                            |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8000/64 (86)                                                            | 6400/96                                   | 2                                            |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16000/128 (172)                                                         | 12800/192                                 | 2                                            |

- DCsv2 serisi VM'ler nesil 2 `Gen2` [VM'lerdir](./linux/generation-2.md#creating-a-generation-2-vm) ve yalnızca görüntüleri destekler.
- Şu anda sadece İngiltere Güney ve Kanada Merkez mevcuttur.
- Önceki nesil Gizli İşlem VM'leri: [DC Serisi](sizes-previous-gen.md)
- Azure Portal Oluşturma VM kullanarak DCsv2 [VM'ler oluşturun - Portal](./linux/quick-create-portal.md)



## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaç](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
