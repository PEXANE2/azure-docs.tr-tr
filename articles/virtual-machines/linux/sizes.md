---
title: Azure 'da Linux VM boyutları
description: Azure 'da Linux sanal makineleri için kullanılabilen farklı boyutları listeler.
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
ms.date: 11/06/2019
ms.author: jonbeck
ms.openlocfilehash: c16ff8fed19340811578ccb39cbac7242b7c5861
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2020
ms.locfileid: "76905750"
---
# <a name="sizes-for-linux-virtual-machines-in-azure"></a>Azure 'da Linux sanal makineleri için Boyutlar
Bu makalede, Linux uygulamalarınızı ve iş yüklerinizi çalıştırmak için kullanabileceğiniz Azure sanal makineleri için kullanılabilen Boyutlar ve seçenekler açıklanmaktadır. Ayrıca, bu kaynakları kullanmayı planlarken göz önünde bulundurmanız gereken dağıtım konuları da sunar. Bu makale [Windows sanal makineleri](../windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)için de kullanılabilir.


| Tür                     | Boyutlar           |    Açıklama       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Genel amaçlı](sizes-general.md)          | B, Dsv3, Dv3, Dasv4, Dav4, DSv2, Dv2, AV2, DC  | Dengeli CPU/bellek oranı. Test ve geliştirme, küçük - orta boyutlu veritabanları, düşük - orta yoğunluklu trafiğe sahip web sunucuları için idealdir. |
| [İşlem için iyileştirilmiş](sizes-compute.md)        | Fsv2           | Yüksek CPU/bellek oranı. Orta yoğunlukta trafiğe sahip web sunucuları, ağ gereçleri, toplu işlemler ve uygulama sunucuları için uygundur.        |
| [Bellek için iyileştirilmiş](sizes-memory.md)         | Esv3, Ev3, Easv4, Eav4, Mv2, d, DSv2, Dv2  | Yüksek bellek-CPU oranı. İlişkisel veritabanı sunucuları, orta veya büyük boyutlu önbellekler ve bellek içi analiz için idealdir.                 |
| [Depolama için iyileştirilmiş](sizes-storage.md)        | Lsv2                | Büyük veri, SQL, NoSQL veritabanları, veri depolama ve büyük işlem veritabanları için ideal yüksek disk aktarım hızı ve GÇ.  |
| [GPU](sizes-gpu.md)            | NC, NCv2, NCv3, ND, NDv2 (Önizleme), NV, NVv3  | Yoğun grafik işleme ve video düzenlemesi için hedeflenen özelleştirilmiş sanal makineler, Ayrıca, ayrıntılı öğrenme ile model eğitimi ve ınilsel (ND). Tek veya birden çok GPU ile kullanılabilir.       |
| [Yüksek performanslı işlem](sizes-hpc.md) | HB, HC, H | İşleme düzeyi yüksek olan isteğe bağlı ağ arabirimleri (RDMA) içeren sanal makineler, şimdiye kadarki en hızlı ve en güçlü CPU ile sunuluyor. |

<br>

- Çeşitli boyutlardaki fiyatlar hakkında daha fazla bilgi için bkz. [sanal makine fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/#Linux). 
- Azure bölgelerindeki VM boyutlarının kullanılabilirliği için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).
- Azure VM 'lerinde genel sınırları görmek için bkz. [Azure aboneliği ve hizmet limitleri, Kotalar ve kısıtlamalar](../../azure-resource-manager/management/azure-subscription-service-limits.md).
- Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.


## <a name="rest-api"></a>REST API

VM boyutlarını sorgulamak için REST API kullanma hakkında daha fazla bilgi için aşağıdakilere bakın:

- [Yeniden boyutlandırma için kullanılabilir sanal makine boyutlarını Listele](https://docs.microsoft.com/rest/api/compute/virtualmachines/listavailablesizes)
- [Bir abonelik için kullanılabilir sanal makine boyutlarını listeleyin](https://docs.microsoft.com/rest/api/compute/resourceskus/list)
- [Kullanılabilirlik kümesindeki kullanılabilir sanal makine boyutlarını listeleme](https://docs.microsoft.com/rest/api/compute/availabilitysets/listavailablesizes)

## <a name="acu"></a>ACU

Azure [işlem birimlerinin (ACU)](acu.md) Azure SKU 'ları genelinde işlem performansını karşılaştırmanıza nasıl yardımcı olabileceğini öğrenin.

## <a name="benchmark-scores"></a>Kıyaslama puanları

[CoreMark kıyaslama puanlarını](compute-benchmark-scores.md)kullanarak Linux VM 'leri için işlem performansı hakkında daha fazla bilgi edinin.

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir farklı VM boyutları hakkında daha fazla bilgi edinin:
- [Genel amaçlı](sizes-general.md)
- [İşlem için iyileştirilmiş](sizes-compute.md)
- [Bellek için iyileştirilmiş](sizes-memory.md)
- [Depolama için iyileştirilmiş](sizes-storage.md)
- [GPU](sizes-gpu.md)
- [Yüksek performanslı işlem](sizes-hpc.md)
- Temel ve standart, yüksek performanslı A8-A11, D/DSv1 (D1-4 ve D11-14 v1), F/Fsv1, G/GS, Lsv1 ve NVv2 serisi için [önceki nesil](sizes-previous-gen.md) sayfayı kontrol edin.



