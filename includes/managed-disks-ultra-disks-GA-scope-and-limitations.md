---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 09/28/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 26a5537496d9e881ece135437c403baf4a4fd67c
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96016650"
---
Şimdilik, Ultra diskler ek sınırlamalara sahiptir ve bunlar şu şekildedir:

Şu anda Ultra diskler için kullanılabilen tek altyapı artıklığı seçenekleri kullanılabilirlik bölgeleri ' dir. Diğer artıklık seçeneklerini kullanan VM 'Ler bir ultra disk iliştiremezsiniz.

Aşağıdaki tabloda, Ultra disklerin ' de kullanılabildiği bölgeler ve bunlara karşılık gelen kullanılabilirlik seçenekleri de özetlenmektedir:

> [!NOTE]
> Aşağıdaki listede yer alan bir bölgede Ultra disk özellikli kullanılabilirlik alanları yoksa, bir ultra disk iliştirmek için bu bölgedeki VM 'Ler herhangi bir altyapı artıklık seçeneği olmadan dağıtılmalıdır.

|Bölgeler  |Artıklık seçenekleri  |
|---------|---------|
|Brezilya Güney     |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|Orta Hindistan     |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|Doğu Asya     |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|Almanya Orta Batı     |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|Güney Kore - Orta     |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|Orta Güney ABD    |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|US Gov Arizona     |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|US Gov Virginia     |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|US Gov Texas     |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|Batı ABD     |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)        |
|Orta Avustralya    |Yalnızca tek VM 'Ler (kullanılabilirlik kümeleri ve sanal makine ölçek kümeleri desteklenmez)|
|Doğu Avustralya     |Üç kullanılabilirlik bölgesi         |
|Güneydoğu Asya    |Üç kullanılabilirlik bölgesi        |
|Kanada Orta *     |Üç kullanılabilirlik bölgesi          |
|Orta ABD     |Üç kullanılabilirlik bölgesi          |
|Doğu ABD     |Üç kullanılabilirlik bölgesi          |
|Doğu ABD 2     |Üç kullanılabilirlik bölgesi         |
|Orta Fransa    |İki kullanılabilirlik bölgesi        |
|Doğu Japonya    |Üç kullanılabilirlik bölgesi        |
|Kuzey Avrupa    |Üç kullanılabilirlik bölgesi        |
|Güney Birleşik Krallık    |Üç kullanılabilirlik bölgesi        |
|West Europe    | Üç kullanılabilirlik bölgesi|
|Batı ABD 2    |Üç kullanılabilirlik bölgesi|

\* Bu bölgeye yönelik Kullanılabilirlik Alanları erişim sağlamak için Azure desteğine başvurun.

- Yalnızca şu VM serisinde desteklenir:
    - [ESv3](../articles/virtual-machines/ev3-esv3-series.md#esv3-series)
    - [Easv4](../articles/virtual-machines/eav4-easv4-series.md#easv4-series)
    - [Edsv4](../articles/virtual-machines/edv4-edsv4-series.md#edsv4-series)
    - [Esv4](../articles/virtual-machines/ev4-esv4-series.md#esv4-series)
    - [DSv3](../articles/virtual-machines/dv3-dsv3-series.md#dsv3-series)
    - [Dasv4](../articles/virtual-machines/dav4-dasv4-series.md#dasv4-series)
    - [Ddsv4](../articles/virtual-machines/ddv4-ddsv4-series.md#ddsv4-series)
    - [Dsv4](../articles/virtual-machines/dv4-dsv4-series.md#dsv4-series)
    - [FSv2](../articles/virtual-machines/fsv2-series.md)
    - [LSv2](../articles/virtual-machines/lsv2-series.md)
    - [M](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
    - [Mv2](../articles/virtual-machines/workloads/sap/hana-vm-operations-storage.md)
- Her VM boyutu, Ultra disklerle desteklenen her bölgede kullanılabilir değildir.
- Yalnızca veri diskleri olarak kullanılabilir. 
- Varsayılan olarak 4k fiziksel sektör boyutunu destekler. 512E sektör boyutu genel kullanıma açık bir teklif olarak kullanılabilir (kaydolma gerekmez), ancak şu anda yalnızca CLı veya PowerShell kullanılarak kullanılabilir. Çoğu uygulama 4k kesim boyutlarına sahiptir ancak bazıları 512 bayt kesim boyutu gerektirir. Bir örnek Oracle Database, 4k yerel disklerini desteklemek için Release 12,2 veya üzeri bir sürümü gerektirir. Daha eski Oracle DB sürümleri için 512 baytlık sektör boyutu gereklidir.
- Yalnızca boş disk olarak oluşturulabilir.
- Şu anda disk anlık görüntülerini, VM görüntülerini, kullanılabilirlik kümelerini, Azure adanmış Konakları veya Azure disk şifrelemesini desteklememektedir.
- Şu anda Azure Backup veya Azure Site Recovery tümleştirmeyi desteklemez.
- Yalnızca önbelleğe alınmamış okuma ve önbelleğe alınmamış yazmaları destekler.
- GA VM 'lerde ıOPS için geçerli en büyük sınır 80.000 ' dir.

Azure Ultra diskler, varsayılan olarak her abonelik için her bölge için en fazla 16 TiB sunar, ancak Ultra diskler isteğe göre daha yüksek kapasiteyi destekler. Kapasiteden artış istemek için Azure desteğine başvurun.
