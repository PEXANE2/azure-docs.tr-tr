---
title: Azure render başvuru mimarileri - Azure Toplu İş
description: Bir şirket içi işleme çiftliğini buluta patlatarak genişletmek için Azure Toplu İş ve diğer Azure hizmetlerini kullanmaya yönelik mimariler
services: batch
ms.service: batch
author: davefellows
manager: evansma
ms.author: labrenne
ms.date: 02/07/2019
ms.topic: conceptual
ms.custom: seodec18
ms.openlocfilehash: 20442a6618ca9357bb3be95879b68bffca45a40d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77022962"
---
# <a name="reference-architectures-for-azure-rendering"></a>Azure işleme için başvuru mimarileri

Bu makalede, bir şirket içi işleme çiftliği olan Azure'u genişletmek veya "patlama" senaryoları için üst düzey mimari diyagramları gösterilmektedir. Örnekler, Azure hesaplama, ağ ve depolama hizmetleri için farklı seçenekleri gösterir.

## <a name="hybrid-with-nfs-or-cfs"></a>NFS veya CFS ile hibrit

Aşağıdaki diyagram, aşağıdaki Azure hizmetlerini içeren karma bir senaryo gösterir:

* **İşlem** - Azure Toplu Seri havuzu veya Sanal Makine Ölçeği Kümesi.

* **Ağ** - Şirket içi: Azure ExpressRoute veya VPN. Azure: Azure VNet.

* **Depolama** - Giriş ve çıktı dosyaları: Azure VM'leri kullanan NFS veya CFS, Azure Dosya Eşitlemi veya RSync aracılığıyla şirket içi depolama yla senkronize edilmiştir. Alternatif olarak: NFS kullanan şirket içi NAS aygıtlarından giriş veya çıkış dosyaları için Avere vFXT.

  ![Bulut patlaması - NFS veya CFS ile hibrit](./media/batch-rendering-architectures/hybrid-nfs-cfs-avere.png)

## <a name="hybrid-with-blobfuse"></a>Blobfuse ile Hibrid

Aşağıdaki diyagram, aşağıdaki Azure hizmetlerini içeren karma bir senaryo gösterir:

* **İşlem** - Azure Toplu Seri havuzu veya Sanal Makine Ölçeği Kümesi.

* **Ağ** - Şirket içi: Azure ExpressRoute veya VPN. Azure: Azure VNet.

* **Depolama** - Giriş ve çıktı dosyaları: Azure Blobfuse üzerinden kaynakları hesaplamak için monte edilmiş Blob depolama.

  ![Bulut patlama - Blobfuse ile Hibrid](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Karma işlem ve depolama

Aşağıdaki diyagram, hem işlem hem de depolama için tam olarak bağlı karma bir senaryo gösterir ve aşağıdaki Azure hizmetlerini içerir:

* **İşlem** - Azure Toplu Seri havuzu veya Sanal Makine Ölçeği Kümesi.

* **Ağ** - Şirket içi: Azure ExpressRoute veya VPN. Azure: Azure VNet.

* **Depolama** - Cross-premises: Avere vFXT. Azure Veri Kutusu üzerinden blob depolamasına veya NAS hızlandırma için şirket içi Avere FXT'ye isteğe bağlı arşivleme.

  ![Bulut patlaması - Karma işlem ve depolama](./media/batch-rendering-architectures/hybrid-compute-storage-avere.png)


## <a name="next-steps"></a>Sonraki adımlar

* Azure Toplu İş [ile Render yöneticilerini](batch-rendering-render-managers.md) kullanma hakkında daha fazla bilgi edinin.

* [Azure'da Oluşturma](batch-rendering-service.md)seçenekleri hakkında daha fazla bilgi edinin.