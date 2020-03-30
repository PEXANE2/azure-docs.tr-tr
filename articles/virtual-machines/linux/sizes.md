---
title: Azure'da Linux VM boyutları
description: Azure'da Linux sanal makineleri için kullanılabilen farklı boyutları listeler.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: da681171-f045-4c80-a5a9-d8bd47964673
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 46fad0dd2e39f1f563ed248458da622ad83f061e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78161125"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Azure'daki Linux sanal makinesi boyutları

Bu makalede, Linux uygulamalarınızı ve iş yüklerinizi çalıştırmak için kullanabileceğiniz Azure sanal makineleri için kullanılabilir boyutlar ve seçenekler açıklanmaktadır. Ayrıca, bu kaynakları kullanmayı planlarken dikkat edilmesi gereken dağıtım konuları da sağlar. Bu makale, [Windows sanal makineleri](../windows/sizes.md?toc=/azure/virtual-machines/windows/toc.json&bc=/azure/virtual-machines/windows/breadcrumb/toc.json)için de kullanılabilir.

| Tür | Boyutlar | Açıklama |
|------|-------|-------------|
| [Genel amaç](../sizes-general.md)   | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC, DCv2  | Dengeli CPU-bellek oranı. Test ve geliştirme, küçük-orta büyüklükteki veritabanları ve küçük-orta büyüklükte trafik hacmine sahip web sunucuları için idealdir. |
| [İşlem için iyileştirilmiş](../sizes-compute.md) | Fsv2 | Yüksek CPU-bellek oranı. Orta trafik web sunucuları, ağ cihazları, toplu işlem ve uygulama sunucuları için idealdir. |
| [Bellek için iyileştirilmiş](../sizes-memory.md) | Esv3, Ev3, Easv4, Eav4, Mv2, M, DSv2, Dv2 | Yüksek bellek-CPU oranı. İlişkisel veritabanı sunucuları, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |
| [Depolama için iyileştirilmiş](../sizes-storage.md) | Lsv2 | Büyük Veri, SQL, NoSQL veritabanları, veri ambarı ve büyük işlem veritabanları için yüksek disk veri girişi ve IO idealdir.  |
| [GPU](../sizes-gpu.md) | NC, NCv2, NCv3, ND, NDv2 (Önizleme), NV, NVv3, NVv4 | Özel sanal makineler ağır grafik işleme ve video düzenleme için hedeflenen yanı sıra model eğitim ve derin öğrenme ile inferencing (ND). Tek veya birden fazla GPU ile kullanılabilir. |
| [Yüksek performanslı işlem](../sizes-hpc.md) | HB, HBv2, HC, H | İsteğe bağlı yüksek iş yapmalı ağ arayüzlerine (RDMA) sahip en hızlı ve en güçlü CPU sanal makinelerimiz. |

- Çeşitli boyutlarda fiyatlandırma hakkında bilgi için, [Sanal Makineler Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux)bakın. 
- Azure bölgelerindeki VM boyutlarının kullanılabilirliği için [bölgeye göre kullanılabilen Ürünler'e](https://azure.microsoft.com/regions/services/)bakın.
- Azure VM'lerinde genel sınırları görmek için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalara](../../azure-subscription-service-limits.md)bakın.
- Azure bilgi [işlem birimlerinin (ACU)](../acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.

## <a name="rest-api"></a>REST API

VM boyutları için sorgulamak için REST API'sini kullanma hakkında bilgi için aşağıdakilere bakın:

- [Yeniden boyutlandırma için kullanılabilir sanal makine boyutlarını listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Abonelik için kullanılabilir sanal makine boyutlarını listele](https://docs.microsoft.com/rest/api/compute/resourceskus/list)
- [Kullanılabilir sanal makine boyutlarını kullanılabilirlik kümesinde listele](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Azure bilgi [işlem birimlerinin (ACU)](../acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.

## <a name="benchmark-scores"></a>Kıyaslama puanları

[CoreMark kıyaslama puanlarını](compute-benchmark-scores.md)kullanarak Linux VM'leri için bilgi işlem performansı hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilen farklı VM boyutları hakkında daha fazla bilgi edinin:

- [Genel amaç](../sizes-general.md)
- [İşlem için iyileştirilmiş](../sizes-compute.md)
- [Bellek için iyileştirilmiş](../sizes-memory.md)
- [Depolama için iyileştirilmiş](../sizes-storage.md)
- [GPU](../sizes-gpu.md)
- [Yüksek performanslı işlem](../sizes-hpc.md)
- Standart, Dv1 (D1-4 ve D11-14 v1) ve A8-A11 serisi için [Önceki nesil](../sizes-previous-gen.md) sayfasını kontrol edin