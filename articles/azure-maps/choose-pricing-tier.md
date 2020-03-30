---
title: Doğru fiyatlandırma katmanını seçin | Microsoft Azure Haritaları
description: Bu makalede, Microsoft Azure Haritalar tarafından sunulan fiyatlandırma katmanları hakkında bilgi edineceksiniz.
author: philmea
ms.author: philmea
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: a8bf7ff9bacd4fe84ee5b64d0aed5cb271ce06f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335669"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Azure Haritalar'da doğru fiyatlandırma katmanını seçin

Azure Haritalar, S0 ve S1 olmak üzere iki fiyatlandırma katmanı sunar. Bu makalenin amacı, ihtiyaçlarınız için doğru fiyatlandırma katmanını seçmenize yardımcı olmaktır. Doğru fiyatlandırma katmanını seçmek için kendinize aşağıdaki iki soruyu sorun.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Kaç eşzamanlı kullanıcıyı desteklemeyi planlıyorum? 
S0 ve S1 fiyatlandırma katmanları farklı miktarda veri veri verisini işler. S0 fiyatlandırma katmanı saniyede en fazla **50 sorgu**işler. S1 katmanı **ise saniyede 50'den fazla sorgu**işler.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Hangi jeouzamsal yetenekleri kullanmayı planlıyorum?
Temel jeouzamsal API'ler hizmet gereksinimlerinizi karşılıyorsa, S0 fiyatlandırma katmanı sizin için uygundur. Uygulamanız için daha gelişmiş özellikler istiyorsanız, S1 fiyatlandırma katmanını tercih etmeyi düşünün. Gelişmiş yetenekler şunlardır: Havadan artı hibrit görüntüler, rota aralığı alma ve toplu coğrafi kodlama. Uygulamanız için en uygun fiyatlandırma katmanını seçmek için **fiyatlandırma katmanı özellikleri** tablosunu gözden geçirin.

### <a name="pricing-tier-capabilities"></a>Fiyatlandırma katmanı yetenekleri

| Özellik                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Harita Oluşturma                              | ✓                   | ✓       |
| Uydu Görüntüleri                       |                     | ✓        |
| Search                                  | ✓                    | ✓        |
| Toplu Arama                            |                     | ✓        |
| Yol                                   | ✓                    |✓        |
| Toplu Yönlendirme                            |                    | ✓        |
| Matris Yönlendirme                          |                     | ✓        |
| Rota Aralığı (Isochrones)                |                     | ✓        |
| Trafik                                |✓                    |✓        |
| Saat Dilimi                               |✓                    |✓        |
| Coğrafi Konum (Önizleme)                    |✓                   |✓        |
| Mekansal Operasyonlar                        |                    |✓        |
| Geofencing                                |                    |✓        |
| Azure Haritalar Verileri (Önizleme)                |                     | ✓        |
| Mobilite (Önizleme)                       |                     | ✓        |
| Hava Durumu (Önizleme)                        |✓                    |✓        |

Bu ek noktaları göz önünde bulundurun:
* Ne tür bir girişiminvar?
* Başvurunuz ne kadar önemli?

### <a name="pricing-tier-targeted-customers"></a>Fiyatlandırma katmanı hedefli müşteriler

S0 ve S1 fiyatlandırma katmanlarını daha iyi anlamak için **fiyatlandırma katmanı hedefli müşteriler** tablosuna bakın. Daha fazla bilgi için Azure [Haritalar fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-maps/)'na bakın. 

| Fiyatlandırma katmanı  |     Hedeflenen müşteriler                                                                |
|-----------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>S0 fiyatlandırma katmanı, konsept kanıtı geliştirme ve erken aşama testinden uygulama üretimi ve dağıtımına kadar üretimin tüm aşamalarındaki uygulamalar için çalışır. Ancak, bu katman küçük ölçekli geliştirme veya düşük eşzamanlı kullanıcılara veya her ikisine sahip müşteriler için tasarlanmıştır. <p>|
| S1            |    <p>S1 fiyatlandırma katmanı, büyük ölçekli kurumsal uygulamaları, görev açısından kritik uygulamaları veya yüksek hacimli eşzamanlı kullanıcıları olan müşteriler içindir. Aynı zamanda gelişmiş jeouzamsal hizmetlere ihtiyaç duyan müşteriler içindir.</p>|

## <a name="next-steps"></a>Sonraki adımlar

Fiyatlandırma katmanlarını görüntüleme ve değiştirme hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"] 
> [Fiyatlandırma katmanını yönetme](how-to-manage-pricing-tier.md)
