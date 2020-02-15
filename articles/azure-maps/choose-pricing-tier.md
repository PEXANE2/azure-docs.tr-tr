---
title: Doğru fiyatlandırma katmanını seçin | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları tarafından sunulan fiyatlandırma katmanları hakkında bilgi edineceksiniz.
author: farah-alyasari
ms.author: v-faalya
ms.date: 01/15/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 048ecdc9b96edcdebecb4cd7df0630aa991aa4d1
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210081"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Azure haritalar 'da doğru fiyatlandırma katmanını seçin

Azure haritalar, S0 ve S1 olmak üzere iki fiyatlandırma katmanı sunar. Bu makalenin amacı, gereksinimleriniz için doğru fiyatlandırma katmanını seçmenize yardımcı olmak içindir. Doğru fiyatlandırma katmanını seçmek için kendinize aşağıdaki iki soruyu sorun.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Kaç tane eşzamanlı kullanıcı desteklemeyi planlıyorum? 
S0 ve S1 fiyatlandırma katmanları farklı miktarda veri aktarımını işler. S0 fiyatlandırma katmanı, **saniyede 50 sorgunun sayısını**işler. S1 katmanı **saniyede 50 ' den fazla sorgu**işler.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Hangi jeo kullanım özelliklerini kullanmayı planlıyorum?
Çekirdek Jeo-uzamsal API 'Leri hizmet gereksinimlerinizi karşılıyorsa, S0 fiyatlandırma katmanı sizin için doğru olur. Uygulamanız için daha gelişmiş yetenekler istiyorsanız S1 fiyatlandırma katmanını tercih edin. Gelişmiş yetenekler şunlardır: havadan, hibrit Plus hibriy, yol aralığı alma ve toplu iş coğrafi kodlama. Uygulamanız için en uygun fiyatlandırma katmanını seçmek için **Fiyatlandırma Katmanı Özellikleri** tablosunu gözden geçirin.

### <a name="pricing-tier-capabilities"></a>Fiyatlandırma Katmanı Özellikleri

| Özellik                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Eşleme Işleme                              | ✓                   | ✓       |
| Uydu Imagery                       |                     | ✓        |
| Arama                                  | ✓                    | ✓        |
| Toplu işlem arama                            |                     | ✓        |
| Yol                                   | ✓                    |✓        |
| Batch yönlendirme                            |                    | ✓        |
| Matris yönlendirme                          |                     | ✓        |
| Yol aralığı (Izites)                |                     | ✓        |
| Trafik                                |✓                    |✓        |
| Saat Dilimi                               |✓                    |✓        |
| Coğrafi konum (Önizleme)                    |✓                   |✓        |
| Uzamsal Işlemler                        |                    |✓        |
| Bırakmanıza                                |                    |✓        |
| Azure haritalar verileri (Önizleme)                |                     | ✓        |
| Mobility (Önizleme)                       |                     | ✓        |
| Hava durumu (Önizleme)                        |✓                    |✓        |

Şu ek noktaları göz önünde bulundurun:
* Ne tür bir kuruluş kullanıyorsunuz?
* Uygulamanız ne kadar kritik?

### <a name="pricing-tier-targeted-customers"></a>Fiyatlandırma Katmanı hedeflenen müşteriler

S0 ve S1 fiyatlandırma katmanlarından daha iyi bir fikir almak için **fiyatlandırma katmanı hedeflenen müşteriler** tablosuna bakın. Daha fazla bilgi için bkz. [Azure Maps fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-maps/). 

| Fiyatlandırma katmanı  |     Hedeflenen müşteriler                                                                |
|-----------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>S0 fiyatlandırma katmanı, tüm üretim aşamalarında uygulamalar için geçerlidir: kavram kanıtı geliştirme ve uygulama üretim ve dağıtımına yönelik erken aşama testi. Ancak, bu katman küçük ölçekli geliştirme veya düşük eşzamanlı kullanıcıları olan müşteriler ya da her ikisi için tasarlanmıştır. <p>|
| S1            |    <p>S1 fiyatlandırma katmanı, büyük ölçekli kurumsal uygulamalar, görev açısından kritik uygulamalar veya yüksek miktarda eşzamanlı kullanıcı olan müşterilere yöneliktir. Bu, gelişmiş Jeo-uzamsal hizmetler gerektiren müşteriler için de gereklidir.</p>|

## <a name="next-steps"></a>Sonraki adımlar

Fiyatlandırma katmanlarını görüntüleme ve değiştirme hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"] 
> [Fiyatlandırma katmanını yönetme](how-to-manage-pricing-tier.md)
