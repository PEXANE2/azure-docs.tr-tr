---
title: Azure NetApp Files için performans konuları | Microsoft Docs
description: Azure NetApp Files için performans konularını açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/25/2019
ms.author: b-juche
ms.openlocfilehash: 97e3c6212edd2ade4eabb96db3543e9b3b68e2ae
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "67454145"
---
# <a name="performance-considerations-for-azure-netapp-files"></a>Azure NetApp Files için performansla ilgili önemli noktalar

Bir birim için [Aktarım hızı sınırı](azure-netapp-files-service-levels.md) , birime atanan kotanın ve seçilen hizmet düzeyinin bir birleşimiyle belirlenir. Azure NetApp Files hakkında performans planları yaparken, bazı noktaları anlamanız gerekir. 

## <a name="quota-and-throughput"></a>Kota ve verimlilik  

Aktarım hızı sınırı, gerçekleşen gerçek performansın yalnızca bir determinantı.  

Okuma ve yazma karışımı, aktarım boyutu, rastgele veya sıralı desenler ve diğer birçok etken dahil olmak üzere tipik depolama performansı konuları, teslim edilen toplam performansa katkıda bulunur.  

Sınamada gözlemlendiği maksimum empırik üretilen iş hacmi 4.500 MiB/sn 'dir.  Premium depolama katmanında 70,31 TiB 'nin bir birim kotası, bu performans düzeyine ulaşmak için yeterince yüksek bir aktarım hızı sınırı sağlar.  

Birim kotası miktarlarını 70,31 TiB 'den daha fazla atamayı düşünüyorsanız ek verileri depolamak için bir birime ek kota atanabilir. Ancak, eklenen kota, gerçek aktarım hızına göre daha fazla artış elde etmez.  

Daha fazla bilgi için bkz. [Azure NetApp Files performans değerlendirmeleri](azure-netapp-files-performance-benchmarks.md) .

## <a name="overprovisioning-the-volume-quota"></a>Birim kotasının aşırı sağlanması

İş yükünün performansı verimlilik sınırına bağlıysa, daha yüksek bir verimlilik düzeyi ayarlamak ve daha yüksek performans elde etmek için birim kotasını fazla sağlamak mümkündür.  

Örneğin, Premium Depolama katmanındaki bir birimde yalnızca 500 GiB veri varsa ancak 128 MiB/sn olması gerekiyorsa, üretilen iş düzeyinin uygun şekilde ayarlanması için kotayı 2 TiB olarak ayarlayabilirsiniz 64 (her TB * 2 TiB = 128 MIB/s).  

Daha yüksek bir verimlilik elde etmek için bir birimi sürekli olarak fazla temin ediyorsanız, bunun yerine daha yüksek bir hizmet düzeyi kullanmayı düşünün.  Yukarıdaki örnekte, bunun yerine Ultra Storage katmanını kullanarak, birim kotasının yarısı ile aynı aktarım hızı sınırını elde edebilirsiniz 128 (örneğin, TiB * 1 TiB = 128 MIB/s).

## <a name="dynamically-increasing-or-decreasing-volume-quota"></a>Birim kotasını dinamik olarak artırma veya azaltma

Performans gereksinimleriniz geçici olarak geçicidir veya sabit bir süre için performans gereksinimlerini artırdıysanız, anında için birim kotasını dinamik olarak artırabilir veya azaltabilirsiniz.  Aşağıdaki noktalara dikkat edin: 

* Birim kotası, GÇ 'yi duraklatmaya gerek kalmadan artırılabilir veya azaltılabilir, ancak birime erişim kesintiye uğramaz veya etkilenmez.  

    Bir birime yönelik etkin bir g/ç işlemi sırasında kotayı ayarlayabilirsiniz.  Birim kotasının birimde depolanan mantıksal veri miktarının hiçbir şekilde azalmadığını unutmayın.

* Birim kotası değiştirildiğinde, karşılık gelen Aktarım hızı sınırı neredeyse anında gerçekleşir. 

    Değişiklik, birim erişimi veya g/ç 'yi kesintiye uğratmaz veya etkilemez.  

* Birim kotasının ayarlanması, kapasite havuzu boyutunda bir değişiklik yapılmasını gerektirir.  

    Kapasite havuzu boyutu dinamik olarak ve birim kullanılabilirliği veya g/ç 'yi etkilemeden ayarlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
- [Azure NetApp Files için performans kıyaslamaları](azure-netapp-files-performance-benchmarks.md)