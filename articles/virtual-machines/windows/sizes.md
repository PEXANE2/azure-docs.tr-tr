---
title: Azure 'da VM boyutları
description: Azure 'daki sanal makineler için kullanılabilen farklı boyutları listeler.
author: ju-shim
ms.service: virtual-machines
ms.topic: conceptual
ms.workload: infrastructure-services
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: a97bd1327ab3cc2a35f94553259fb6095217fe7e
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86508228"
---
# <a name="sizes-for-windows-virtual-machines-in-azure"></a>Azure'daki Windows sanal makinesi boyutları

Bu makalede, Windows uygulamalarınızı ve iş yüklerinizi çalıştırmak için kullanabileceğiniz Azure sanal makineleri için kullanılabilen Boyutlar ve seçenekler açıklanmaktadır. Ayrıca, bu kaynakları kullanmayı planlarken göz önünde bulundurmanız gereken dağıtım konuları da sunar.  Bu makale hem Windows hem de Linux VM 'Leri için geçerlidir.

| Tür | Boyutlar | Açıklama |
|------|-------|-------------|
| [Genel amaçlı](../sizes-general.md) | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, AV2, DC, DCv2, dv4, Dsv4, Ddv4, Ddsv4 | Dengeli CPU/bellek oranı. Test ve geliştirme, küçük-orta büyüklükteki veritabanları ve küçük-orta büyüklükte trafik hacmine sahip web sunucuları için idealdir. |
| [İşlem için iyileştirilmiş](../sizes-compute.md) | Fsv2 | Yüksek CPU-bellek oranı. Orta trafikli web sunucuları, ağ araçları, toplu süreçler ve uygulama sunucuları için iyi. |
| [Bellek için iyileştirilmiş](../sizes-memory.md) | Esv3, Ev3, Easv4, Eav4, Ev4, Esv4, Edv4, Edsv4, Mv2, d, DSv2, Dv2 | Yüksek bellek-CPU oranı. İlişkisel veritabanı sunucuları, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir. |
| [Depolama için iyileştirilmiş](../sizes-storage.md)  | Lsv2 | Büyük veri, SQL, NoSQL veritabanları, veri depolama ve büyük işlem veritabanları için ideal yüksek disk aktarım hızı ve GÇ.  |
| [GPU](../sizes-gpu.md) | NC, NCv2, NCv3, ND, NDv2 (Önizleme), NV, NVv3, NVv4 | Yoğun grafik işleme ve video düzenlemesi için hedeflenen özelleştirilmiş sanal makineler, Ayrıca, ayrıntılı öğrenme ile model eğitimi ve ınilsel (ND). Tek veya birden çok GPU ile kullanılabilir. |
| [Yüksek performanslı işlem](../sizes-hpc.md) | HB, HBv2, HC, H | İsteğe bağlı yüksek performanslı ağ arabirimlerine (RDMA) sahip en hızlı ve en güçlü CPU sanal makinelerimiz. |

- Çeşitli boyutlardaki fiyatlar hakkında daha fazla bilgi için bkz. [sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows).
- Azure VM 'lerinde genel sınırları görmek için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- Depolama maliyetleri, depolama hesabında kullanılan sayfa sayısına göre ayrıca hesaplanır. Ayrıntılar için [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).
- Azure [işlem birimlerinin (ACU)](../acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.

## <a name="rest-api"></a>REST API

VM boyutlarını sorgulamak için REST API kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Yeniden boyutlandırma için kullanılabilir sanal makine boyutlarını Listele](/rest/api/compute/virtualmachines/listavailablesizes)
- [Bir abonelik için kullanılabilir sanal makine boyutlarını listeleyin](/rest/api/compute/resourceskus/list)
- [Kullanılabilirlik kümesindeki kullanılabilir sanal makine boyutlarını listeleme](/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Azure [işlem birimlerinin (ACU)](../acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.

## <a name="benchmark-scores"></a>Kıyaslama puanları

[CoreMark kıyaslama puanlarını](compute-benchmark-scores.md)kullanarak Windows VM 'lerinin işlem performansı hakkında daha fazla bilgi edinin.

## <a name="manage-costs"></a>Maliyetleri yönetme

[!INCLUDE [cost-management-horizontal](../../../includes/cost-management-horizontal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir farklı VM boyutları hakkında daha fazla bilgi edinin:

- [Genel amaçlı](../sizes-general.md)
- [İşlem için iyileştirilmiş](../sizes-compute.md)
- [Bellek için iyileştirilmiş](../sizes-memory.md)
- [Depolama için iyileştirilmiş](../sizes-storage.md)
- [GPU için iyileştirilmiş](../sizes-gpu.md)
- [Yüksek performanslı işlem](../sizes-hpc.md)
- Standart, Dv1 (D1-4 ve D11-14 v1) ve A8-A11 serisi için [önceki nesil](../sizes-previous-gen.md) sayfayı denetleyin
