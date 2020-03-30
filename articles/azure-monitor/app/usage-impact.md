---
title: Azure Uygulama Öngörüleri Kullanım Etkisi | Microsoft dokümanlar
description: Farklı özelliklerin uygulamalarınızın bazı bölümleri için dönüşüm oranlarını nasıl etkilenebildiğini analiz edin.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 01/08/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 787221c4df3f06029d19ee779a28bb763723f27d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671044"
---
# <a name="impact-analysis-with-application-insights"></a>Uygulama Öngörüleri ile etki analizi

Etki, yükleme sürelerinin ve diğer özelliklerin uygulamanızın çeşitli bölümleri için dönüşüm oranlarını nasıl etkilediğini analiz eder. Daha kesin olarak ifade etmek gerekirse, sayfa **görünümü,** **özel olay**veya **istek** herhangi bir **boyutu** farklı bir **sayfa görünümü** veya **özel olay**kullanımını nasıl etkilediğini keşfeder. 

![Darbe aracı](./media/usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>Impact'in ne yaptığından hala emin değil misiniz?

Impact'i düşünmenin bir yolu, sitenizin bazı yönlerindeki yavaşlığın kullanıcıların etrafta kalıp kalmamasını nasıl etkilediği ne kadar yavaşolduğu yla ilgili ekibinizdeki biriyle tartışmaları halletmek için nihai bir araçtır. Kullanıcılar belirli bir miktar yavaşlığa tolerans gösterse de Impact, kullanıcı dönüşümlerini en üst düzeye çıkarmak için optimizasyon ve performansı en iyi nasıl dengelediğinize dair size fikir verir.

Ama performansı analiz etmek Impact'in yeteneklerinin sadece bir alt kümesidir. Impact özel olayları ve boyutları desteklediği için, kullanıcı tarayıcıseçiminin farklı dönüşüm oranlarıyla nasıl ilişkili olduğu gibi soruları yanıtlamak yalnızca birkaç tıklama uzakta.

![Tarayıcılar tarafından ekran görüntüsü dönüştürme](./media/usage-impact/0004-browsers.png)

> [!NOTE]
> Uygulama Öngörüleri kaynağınız, Etki aracını kullanmak için sayfa görünümleri veya özel olaylar içermelidir. [Application Insights JavaScript SDK ile sayfa görünümlerini otomatik olarak toplamak için uygulamanızı nasıl ayarlayabilirsiniz](../../azure-monitor/app/javascript.md)öğrenin. Ayrıca, korelasyon analiz beri, örnek boyutu önemli olduğunu unutmayın.
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>Sayfa yükleme süresi sayfamda kaç kişinin dönüşüm emlenmesini etkiliyor mu?

Impact aracıyla soruları yanıtlamaya başlamak için bir başlangıç sayfası görünümü, özel etkinlik veya istek seçin.

![Darbe aracı](./media/usage-impact/0002-dropdown.png)

1. **Sayfa görünümü** açılır için bir sayfa görünümü seçin.
2. **Varsayılan Süre** seçiminde **nasıl** açılır bırak (Bu bağlamda **Süre,** Sayfa **Yük Süresi**için bir diğer addır .)
3. Açılır bırakma **kullanımının etkileri** için özel bir olay seçin. Bu olay, adım 1'de seçtiğiniz sayfa görünümündeki bir Web-Servis Elemanı öğesine karşılık gelir.

![Sonuçların ekran görüntüsü](./media/usage-impact/0003-results.png)

Bu durumda **Ürün Sayfası** yükleme süresi arttıkça **tıklanan Ürün Satın Alma'ya** dönüşüm oranı azalır. Yukarıdaki dağıtıma bağlı olarak, olası %55'lik bir dönüşüm oranına ulaşmak için 3,5 saniyelik en uygun sayfa yükleme süresi hedeflenebilir. Yükleme süresini 3,5 saniyenin altına düşürmek için yapılan diğer performans iyileştirmeleri şu anda ek dönüşüm avantajlarıyla ilişkili değildir.

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>Sayfa görüntülemelerini veya yükleme sürelerini özel yollarla takip edersem ne olur?

Darbe hem standart hem de özel özellikleri ve ölçüleri destekler. Ne istersen kullan. Süre yerine, daha spesifik olmak için birincil ve ikincil olaylardaki filtreleri kullanın.

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>Farklı ülkelerden veya bölgelerden kullanıcılar farklı oranlarda dönüşüm yapar mı?

1. **Sayfa görünümü** açılır için bir sayfa görünümü seçin.
2. Açılır **düşüşünü analiz** etmek için "Ülke veya bölge"yi seçin
3. Açılır bırakma **kullanımının etkileri** için, adım 1'de seçtiğiniz sayfa görünümünde bir Kullanıcı Arabirimi öğesine karşılık gelen özel bir olay seçin.

Bu durumda, sonuçlar artık ilk örnekte olduğu gibi sürekli bir x ekseni modeline sığmaz. Bunun yerine, parçalı bir hunibenzer bir görselleştirme sunulur. Ülkeye/bölgeye göre özel etkinliğinize dönüşüm varyasyonunu görüntülemek için **Kullanıma** göre sıralayın.


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>Impact aracı bu dönüşüm oranlarını nasıl hesaplar?

Kaputun altında, Etki aracı [Pearson korelasyon katsayısına](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient)dayanır. Sonuçlar -1 ile 1 arasında ,1 negatif doğrusal korelasyon ve 1 pozitif doğrusal korelasyon temsil eden hesaplanır.

Etki Analizi'nin nasıl çalıştığının temel dökümü aşağıdaki gibidir:

A _A_ = ilk açılır sayfada seçtiğiniz ana sayfa görünümü/özel olay/istek olsun. (**Sayfa görünümü için**).

Let _B_ = seçtiğiniz ikincil sayfa görünümü/özel olay (**kullanımını etkiler).**

Impact, seçilen zaman aralığındaki kullanıcılardan gelen tüm oturumların bir örneğine bakar. Her oturum için, _A'nın_her oluşumunu arar.

Oturumlar daha sonra iki koşuldan birine göre iki farklı alt _oturum_ türüne ayrılır:

- Dönüştürülmüş bir alt _oturum, B_ olayıyla biten bir oturumdan oluşur ve _B'den_önce gerçekleşen tüm _A_ olaylarını kapsar.
- Tüm _A'lar_ _B_terminali olmadan gerçekleştiğinde dönüştürülmemiş bir alt oturum oluşur.

Etki'nin nasıl hesaplandığı, ölçüme veya boyuta göre analiz edip etmediğime bağlı olarak değişir. Ölçümler için bir alt oturumdaki tüm _A'lar_ortalamaolarak alınır. Boyutlar için her _A'nın_ _değeri, B'ye_ atanan değere _1/N_ katkıda bulunurken, _N_ alt oturumdaki _A'nın_sayısıdır.

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım deneyimlerini etkinleştirmek için [özel etkinlikler](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views)göndermeye başlayın.
- Zaten özel etkinlikler veya sayfa görünümleri gönderiyorsanız, kullanıcıların hizmetinizi nasıl kullandığını öğrenmek için Kullanım araçlarını keşfedin.
    - [Huniler](usage-funnels.md)
    - [Bekletme](usage-retention.md)
    - [Kullanıcı Akışları](usage-flows.md)
    - [Çalışma Kitapları](../../azure-monitor/app/usage-workbooks.md)
    - [Kullanıcı bağlamı ekleme](usage-send-user-context.md)
