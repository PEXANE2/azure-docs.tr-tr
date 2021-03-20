---
title: İlk sunucu grubu boyutu-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Kullanım durumu için doğru başlangıç boyutunu seçin
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: how-to
ms.date: 11/17/2020
ms.openlocfilehash: 7e68e9f8caad7d7e4bc44bc4e1e55150a78b4a98
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "95026412"
---
# <a name="pick-initial-size-for-hyperscale-citus-server-group"></a>Hiperscale (Citus) sunucu grubu için başlangıç boyutunu seç

Bir sunucu grubunun boyutu, hem düğüm sayısı hem de donanım kapasitesi [kolayca değiştirilebilir](howto-hyperscale-scale-grow.md)). Ancak yine de yeni bir sunucu grubu için bir başlangıç boyutu seçmeniz gerekir. Makul bir seçenek için bazı ipuçları aşağıda verilmiştir.

## <a name="multi-tenant-saas-use-case"></a>Çok kiracılı SaaS kullanımı-örnek

Mevcut bir tek düğümlü PostgreSQL veritabanı örneğinden hiper ölçeğe geçiş yaparken, toplam çalışan sanal çekirdekleri ve RAM sayısının orijinal örneğe eşit olduğu bir küme seçin. Bu tür senaryolarda, parça kaynak kullanımını iyileştirtiğinden ve daha küçük bir dizin kullanılmasına izin verirken 2 3x performans iyileştirmeleri gördük.

Sanal çekirdek sayısı aslında tek karardır. RAM ayırma şu anda, [hiper ölçek (Citus) yapılandırma seçenekleri](concepts-hyperscale-configuration-options.md) sayfasında açıklandığı gibi, vCore sayısına göre belirlenir.
Düzenleyici düğümü, çalışan olarak çok RAM gerektirmez, ancak RAM ve sanal çekirdekleri bağımsız olarak seçme yolu yoktur.

## <a name="real-time-analytics-use-case"></a>Gerçek zamanlı analiz kullanımı-örnek

Toplam Vçekirdekler: RAM 'e uygun veri miktarı kullanıldığında, hiper ölçekte (Citus) bir doğrusal performans geliştirmesini, çalışan çekirdekleri sayısıyla orantılı olarak bekleyebilir. Gereksinimlerinize göre doğru sanal çekirdek sayısını öğrenmek için, tek düğümlü veritabanınızdaki sorgular için geçerli gecikme süresini ve hiper ölçekte gereken gecikme süresini göz önünde bulundurun (Citus). Geçerli gecikme süresini istenen gecikme süresine bölün ve sonucu yuvarlayın.

Çalışan RAM'i: En iyi seçenek, çalışan kümesinin çoğunun belleğe sığabileceği kadar çok bellek sağlamak olacaktır. Uygulamanızın kullandığı sorgu türleri bellek gereksinimlerini etkiler. Sorgu üzerinde analizine ilişkin AÇıKLA ' yı çalıştırarak, gereken bellek miktarını belirleyebilirsiniz. Sanal çekirdekler ve RAM 'in, [Hyperscale (Citus) yapılandırma seçenekleri](concepts-hyperscale-configuration-options.md) makalesinde açıklandığı şekilde birlikte ölçeklendirilmesini unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Sunucu grubu ölçeklendirme](howto-hyperscale-scale-grow.md)
- Sunucu grubu [performans seçenekleri](concepts-hyperscale-configuration-options.md)hakkında daha fazla bilgi edinin.
