---
title: Doğru fiyatlandırma katmanını seçin | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları tarafından sunulan fiyatlandırma katmanları hakkında bilgi edineceksiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 8764e9161f952118ca7ae28343dcd16477cf1eee
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76155766"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Azure haritalar 'da doğru fiyatlandırma katmanını seçin

Azure haritalar, S0 ve S1 olmak üzere iki fiyatlandırma katmanı sunar. Bu makalenin amacı, gereksinimleriniz için doğru fiyatlandırma katmanını seçmenize yardımcı olmak içindir. Doğru fiyatlandırma katmanını seçmenize yardımcı olmak için aşağıdaki iki soruyu kendinize sorun.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Hangi jeo kullanım özelliklerini kullanmayı planlıyorum?
Çekirdek Jeo-uzamsal API 'Leri hizmet gereksinimlerinizi karşılıyorsa, S0 fiyatlandırma katmanı sizin için doğru olur. Uygulamanız için daha gelişmiş yetenekler istiyorsanız S1 fiyatlandırma katmanını tercih edin. Gelişmiş yetenekler örneği: havadan, hibrit ve karma imagery, yol aralığı alma ve toplu iş coğrafi kodlama. **Fiyatlandırma Katmanı Özellikleri** tablosu, uygulamanız için en uygun fiyatlandırma katmanını seçmenize yardımcı olabilir.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Kaç tane eşzamanlı kullanıcı desteklemeyi planlıyorum? 
S0 ve S1 fiyatlandırma katmanları farklı miktarda veri aktarımını işler. S0 fiyatlandırma katmanı saniyede **50 sorguyu**(S1 katmanı **saniyede 50 ' den fazla sorgu**işlerken) işler.

### <a name="pricing-tier-capabilities"></a>Fiyatlandırma Katmanı Özellikleri

| Özellik                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Eşleme Işleme                              | ✓                   | ✓       |
| Uydu Imagery                       |                     | ✓        |
| Arama                                  | ✓                    | ✓        |
| Toplu Arama                            |                     | ✓        |
| Yol                                   | ✓                    |✓        |
| Toplu Yönlendirme                            |                    | ✓        |
| Matris Yönlendirme                          |                     | ✓        |
| Rota Aralığı (İzokronlar)                |                     | ✓        |
| Trafik                                |✓                    |✓        |
| Saat Dilimi                               |✓                    |✓        |
| Coğrafi konum (Önizleme)                    |✓                   |✓        |
| Uzamsal İşlemler                        |                    |✓        |
| Coğrafi Sınırlama                                |                    |✓        |
| Azure haritalar verileri (Önizleme)                |                     | ✓        |
| Mobility (Önizleme)                       |                     | ✓        |
| Hava durumu (Önizleme)                        |✓                    |✓        |

Bu ek veri noktalarına göz önünde bulundurulmayı düşünülüyor:
* Ne tür bir kuruluş kullanıyorsunuz?
* Uygulamanız ne kadar kritik?

### <a name="pricing-tier-targeted-customers"></a>Fiyatlandırma Katmanı hedeflenen müşteriler

S0 ve S1 fiyatlandırma katmanlarından daha iyi bir fikir almak için **fiyatlandırma katmanı hedeflenen müşteriler** tablosuna bakın. Daha fazla bilgi için bkz. [Azure Maps fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-maps/). 

| Fiyatlandırma katmanı  |     Hedeflenen müşteriler                                                                |
|-----------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>S0 fiyatlandırma katmanı, tüm üretim aşamalarında uygulamalar için geçerlidir: kavram kanıtı geliştirme ve uygulama üretim ve dağıtımına yönelik erken aşama testi. Ancak, bu katman küçük ölçekli geliştirme veya düşük eşzamanlı kullanıcıları olan müşteriler ya da her ikisi için tasarlanmıştır. <p>|
| S1            |    <p>S1 fiyatlandırma katmanı, büyük ölçekli kurumsal, görev açısından kritik uygulamalar veya çok sayıda eşzamanlı kullanıcı için destek gerektiren müşterilere yöneliktir. Bu, gelişmiş Jeo-uzamsal hizmetler gerektiren müşteriler için de gereklidir.</p>|

## <a name="next-steps"></a>Sonraki adımlar

Fiyatlandırma katmanlarını görüntüleme ve değiştirme hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"] 
> [Fiyatlandırma katmanını yönetme](how-to-manage-pricing-tier.md)
