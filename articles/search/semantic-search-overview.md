---
title: Anlamsal arama
titleSuffix: Azure Cognitive Search
description: Bilişsel Arama, arama sonuçlarının daha sezgisel olmasını sağlamak için Bing 'den derin öğrenme anlam arama modellerini nasıl kullandığını öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/05/2021
ms.custom: references_regions
ms.openlocfilehash: 19b7f9bc19bec989e524dce7172037025e2fe4fd
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2021
ms.locfileid: "102432988"
---
# <a name="semantic-search-in-azure-cognitive-search"></a>Azure Bilişsel Arama 'de anlamsal arama

> [!IMPORTANT]
> Anlamsal arama özellikleri, yalnızca önizleme REST API aracılığıyla kullanılabilen genel önizlemededir. Önizleme özellikleri, olduğu gibi, [ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)altında sunulur.

Anlamsal arama, daha yüksek kaliteli, daha doğal bir sorgu deneyimini destekleyen, sorguyla ilgili özelliklerin bir koleksiyonudur. Özellikler, arama sonuçlarının anlam yeniden kullanımını, açıklamalı alt yazılar ve yanıt oluşturmayı anlam vurgulamada içerir. [Tam metin arama altyapısından](search-lucene-query-architecture.md) döndürülen ilk 50 sonuç, en ilgili eşleşmeleri bulmak için yeniden yapılır.

Temel alınan teknoloji Bing ve Microsoft Research ' dir ve Bilişsel Arama altyapısına tümleştirilir. Araştırmayı yedekleyen araştırma ve AI yatırımları hakkında daha fazla bilgi için bkz. [Bing 'den AI 'Nin Azure bilişsel arama (Microsoft Research blogu) nasıl güçlemesinden](https://www.microsoft.com/research/blog/the-science-behind-semantic-search-how-ai-from-bing-is-powering-azure-cognitive-search/).

Sorgularda anlam arama 'yı kullanmak için, arama isteğinde küçük değişiklikler yapmanız gerekir, ancak ek yapılandırma veya yeniden dizin oluşturma gerekli değildir.

Genel Önizleme özellikleri şunları içerir:

+ Bir ilgi Puanını hesaplamak için bağlam veya anlam anlamı kullanan anlam derecelendirme modeli
+ Kolay tarama için bir sonuçtan anahtar passges özetleyen anlam başlıkları
+ Sorgu bir soru ise sorguya anlam yanıtları
+ Anahtar tümceciklere ve terimlere odaklanarak anlamlı vurgular
+ Sorgu terimleri arama altyapısına ulaşmadan önce yazım hatalarını düzelten yazım denetimi

## <a name="availability-and-pricing"></a>Kullanılabilirlik ve fiyatlandırma

Bir Standart katmanda (S1, S2, S3) oluşturulan ve şu bölgelerden birinde bulunan Arama hizmetlerinde (S1, S2, S3), bir [kaydolma kaydı](https://aka.ms/SemanticSearchPreviewSignup)aracılığıyla anlamsal sıralama kullanılabilir: Orta Kuzey ABD, Batı ABD, Batı ABD 2, Doğu ABD 2, Kuzey Avrupa, Batı Avrupa. Yazım denetimi aynı bölgelerde mevcuttur, ancak katman kısıtlamalarına sahip değildir. Katman ve bölge ölçütlerine uyan mevcut bir hizmetiniz varsa, yalnızca kaydolma gereklidir.

2 Mart 'ta önizleme başlatma arasında 1 Nisan 'da, yazım düzeltme ve anlam derecelendirmesi ücretsiz olarak sunulur. 1 Nisan 'dan sonra, bu işlevi çalıştırmanın hesaplama maliyetleri faturalandırılabilir bir olay olur. Beklenen maliyet 250.000 sorguları için yaklaşık USD $500/ay olur. [Bilişsel arama fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/search/) belgelenen ayrıntılı maliyet bilgilerini ve [tahmin etme ve maliyetleri yönetme](search-sku-manage-costs.md)' de bulabilirsiniz.

## <a name="semantic-search-architecture"></a>Anlamsal arama mimarisi

Anlamsal aramanın bileşenleri, mevcut sorgu yürütme işlem hattının üzerine katmanlıdır. Yazım denetimi (diyagramda gösterilmez), tek sorgu koşullarında yazım hatalarını düzelterek geri çekmeyi geliştirir. Ayrıştırma ve analiz tamamlandıktan sonra, arama motoru, sorguyla eşleşen belgeleri alır ve hizmetin ne zaman oluşturulduğuna bağlı olarak BM25 veya klasik olarak [varsayılan Puanlama algoritmasını](index-similarity-and-scoring.md#similarity-ranking-algorithms)kullanarak bunları puan alır. Puanlama profilleri de bu aşamada uygulanır.

İlk 50 eşleşme almış olan [anlam derecelendirme modeli](semantic-how-to-query-response.md) , belge Corpus ' i yeniden değerlendirir. Sonuçlar 50 'den fazla eşleşme içerebilir, ancak yalnızca ilk 50 tekrar tekrar sunulacaktır. Model derecelendirmesi için hem makine öğrenimi hem de aktarım öğrenimini kullanarak, her birinin sorgu amacını ne kadar iyi bir şekilde eşleştirmesini temel alır.

Açıklamalı alt yazılar ve yanıtlar oluşturmak için anlamsal arama, bir sonucu en iyi özetleyen anahtar pasları ayıklamak ve vurgulamak için dil gösterimini kullanır. Arama sorgusu bir soru ise ve yanıtlar istenirse, yanıt, arama sorgusunda gösterildiği gibi, soruyu en iyi şekilde cevapladığı bir metin metni içerir.

:::image type="content" source="media/semantic-search-overview/semantic-query-architecture.png" alt-text="Sorgu ardışık düzeninde anlam bileşenleri" border="true":::

## <a name="next-steps"></a>Sonraki adımlar

Yeni bir sorgu türü, anlamsal aramanın ilgi derecesini ve yanıt yapılarını mümkün bir şekilde sunar.

Başlamak için [bir anlam sorgusu oluşturun](semantic-how-to-query-request.md) . Veya ilgili bilgiler için aşağıdaki makalelerden birini gözden geçirin.

+ [Sorgu koşullarına yazım denetimi Ekle](speller-how-to-add.md)
+ [Anlamsal derecelendirme ve yanıtlar (yanıtlar ve açıklamalı alt yazılar)](semantic-how-to-query-response.md)