---
title: Microsoft Azure FXT Edge Filer'a genel bakış
description: Yüksek performanslı bilgi işlem için etkin bir arşiv ve dosya erişim hızlandırıcı çözümü olan Azure FXT Edge Filer karma depolama önbelleğini açıklar
author: ekpgh
ms.service: fxt-edge-filer
ms.topic: overview
ms.date: 07/01/2019
ms.author: rohogue
ms.openlocfilehash: acf35015ff7851f7ea2a2527852be2573512e35d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "72254848"
---
# <a name="what-is-azure-fxt-edge-filer-hybrid-storage-cache"></a>Azure FXT Edge Filer hibrit depolama önbelleği nedir?

Azure FXT Edge Filer, yüksek performanslı bilgi işlem (HPC) görevleri için hızlı dosya erişimi ve etkin arşiv sağlayan karma bir depolama önbelleğe alma cihazıdır.

İster yerel bir veri merkezinde, ister uzaktan veya bulutta depolanmış olsun, birden çok veri kaynağıyla çalışır. Azure FXT Edge Filer, çeşitli depolama sistemlerindeki veriler için birleşik bir ad alanı sağlayabilir.

Üç veya daha fazla FXT Edge Filer donanım aygıtı önbelleği sağlamak için kümelenmiş bir dosya sistemi olarak birlikte çalışır. Gerekli donanımı satın alma hakkında daha fazla bilgi için Microsoft temsilcinize başvurun. 

Daha fazla bilgi edinmek için Azure [FXT Edge Filer'daki](https://azure.microsoft.com/services/fxt-edge-filer/)ürün bilgilerini ve veri sayfasını okuyun.

## <a name="use-cases"></a>Uygulama alanları

Azure FXT Edge Filer, aşağıdaki gibi iş akışları için üretkenliği en iyi şekilde artırır:

* Okuma ağırlıklı dosya erişimi iş akışı 
* NFSv3 veya SMB2 protokolleri
* 1000 ila 100.000 CPU çekirdeğinin işlem çiftlikleri

### <a name="nas-optimization-and-scaling"></a>NAS optimizasyonu ve ölçekleme

Mevcut NetApp ve Dell EMC Isilon NAS sistemlerine erişimi kolaylaştırmak için Azure FXT Edge Filer önbelleğini kullanabilirsiniz. Ayrıca, istemci tarafındaki veri erişim işlemlerini yeniden çalışmanız gerekmeden ölçeklenebilirlik sağlamak için Azure Blob veya başka bir bulut depolama alanı ekleyebilirsiniz. 

### <a name="wan-caching"></a>WAN önbelleğe alma

Azure FXT Edge Filer, ihtiyaç duydukları veriler başka bir yerde depolandığında güç kullanıcılarının hızlı dosya erişimini desteklemek için kullanılabilir. Yedekleme ve diğer veri yönetim sistemlerini merkezi bir veri merkezinde tutarken erişim sağlayın. 

### <a name="active-archive-in-azure-blob"></a>Azure Blob'da etkin arşiv

Erişim noktası olarak Azure FXT Edge Filer ile veri merkezinizi bulut depolama alanına genişletin. 

## <a name="features"></a>Özellikler 

İki donanım modeli mevcuttur. 

| Model | Dramı | NVMe SSD | Ağ bağlantı noktaları | 
|-------|------|----------|---------------|
| FXT 6600 | 1536 GB | 25.6 TB | 6 x 25Gb/10Gb + 2 x 1Gb |
| FXT 6400 | 768 GB | 12.8 TB | 6 x 25Gb/10Gb + 2 x 1Gb |


## <a name="next-steps"></a>Sonraki adımlar

* [Özellikleri](fxt-specs.md) veya [yükleme öğreticisini](fxt-install.md)okuyarak Azure FXT Edge Filer hakkında bilgi edinmeye devam edin.
* [Azure FXT Edge Filer ürün sayfasından Azure FXT Edge Filer'ı](https://azure.microsoft.com/services/fxt-edge-filer/)nasıl satın alabildiğini öğrenin.