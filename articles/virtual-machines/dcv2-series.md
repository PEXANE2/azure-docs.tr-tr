---
title: DC-Series-Azure sanal makineleri
description: DC Serisi VM 'Ler için Özellikler.
services: virtual-machines
author: susaxen
ms.service: virtual-machines
ms.topic: article
ms.date: 02/20/2020
ms.author: lahugh
ms.openlocfilehash: 955b3d967d2e62e1158f329a58f040121ca8b416
ms.sourcegitcommit: 12f23307f8fedc02cd6f736121a2a9cea72e9454
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84220978"
---
# <a name="dcsv2-series"></a>DCsv2 serisi


DCsv2 serisi, genel bulutta işlendiği sırada verilerinizin ve kodunuzun gizliliğini ve bütünlüğünü korumaya yardımcı olabilir. Bu makineler, SGX teknolojisini içeren en son Intel XEON E-2288G Işlemcisi tarafından desteklenir. Intel Turbo Boost teknolojisiyle bu makineler, 5.0 GHz 'ye kadar sürebilir. DCsv2 serisi örnekler, müşterilerin, kullanıldığı sırada kod ve verilerini korumak için güvenli şifreleme tabanlı uygulamalar oluşturmasına imkan tanır.

Örnek kullanım örnekleri şunlardır: gizli multiparti veri paylaşımı, sahtekarlık algılama, para-para, blok zinciri, gizli kullanım analizi, zekası Analizi ve gizli makine öğrenimi.

Premium Depolama: desteklenen *

Premium depolama önbelleği: desteklenir *

Dinamik Geçiş: desteklenmiyor

Güncelleştirmeleri koruyan bellek: desteklenmiyor

Standard_DC8_v2 hariç *



| Boyut             | Sanal işlemci | Bellek: GiB | Geçici depolama (SSD) GiB | Maksimum veri diskleri | Maksimum önbelleğe alınmış ve geçici depolama aktarım hızı: IOPS-MB/sn (önbellek boyutu GiB biriminde) | En fazla NIC/beklenen ağ bant genişliği (MBps) | EPC belleği (MIB) |
|------------------|------|-------------|------------------------|----------------|-------------------------------------------------------------------------|----------------------------------------------|---------------------|
| Standard_DC1s_v2 | 1    | 4           | 50                     | 1              | 2000/16                                                                                               | 2   | 28                                      |
| Standard_DC2s_v2 | 2    | 8           | 100                    | 2              | 4000/32                                                                                               | 2  | 56                                          |
| Standard_DC4s_v2 | 4    | 16          | 200                    | 4              | 8000/64                                                                                               | 2  | 112                                          |
| Standard_DC8_v2  | 8   | 32          | 400                    | 8              | 16000/128                                                                                         | 2   | 168                                         |

- DCsv2 serisi VM 'Ler [2. nesil sanal makineler](./linux/generation-2.md#creating-a-generation-2-vm) ve yalnızca `Gen2` görüntüleri destekler.
- Şu anda yalnızca UK Güney, Kanada Orta ve ABD Doğu için kullanılabilir.
- Önceki nesil gizli Işlem VM 'Leri: [DC Serisi](sizes-previous-gen.md#preview-dc-series)
- [Azure Portal](./linux/quick-create-portal.md) veya [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-azure-compute.acc-virtual-machine-v2?tab=overview) 'ni kullanarak DCsv2 VM oluşturma



## <a name="other-sizes"></a>Diğer boyutlar

- [Genel amaçlı](sizes-general.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU için iyileştirilmiş](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- [Önceki nesiller](sizes-previous-gen.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.
