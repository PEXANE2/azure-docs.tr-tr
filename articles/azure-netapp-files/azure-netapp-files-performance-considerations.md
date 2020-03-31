---
title: Azure NetApp Dosyaları için performans hususları | Microsoft Dokümanlar
description: Azure NetApp Dosyaları için performans hususlarını açıklar.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67454145"
---
# <a name="performance-considerations-for-azure-netapp-files"></a>Azure NetApp Files için performansla ilgili önemli noktalar

Bir birim için [iş hacmi sınırı,](azure-netapp-files-service-levels.md) birim ve seçilen hizmet düzeyine atanan kotanın bir birleşimi ile belirlenir. Azure NetApp Dosyaları hakkında performans planları yaptığınızda, çeşitli hususları anlamanız gerekir. 

## <a name="quota-and-throughput"></a>Kota ve iş artışı  

İş sonu sınırı, gerçek performansın yalnızca bir belirleyicisidir.  

Okuma ve yazma karışımı, aktarım boyutu, rasgele veya sıralı desenler ve diğer birçok etken dahil olmak üzere tipik depolama performansı konuları, teslim edilen toplam performansa katkıda bulunacaktır.  

Testte gözlenen maksimum ampirik verim 4.500 MiB/s'dir.  Premium depolama katmanında, 70,31 TiB'lik bir hacim kotası, bu performans düzeyine ulaşmak için yeterince yüksek bir iş hacmi sınırı sağlayacaktır.  

70.31 TiB'nin ötesinde birim kota tutarları atamayı düşünüyorsanız, ek veri depolamak için bir topluma ek kota atanabilir. Ancak, eklenen kota gerçek üretim daha fazla artışa neden olmaz.  

Ek bilgi [için Azure NetApp Dosyaları için Performans ölçütlerine](azure-netapp-files-performance-benchmarks.md) bakın.

## <a name="overprovisioning-the-volume-quota"></a>Birim kotasının fazla verilmesi

Bir iş yükünün performansı iş hacmi sınırı yla sınırlanırsa, daha yüksek bir iş hacmi düzeyi ayarlamak ve daha yüksek performans elde etmek için birim kotasını aşırı sağlamak mümkündür.  

Örneğin, Premium depolama katmanındaki bir birimde yalnızca 500 GiB veri varsa ancak 128 MiB/s'lik bir veri gerekiyorsa, kotayı 2 TiB olarak ayarlayabilirsiniz, böylece iş hacmi düzeyi buna göre ayarlanır (TB başına 64 MiB/s / 2 TiB = 128 MiB/s).  

Daha yüksek bir iş hacmi elde etmek için sürekli olarak fazla bir birim veriyorsanız, bunun yerine daha yüksek bir hizmet düzeyi kullanmayı düşünün.  Yukarıdaki örnekte, bunun yerine Ultra depolama katmanını kullanarak hacim kotasının yarısıyla aynı iş hacmi sınırını elde edebilirsiniz (TiB başına 128 MiB/s başına * 1 TiB = 128 MiB/s).

## <a name="dynamically-increasing-or-decreasing-volume-quota"></a>Birim kotası dinamik olarak artırılıyor veya azalıyor

Performans gereksinimleriniz doğada geçiciyse veya belirli bir süre için performans gereksinimlerini artırdıysanız, üretim sınırını anında ayarlamak için birim kotasını dinamik olarak artırabilir veya azaltabilirsiniz.  Aşağıdaki hususlara dikkat edin: 

* Birim kotası, IO'yu duraklatmaya gerek kalmadan artırılabilir veya azaltılabilir ve toplua erişim kesintiye uğramaz veya etkilenmez.  

    Etkin bir G/Ç işlemi sırasında kotayı bir birim olarak ayarlayabilirsiniz.  Birim kotasının, birimde depolanan mantıksal veri miktarının altına düşürülmeyeceğini unutmayın.

* Birim kotası değiştirildiğinde, ilgili iş hacmi limitindeki değişiklik neredeyse anında gerçekleşir. 

    Değişiklik, birim erişimini veya G/Ç'yi kesintiye uğratmaz veya etkilemez.  

* Birim kotası ayarlama kapasite havuzu boyutunda bir değişiklik gerektirir.  

    Kapasite havuzu boyutu dinamik olarak ayarlanabilir ve birim kullanılabilirliğini veya G/Ç'yi etkilemeden.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
- [Azure NetApp Files için performans kıyaslamaları](azure-netapp-files-performance-benchmarks.md)