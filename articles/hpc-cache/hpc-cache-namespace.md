---
title: Azure HPC Önbelleği Oluşturma
description: Azure HPC Önbellek örneği oluşturma
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: rohogue
ms.openlocfilehash: aaa939051a1aeafdb0650119772fc7214506aa8d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73582190"
---
# <a name="plan-the-aggregated-namespace"></a>Toplanan ad alanını planlama

Azure HPC Önbelleği, istemcilerin arka uç depolama sisteminin ayrıntılarını gizleyen sanal bir ad alanı aracılığıyla çeşitli depolama sistemlerine erişmesine olanak tanır.

Bir depolama hedefi eklediğinizde, istemciye bakan dosya yolunu ayarlarsınız. İstemci makineleri bu dosya yolunu bağlar ve depolama sistemini doğrudan monte etmek yerine önbelleğe dosya okuma isteklerini yapabilir.

Azure HPC Önbelleği bu sanal dosya sistemini yönettiğinden, istemciye bakan yolu değiştirmeden depolama hedefini değiştirebilirsiniz. Örneğin, istemciye yönelik yordamları yeniden yazmaya gerek kalmadan bir donanım depolama sistemini bulut depolama yla değiştirebilirsiniz.

## <a name="aggregated-namespace-example"></a>Toplu ad alanı örneği

Müşteri makinelerinin gereksinim duydukları bilgilere kolayca ulaşabilmesi ve yöneticilerin ve iş akışı mühendislerinin yolları kolayca ayırt edebilmeleri için toplu ad alanınızı planlayın.

Örneğin, Azure Blob'da depolanan verileri işlemek için bir Azure HPC Önbellek örneğinin kullanıldığı bir sistem düşünün. Çözümleme, şirket içi bir veri merkezinde depolanan şablon dosyaları gerektirir.

Şablon verileri bir veri merkezinde depolanır ve bu iş için gereken bilgiler bu alt dizinlerde depolanır:

    /goldline/templates/acme2017/sku798
    /goldline/templates/acme2017/sku980 

Veri merkezi depolama sistemi şu dışa aktarımları ortaya çıkarır:

    /
    /goldline
    /goldline/templates

Analiz edilecek veriler [CLFSLoad yardımcı programı](hpc-cache-ingest.md#pre-load-data-in-blob-storage-with-clfsload)kullanılarak "sourcecollection" adlı bir Azure Blob depolama kapsayıcısına kopyalanmıştır.

Önbellek üzerinden kolay erişim sağlamak için, bu sanal ad alanı yolları ile depolama hedefleri oluşturmayı düşünün:

| Arka uç depolama sistemi <br/> (NFS dosya yolu veya Blob kapsayıcısı) | Sanal ad alanı yolu |
|-----------------------------------------|------------------------|
| /goldline/şablonlar/acme2017/sku798     | /şablonlar/sku798      |
| /goldline/şablonlar/acme2017/sku980     | /şablonlar/sku980      |
| kaynak koleksiyonu                        | /kaynak/               |

Her biri benzersiz bir dışa aktarma yoluna başvurulsa, Bir NFS depolama hedefinin birden çok sanal ad alanı yolu olabilir.

NFS kaynak yolları aynı dışa aktarmanın alt dizinleri olduğundan, aynı depolama hedefinden birden çok ad alanı yolu tanımlamanız gerekir.

| Depolama hedef ana bilgisayar adı  | NFS dışa aktarma yolu      | Alt dizin yolu | Ad alanı yolu    |
|--------------------------|----------------------|-------------------|-------------------|
| *IP adresi veya ana bilgisayar adı* | /goldline/şablonlar  | acme2017/sku798   | /şablonlar/sku798 |
| *IP adresi veya ana bilgisayar adı* | /goldline/şablonlar  | acme2017/sku980   | /şablonlar/sku980 |

İstemci uygulaması önbelleği monte edebilir ve toplu ``/source``ad ``/templates/sku798``alanı ``/templates/sku980``dosya yollarına kolayca erişebilir, ve .

## <a name="next-steps"></a>Sonraki adımlar

Sanal dosya sisteminizi nasıl ayarlayabileceğinize karar verdikten sonra, arka uç depolama alanınızı istemciye bakan sanal dosya yollarınız ile eşlemek için [depolama hedefleri oluşturun.](hpc-cache-add-storage.md)
