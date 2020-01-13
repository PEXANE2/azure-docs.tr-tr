---
title: Doğru fiyatlandırma katmanını seçin | Microsoft Azure haritaları
description: Bu makalede, Microsoft Azure haritaları tarafından sunulan fiyatlandırma katmanları hakkında bilgi edineceksiniz.
author: walsehgal
ms.author: v-musehg
ms.date: 01/02/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 5ff774f9848db948058075a98504e6c13db5467a
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911759"
---
# <a name="choose-the-right-pricing-tier-in-azure-maps"></a>Azure haritalar 'da doğru fiyatlandırma katmanını seçin

Azure Haritalar, iki fiyatlandırma katmanı sunuyor. Bu makalenin amacı, gereksinimleriniz için doğru fiyatlandırma katmanını seçmenize yardımcı olmak içindir. Doğru fiyatlandırma katmanını seçmenize yardımcı olmak için aşağıdaki iki soruyu kendinize sorun.

## <a name="what-geospatial-capabilities-do-i-plan-to-use"></a>Hangi jeo kullanım özelliklerini kullanmayı planlıyorum?
Çekirdek Jeo-uzamsal API 'Leri hizmet gereksinimlerinizi karşılıyorsa, S0 fiyatlandırma katmanı sizin için doğru olur. Uygulamanız için daha gelişmiş yetenekler istiyorsanız S1 fiyatlandırma katmanını tercih edin. Örnek yetenekler, Areal Plus hibrit, yönlendirme aralığı alma ve toplu iş coğrafi kodlama olanakları vardır. Aşağıdaki **Fiyatlandırma Katmanı Özellikleri** tablosu, uygulamanızın ihtiyaçlarına daha iyi fikir verir. Ayrıca, uygulamanız için en uygun fiyatlandırma katmanını seçmenize de yardımcı olur.

## <a name="how-many-concurrent-users-do-i-plan-to-support"></a>Kaç tane eşzamanlı kullanıcı desteklemeyi planlıyorum? 
S0 ve S1 fiyatlandırma katmanları farklı miktarda veri aktarımını işler. Azure haritalar fiyatlandırma katmanını seçmeden önce bazı sorularınızı sorun. Örnek olarak "kaç tane eşzamanlı kullanıcı desteklemek istiyorum?" S0 fiyatlandırma katmanı, **saniyede 50 sorgunun sayısını**işler. S1 fiyatlandırma katmanı, **saniye başına 50 ' den fazla sorgu**işler.

### <a name="pricing-tier-capabilities"></a>Fiyatlandırma Katmanı Özellikleri

| Özellik                              |        S0           |  S1      |
|-----------------------------------------|:-------------------:|:--------:|
| Ara (FWD/Rev coğrafi kodlama, ilgi noktaları)  |        ✓           |     ✓    |
| Toplu iş coğrafi kodlama (Önizleme)              |                   |     ✓    |
| Aramadan çokgenler          |                   |     ✓    |
| Yönlendirme                                 |        ✓           |     ✓    |
| Yol aralığı                    |                   |     ✓    |
| Batch yönlendirme (Önizleme)                |                   |     ✓    |
| Matris yönlendirme (Önizleme)               |                   |     ✓    |
| İşleme                                  |        ✓           |     ✓    |
| Imagery Plus karma canlandırın    |            |     ✓    |
| Trafik                                 |        ✓           |     ✓    |
| Saat dilimleri                              |        ✓           |     ✓    |
| Coğrafi konum (Önizleme)                |        ✓           |     ✓    |
| Veri (Önizleme)               |                   |     ✓    |
| Uzamsal (Önizleme)               |                   |     ✓    |
| Bölge sınırlaması (Önizleme)               |                   |     ✓    |



Bu ek veri noktalarına göz önünde bulundurulmayı düşünülüyor:
* Ne tür bir kuruluş kullanıyorsunuz?
* Uygulama ne kadar kritik oluşturuluyor?

S0 ve S1 fiyatlandırma katmanlarından daha iyi bir fikir almak için **fiyatlandırma katmanı hedeflenen müşteriler** tablosuna bakın. Daha fazla bilgi için bkz. [Azure Maps fiyatlandırması](https://azure.microsoft.com/pricing/details/azure-maps/). 

### <a name="pricing-tier-targeted-customers"></a>Fiyatlandırma Katmanı hedeflenen müşteriler

| Fiyatlandırma katmanı  |     Hedeflenen müşteriler                                                                |
|---------------|:-----------------------------------------------------------------------------------------|
| S0            |    <p>S0 fiyatlandırma katmanı, küçük ya da orta ölçekli kuruluşlar olan müşteriler içindir. Yüksek miktarda eşzamanlı kullanıcı beklenmezseniz, sizin için doğru fiyatlandırma katmanınız. Yukarıdaki tabloda gösterilen temel Jeo-uzamsal API 'Lerin hizmet gereksinimlerinizi karşılaması durumunda da bu da doğru olur. Bu katman genel kullanıma sunulmuştur. Üretim aşamasındaki tüm aşamalardaki uygulamalar için geçerlidir: kavram kanıtı geliştirme ve uygulama üretim ve dağıtımına yönelik erken aşama testi.<p>|
| S1            |    <p>S1 fiyatlandırma katmanı, büyük ölçekli kurumsal, görev açısından kritik uygulamalar veya çok sayıda eşzamanlı kullanıcı için destek gerektiren müşterilere yöneliktir. Bu, gelişmiş Jeo-uzamsal hizmetler gerektiren müşteriler için de gereklidir.</p>|

## <a name="next-steps"></a>Sonraki adımlar

Fiyatlandırma katmanlarını görüntüleme ve değiştirme hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"] 
> [Fiyatlandırma katmanını yönetme](how-to-manage-pricing-tier.md)
