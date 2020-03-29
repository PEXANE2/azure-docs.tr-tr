---
title: Azure'da Windows VM boyutları
description: Azure'da Windows sanal makineleri için kullanılabilen farklı boyutları listeler.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: gwallace
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: aabf0d30-04eb-4d34-b44a-69f8bfb84f22
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jonbeck
ms.openlocfilehash: 398575cc5c3dea96aa644533eb6ce8a262d1981c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78164501"
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Azure'daki Windows sanal makinesi boyutları

Bu makalede, Windows uygulamalarınızı ve iş yüklerinizi çalıştırmak için kullanabileceğiniz Azure sanal makineleri için kullanılabilir boyutlar ve seçenekler açıklanmaktadır. Ayrıca, bu kaynakları kullanmayı planlarken dikkat edilmesi gereken dağıtım konuları da sağlar.  Bu makale, [Linux sanal makineleri](../linux/sizes.md)için de kullanılabilir.

| Tür | Boyutlar | Açıklama |
|------|-------|-------------|
| [Genel amaç](../sizes-general.md) | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, Av2, DC, DCv2 | Dengeli CPU-bellek oranı. Test ve geliştirme, küçük-orta büyüklükteki veritabanları ve küçük-orta büyüklükte trafik hacmine sahip web sunucuları için idealdir. |
| [İşlem için iyileştirilmiş](../sizes-compute.md) | Fsv2 | Yüksek CPU-bellek oranı. Orta trafik web sunucuları, ağ cihazları, toplu işlem ve uygulama sunucuları için idealdir. |
| [Bellek için iyileştirilmiş](../sizes-memory.md) | Esv3, Ev3, Easv4, Eav4, Mv2, M, DSv2, Dv2 | Yüksek bellek-CPU oranı. İlişkisel veritabanı sunucuları, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir. |
| [Depolama için iyileştirilmiş](../sizes-storage.md)  | Lsv2 | Büyük Veri, SQL, NoSQL veritabanları, veri ambarı ve büyük işlem veritabanları için yüksek disk veri girişi ve IO idealdir.  |
| [GPU](../sizes-gpu.md) | NC, NCv2, NCv3, ND, NDv2 (Önizleme), NV, NVv3, NVv4 | Özel sanal makineler ağır grafik işleme ve video düzenleme için hedeflenen yanı sıra model eğitim ve derin öğrenme ile inferencing (ND). Tek veya birden fazla GPU ile kullanılabilir. |
| [Yüksek performanslı işlem](../sizes-hpc.md) | HB, HBv2, HC, H | İsteğe bağlı yüksek iş yapmalı ağ arayüzlerine (RDMA) sahip en hızlı ve en güçlü CPU sanal makinelerimiz. |

- Çeşitli boyutlarda fiyatlandırma hakkında bilgi için, [Sanal Makineler Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)bakın.
- Azure VM'lerinde genel sınırları görmek için [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalara](../../azure-subscription-service-limits.md)bakın.
- Depolama maliyetleri, depolama hesabında kullanılan sayfa sayısına göre ayrıca hesaplanır. Ayrıntılar için Azure [Depolama Fiyatlandırması.](https://azure.microsoft.com/pricing/details/storage/)
- Azure bilgi [işlem birimlerinin (ACU)](../acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.

## <a name="rest-api"></a>REST API

VM boyutları için sorgulamak için REST API'sini kullanma hakkında bilgi için aşağıdakilere bakın:

- [Yeniden boyutlandırma için kullanılabilir sanal makine boyutlarını listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Abonelik için kullanılabilir sanal makine boyutlarını listele](https://docs.microsoft.com/rest/api/compute/resourceskus/list)
- [Kullanılabilir sanal makine boyutlarını kullanılabilirlik kümesinde listele](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Azure bilgi [işlem birimlerinin (ACU)](../acu.md) Azure SK'leri genelinde bilgi işlem performansını karşılaştırmanıza nasıl yardımcı olabileceği hakkında daha fazla bilgi edinin.

## <a name="benchmark-scores"></a>Kıyaslama puanları

[CoreMark kıyaslama puanlarını](compute-benchmark-scores.md)kullanarak Windows VM'ler için bilgi işlem performansı hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilen farklı VM boyutları hakkında daha fazla bilgi edinin:

- [Genel amaç](../sizes-general.md)
- [İşlem için iyileştirilmiş](../sizes-compute.md)
- [Bellek için iyileştirilmiş](../sizes-memory.md)
- [Depolama için iyileştirilmiş](../sizes-storage.md)
- [GPU için iyileştirilmiş](../sizes-gpu.md)
- [Yüksek performanslı işlem](../sizes-hpc.md)
- Standart, Dv1 (D1-4 ve D11-14 v1) ve A8-A11 serisi için [Önceki nesil](../sizes-previous-gen.md) sayfasını kontrol edin
