---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 04/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a5e0e459800e7cb57672518597f3d04a74f53118
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81008664"
---
Şimdilik, Ultra diskler ek sınırlamalara sahiptir ve bunlar şu şekildedir:

Şu anda Ultra diskler için kullanılabilen tek altyapı artıklığı seçenekleri kullanılabilirlik bölgeleri ' dir. Diğer artıklık seçeneklerini kullanan VM 'Ler bir ultra disk iliştiremezsiniz.

Aşağıdaki tabloda, Ultra disklerin ' de kullanılabildiği bölgeler ve bunlara karşılık gelen kullanılabilirlik seçenekleri de özetlenmektedir:

> [!NOTE]
> Bu bölgelerdeki bazı kullanılabilirlik bölgeleri Ultra diskler sunmaz.

|Bölgeler  |Altyapı yedekliliği yok  |Kullanılabilirlik alanları  |
|---------|---------|---------|
|Batı ABD     |Yes         |Hayır         |
|Batı ABD 2    |Hayır         |Yes         |
|Doğu ABD     |Hayır         |Yes         |
|Doğu ABD 2     |Hayır         |Yes         |
|Güneydoğu Asya     |Hayır         |Yes         |
|Kuzey Avrupa     |Hayır         |Yes         |
|Batı Avrupa     |Hayır         |Yes         |
|Güney Birleşik Krallık     |Hayır         |Yes         |

- Yalnızca şu VM serisinde desteklenir:
    - [ESv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - [DSv3](https://azure.microsoft.com/blog/introducing-the-new-dv3-and-ev3-vm-sizes/)
    - FSv2
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Her VM boyutu, tüm desteklenen bölgelerde Ultra disklerle kullanılamaz
- Yalnızca veri diskleri olarak kullanılabilir ve yalnızca 4k fiziksel sektör boyutunu destekler. Ultra diskin 4K yerel sektör boyutu nedeniyle, Ultra disklerle uyumlu olmayacak bazı uygulamalar vardır. Oracle Database bir örnek, Ultra diskleri desteklemek için sürüm 12,2 veya sonraki bir sürümü gerektirir.  
- Yalnızca boş disk olarak oluşturulabilir  
- Şu anda disk anlık görüntülerini, VM görüntülerini, kullanılabilirlik kümelerini, Azure adanmış Konakları veya Azure disk şifrelemesini desteklemez
- Azure Backup veya Azure Site Recovery tümleştirmesini desteklememektedir
- GA VM 'lerde ıOPS için geçerli en büyük sınır 80.000 ' dir.

Azure Ultra diskler, varsayılan olarak her abonelik için her bölge için en fazla 16 TiB sunar, ancak Ultra diskler isteğe göre daha yüksek kapasiteyi destekler. Kapasiteden artış istemek için Azure desteğine başvurun.