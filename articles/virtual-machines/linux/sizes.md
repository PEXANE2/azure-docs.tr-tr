---
title: Azure 'da Linux VM boyutları
description: Azure 'da Linux sanal makineleri için kullanılabilen farklı boyutları listeler.
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 246ab6551667f54e3ef4ec8f91573d9aaa98d64c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81758397"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Azure'daki Linux sanal makinesi boyutları

Bu makalede, Linux uygulamalarınızı ve iş yüklerinizi çalıştırmak için kullanabileceğiniz Azure sanal makineleri için kullanılabilen Boyutlar ve seçenekler açıklanmaktadır. Ayrıca, bu kaynakları kullanmayı planlarken göz önünde bulundurmanız gereken dağıtım konuları da sunar. Bu makale [Windows sanal makineleri](../windows/sizes.md?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json)için de kullanılabilir.

| Tür | Boyutlar | Açıklama |
|------|-------|-------------|
| [Genel amaçlı](../sizes-general.md)   | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, AV2, DC, DCv2  | Dengeli CPU/bellek oranı. Test ve geliştirme, küçük-orta büyüklükteki veritabanları ve küçük-orta büyüklükte trafik hacmine sahip web sunucuları için idealdir. |
| [İşlem için iyileştirilmiş](../sizes-compute.md) | Fsv2 | Yüksek CPU-bellek oranı. Orta trafikli web sunucuları, ağ araçları, toplu süreçler ve uygulama sunucuları için iyi. |
| [Bellek için iyileştirilmiş](../sizes-memory.md) | Esv3, Ev3, Easv4, Eav4, Mv2, d, DSv2, Dv2 | Yüksek bellek-CPU oranı. İlişkisel veritabanı sunucuları, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |
| [Depolama için iyileştirilmiş](../sizes-storage.md) | Lsv2 | Büyük veri, SQL, NoSQL veritabanları, veri depolama ve büyük işlem veritabanları için ideal yüksek disk aktarım hızı ve GÇ.  |
| [GPU](../sizes-gpu.md) | NC, NCv2, NCv3, ND, NDv2 (Önizleme), NV, NVv3, NVv4 | Yoğun grafik işleme ve video düzenlemesi için hedeflenen özelleştirilmiş sanal makineler, Ayrıca, ayrıntılı öğrenme ile model eğitimi ve ınilsel (ND). Tek veya birden çok GPU ile kullanılabilir. |
| [Yüksek performanslı işlem](../sizes-hpc.md) | HB, HBv2, HC, H | İsteğe bağlı yüksek performanslı ağ arabirimlerine (RDMA) sahip en hızlı ve en güçlü CPU sanal makinelerimiz. |

- Çeşitli boyutlardaki fiyatlar hakkında daha fazla bilgi için bkz. [sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Azure bölgelerindeki VM boyutlarının kullanılabilirliği için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).
- Azure VM 'lerinde genel sınırları görmek için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md).
- Azure [işlem birimlerinin (ACU)](../acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.

## <a name="rest-api"></a>REST API

VM boyutlarını sorgulamak için REST API kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Yeniden boyutlandırma için kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Bir abonelik için kullanılabilir sanal makine boyutlarını listeleyin](https://docs.microsoft.com/rest/api/compute/resourceskus/list)
- [Kullanılabilirlik kümesindeki kullanılabilir sanal makine boyutlarını listeleme](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Azure [işlem birimlerinin (ACU)](../acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.

## <a name="benchmark-scores"></a>Kıyaslama puanları

[CoreMark kıyaslama puanlarını](compute-benchmark-scores.md)kullanarak Linux VM 'leri için işlem performansı hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir farklı VM boyutları hakkında daha fazla bilgi edinin:

- [Genel amaçlı](../sizes-general.md)
- [İşlem için iyileştirilmiş](../sizes-compute.md)
- [Bellek için iyileştirilmiş](../sizes-memory.md)
- [Depolama için iyileştirilmiş](../sizes-storage.md)
- [GPU](../sizes-gpu.md)
- [Yüksek performanslı işlem](../sizes-hpc.md)
- Standart, Dv1 (D1-4 ve D11-14 v1) ve A8-A11 serisi için [önceki nesil](../sizes-previous-gen.md) sayfayı denetleyin