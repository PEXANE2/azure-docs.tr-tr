---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 05/11/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 5f14a269478541eaa5852697a917afb3d771841a
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83196965"
---
Şimdilik, Ultra diskler ek sınırlamalara sahiptir ve bunlar şu şekildedir:

Şu anda Ultra diskler için kullanılabilen tek altyapı artıklığı seçenekleri kullanılabilirlik bölgeleri ' dir. Diğer artıklık seçeneklerini kullanan VM 'Ler bir ultra disk iliştiremezsiniz.

Aşağıdaki tabloda, Ultra disklerin ' de kullanılabildiği bölgeler ve bunlara karşılık gelen kullanılabilirlik seçenekleri de özetlenmektedir:

> [!NOTE]
> Aşağıdaki listede yer alan bir bölgede Ultra disk özellikli kullanılabilirlik alanları yoksa, bir ultra disk iliştirmek için bu bölgedeki VM 'Ler herhangi bir altyapı artıklık seçeneği olmadan dağıtılmalıdır.

|Bölgeler  |Ultra diskleri destekleyen kullanılabilirlik bölgesi sayısı  |
|---------|---------|
|US Gov Virginia     |Yok         |
|Orta Güney ABD     |Yok         |
|Orta ABD     |Üç bölge         |
|Batı ABD     |Yok         |
|Batı ABD 2    |Üç bölge         |
|Doğu ABD     |Üç bölge         |
|Doğu ABD 2     |İki bölge         |
|Güneydoğu Asya     |Üç bölge         |
|Kuzey Avrupa     |Üç bölge          |
|Batı Avrupa     |Üç bölge          |
|Güney Birleşik Krallık     |Üç bölge          |
|Doğu Japonya     |İki bölge         |



- Yalnızca şu VM serisinde desteklenir:
    - [ESv3](../articles/virtual-machines/ev3-esv3-series.md#esv3-series)
    - [DSv3](../articles/virtual-machines/dv3-dsv3-series.md#dsv3-series)
    - [FSv2](../articles/virtual-machines/fsv2-series.md)
    - [LSv2](../articles/virtual-machines/lsv2-series.md)
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Her VM boyutu, tüm desteklenen bölgelerde Ultra disklerle kullanılamaz
- Yalnızca veri diskleri olarak kullanılabilir ve yalnızca 4k fiziksel sektör boyutunu destekler. Ultra diskin 4K yerel sektör boyutu nedeniyle, Ultra disklerle uyumlu olmayacak bazı uygulamalar vardır. Oracle Database bir örnek, Ultra diskleri desteklemek için sürüm 12,2 veya sonraki bir sürümü gerektirir.  
- Yalnızca boş disk olarak oluşturulabilir  
- Şu anda disk anlık görüntülerini, VM görüntülerini, kullanılabilirlik kümelerini, Azure adanmış Konakları veya Azure disk şifrelemesini desteklemez
- Azure Backup veya Azure Site Recovery tümleştirmesini desteklememektedir
- GA VM 'lerde ıOPS için geçerli en büyük sınır 80.000 ' dir.

Azure Ultra diskler, varsayılan olarak her abonelik için her bölge için en fazla 16 TiB sunar, ancak Ultra diskler isteğe göre daha yüksek kapasiteyi destekler. Kapasiteden artış istemek için Azure desteğine başvurun.