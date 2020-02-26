---
title: Azure 'da Windows VM boyutları
description: Azure 'da Windows sanal makineleri için kullanılabilen farklı boyutları listeler.
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
ms.openlocfilehash: 755ea3ea3c246a1fa9fb7d514569c6fcecc0c04c
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77598723"
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Azure 'da Windows sanal makineleri için Boyutlar

Bu makalede, Windows uygulamalarınızı ve iş yüklerinizi çalıştırmak için kullanabileceğiniz Azure sanal makineleri için kullanılabilen Boyutlar ve seçenekler açıklanmaktadır. Ayrıca, bu kaynakları kullanmayı planlarken göz önünde bulundurmanız gereken dağıtım konuları da sunar.  Bu makale, [Linux sanal makineleri](../linux/sizes.md)için de kullanılabilir.

| Tür | Boyutlar | Açıklama |
|------|-------|-------------|
| [Genel amaçlı](../sizes-general.md) | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, AV2, DC | Dengeli CPU/bellek oranı. Test ve geliştirme için idealdir, küçük ve orta ölçekli veritabanları ve düşük düzeyli trafik Web sunucuları. |
| [İşlem için iyileştirilmiş](../sizes-compute.md) | Fsv2 | Yüksek CPU-bellek oranı. Orta trafikli web sunucuları, ağ araçları, toplu süreçler ve uygulama sunucuları için iyi. |
| [Bellek için iyileştirilmiş](../sizes-memory.md) | Esv3, Ev3, Easv4, Eav4, Mv2, d, DSv2, Dv2 | Yüksek bellek-CPU oranı. İlişkisel veritabanı sunucuları, orta ve büyük önbellekler ve bellek içi analiz için harika. |
| [Depolama için iyileştirilmiş](../sizes-storage.md)  | Lsv2 | Büyük veri, SQL, NoSQL veritabanları, veri depolama ve büyük işlem veritabanları için ideal yüksek disk aktarım hızı ve GÇ.  |
| [GPU](../sizes-gpu.md) | NC, NCv2, NCv3, ND, NDv2 (Önizleme), NV, NVv3, NVv4 | Yoğun grafik işleme ve video düzenlemesi için hedeflenen özelleştirilmiş sanal makineler, Ayrıca, ayrıntılı öğrenme ile model eğitimi ve ınilsel (ND). Tek veya birden çok GPU ile kullanılabilir. |
| [Yüksek performanslı işlem](../sizes-hpc.md) | HB, HC, H | İsteğe bağlı yüksek performanslı ağ arabirimlerine (RDMA) sahip en hızlı ve en güçlü CPU sanal makinelerimiz. |

- Çeşitli boyutlardaki fiyatlar hakkında daha fazla bilgi için bkz. [sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).
- Azure VM 'lerinde genel sınırları görmek için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-subscription-service-limits.md).
- Depolama maliyetleri, depolama hesabında kullanılan sayfa sayısına göre ayrıca hesaplanır. Ayrıntılar için [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).
- Azure [işlem birimlerinin (ACU)](../acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.

## <a name="rest-api"></a>REST API

VM boyutlarını sorgulamak için REST API kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Yeniden boyutlandırma için kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Bir abonelik için kullanılabilir sanal makine boyutlarını listeleyin](https://docs.microsoft.com/rest/api/compute/resourceskus/list)
- [Kullanılabilirlik kümesindeki kullanılabilir sanal makine boyutlarını listeleme](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Azure [işlem birimlerinin (ACU)](../acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.

## <a name="benchmark-scores"></a>Kıyaslama puanları

[CoreMark kıyaslama puanlarını](compute-benchmark-scores.md)kullanarak Windows VM 'lerinin işlem performansı hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir farklı VM boyutları hakkında daha fazla bilgi edinin:

- [Genel amaçlı](../sizes-general.md)
- [İşlem için iyileştirilmiş](../sizes-compute.md)
- [Bellek için iyileştirilmiş](../sizes-memory.md)
- [Depolama için iyileştirilmiş](../sizes-storage.md)
- [GPU için iyileştirilmiş](../sizes-gpu.md)
- [Yüksek performanslı işlem](../sizes-hpc.md)
- Standart, Dv1 (D1-4 ve D11-14 v1) ve A8-A11 serisi için [önceki nesil](../sizes-previous-gen.md) sayfayı denetleyin
