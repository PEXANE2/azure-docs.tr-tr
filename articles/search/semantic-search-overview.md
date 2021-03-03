---
title: Anlamsal arama
titleSuffix: Azure Cognitive Search
description: Bilişsel Arama, arama sonuçlarının daha sezgisel olmasını sağlamak için Bing 'den derin öğrenme anlam arama modellerini nasıl kullandığını öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/02/2021
ms.custom: references_regions
ms.openlocfilehash: eedb3dfeafbd378cfff7afb04fcc2b9aa6e791d6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2021
ms.locfileid: "101680350"
---
# <a name="semantic-search-in-azure-cognitive-search"></a>Azure Bilişsel Arama 'de anlamsal arama

> [!IMPORTANT]
> Anlamsal arama özellikleri, yalnızca önizleme REST API aracılığıyla kullanılabilen genel önizlemededir. Önizleme özellikleri, olduğu gibi, [ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)altında sunulur.

Anlamsal arama, daha yüksek kaliteli, daha doğal bir sorgu deneyimini destekleyen, sorguyla ilgili özelliklerin bir koleksiyonudur. Özellikler, arama sonuçlarının anlam yeniden kullanımını, açıklamalı alt yazılar ve yanıt oluşturmayı anlam vurgulamada içerir. [Tam metin arama altyapısından](search-lucene-query-architecture.md) döndürülen ilk 50 sonuç, en ilgili eşleşmeleri bulmak için yeniden yapılır.

Temel teknoloji, Bing ve Microsoft Research yatırımlarından yararlanır ve Bilişsel Arama altyapısıyla tümleşiktir. Bunu kullanmak için, arama isteğinde küçük değişiklikler yapmanız gerekir, ancak ek yapılandırma veya yeniden dizin oluşturma gerekli değildir.

Genel Önizleme özellikleri şunları içerir:

+ Arama sorgu koşullarının bağlam veya anlam anlamı temelinde ilgili belgeleri alan anlam derecelendirme algoritması
+ İlgili passges vurgulayan anlam başlıkları
+ Sorgunun anlam yanıtları, ayrıca sonuçlardan formül
+ Sorgu terimleri arama altyapısına ulaşmadan önce yazım hatalarını düzelten yazım denetimi

## <a name="semantic-search-architecture"></a>Anlamsal arama mimarisi

Anlamsal aramanın bileşenleri, mevcut sorgu yürütme işlem hattının üzerine katmanlıdır. Yazım denetimi (diyagramda gösterilmez), tek sorgu koşullarında yazım hatalarını düzelterek geri çekmeyi geliştirir. Tüm ayrıştırma ve analiz işlemi tamamlandıktan sonra, arama motoru, sorguyla eşleşen belgeleri alır ve hizmetin ne zaman oluşturulduğuna bağlı olarak, BM25 veya klasik olan [varsayılan Puanlama algoritmasını](index-similarity-and-scoring.md#similarity-ranking-algorithms)kullanarak bunları puan alır. Puanlama profilleri de bu aşamada uygulanır. 

İlk 50 eşleşme almış olan [anlam derecelendirmesi algoritması](semantic-how-to-query-response.md) , belge Corpus ' i yeniden değerlendirir. Sonuçlar 50 'den fazla eşleşme içerebilir, ancak yalnızca ilk 50 tekrar tekrar sunulacaktır. Derecelendirme için, algoritma hem makine öğrenimi hem de aktarım öğrenimini kullanarak her birinin sorgu amacına ne kadar iyi eşleştiğini temel alır.

Açıklamalı alt yazılar ve yanıtlar oluşturmak için dil temsili modellerini kullanır. Bir başlık içinde, algoritma, sorgu sonuçlarının en iyi şekilde özetlendiği vurguları içeren açıklamalı alt yazılar çıkarmak için Bing tarafından geliştirilen bir dil modeli kullanır. Arama sorgusu bir soru ise ve yanıtlar isteniyorsa, benzer bir dil modeli, arama sorgusu tarafından ifade edilen soruyu en iyi şekilde yanıtlayan metin yollarını tanımlar.

:::image type="content" source="media/semantic-search-overview/semantic-query-architecture.png" alt-text="Sorgu ardışık düzeninde anlam bileşenleri" border="true":::

## <a name="availability-and-pricing"></a>Kullanılabilirlik ve fiyatlandırma

Bir Standart katmanda (S1, S2, S3) oluşturulan ve şu bölgelerden birinde bulunan Arama hizmetlerinde (S1, S2, S3), bir [kaydolma kaydı](https://aka.ms/SemanticSearchPreviewSignup)aracılığıyla anlamsal sıralama kullanılabilir: Orta Kuzey ABD, Batı ABD, Batı ABD 2, Doğu ABD 2, Kuzey Avrupa, Batı Avrupa. Yazım denetimi aynı bölgelerde mevcuttur, ancak katman kısıtlamalarına sahip değildir. Katman ve bölge ölçütlerine uyan mevcut bir hizmetiniz varsa, yalnızca kaydolma gereklidir.

2 Mart 'ta önizleme başlatma arasında 1 Nisan 'da, yazım düzeltme ve anlam derecelendirmesi ücretsiz olarak sunulur. 1 Nisan 'dan sonra, bu işlevi çalıştırmanın hesaplama maliyetleri faturalandırılabilir bir olay olur. Beklenen maliyet 250.000 sorguları için yaklaşık USD $500/ay olur. [Bilişsel arama fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/) belgelenen ayrıntılı maliyet bilgilerini ve [tahmin etme ve maliyetleri yönetme](search-sku-manage-costs.md)' de bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir sorgu türü, anlamsal aramanın ilgi derecesini ve yanıt yapılarını mümkün bir şekilde sunar. Başlamak için [bir anlam sorgusu oluşturun](semantic-how-to-query-request.md) . Veya ilgili bilgiler için aşağıdaki makalelerden birini gözden geçirin.

+ [Sorgu koşullarına yazım denetimi Ekle](speller-how-to-add.md)
+ [Anlamsal derecelendirme ve yanıtlar](semantic-how-to-query-response.md)