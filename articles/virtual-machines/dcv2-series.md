---
title: DC serisi - Azure Sanal Makineler
description: DC serisi VM'ler için teknik özellikler.
services: virtual-machines
author: susaxen
ms.service: virtual-machines
ms.topic: article
ms.date: 02/20/2020
ms.author: lahugh
ms.openlocfilehash: d35e37e53b84d317446a93a2301fc3b703b426b7
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82085731"
---
# <a name="dcsv2-series"></a>DCsv2 serisi


DCsv2 serisi, genel bulutta işlenirken verilerinizin ve kodlarınızın gizliliğini ve bütünlüğünü korumaya yardımcı olabilir. Bu makineler, SGX teknolojisine sahip en son nesil Intel XEON E-2288G İşlemci tarafından desteklenen bir cihazdır. Intel Turbo Boost Teknolojisi ile bu makineler 5.0GHz'e kadar çıkabiliyor. DCsv2 serisi örnekleri, müşterilerin kullanımdayken kodlarını ve verilerini korumak için güvenli enklav tabanlı uygulamalar oluşturmasına olanak tanır.

Örnek kullanım durumları şunlardır: gizli çok taraflı veri paylaşımı, dolandırıcılık algılama, kara para aklamayla mücadele, blockchain, gizli kullanım analitiği, istihbarat analizi ve gizli makine öğrenimi.

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
- Şu anda yalnızca İngiltere Güney, Kanada Orta ve ABD Doğu'da mevcuttur.
- Önceki nesil Gizli İşlem VM'leri: [DC serisi](sizes-previous-gen.md#preview-dc-series)
- [Azure portalını](./linux/quick-create-portal.md) veya [Azure Marketini](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview) kullanarak DCsv2 VM'leri oluşturun



## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure bilgi [işlem birimlerinin (ACU)](acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.
