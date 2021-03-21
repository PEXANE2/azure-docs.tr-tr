---
title: Anlam derecelendirmesi
titleSuffix: Azure Cognitive Search
description: Anlamsal derecelendirme algoritmasının Azure Bilişsel Arama 'da nasıl çalıştığını öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/18/2021
ms.openlocfilehash: bb65a53f1ba6e97a39bd0c0170c5c41da38aee8b
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "104720517"
---
# <a name="semantic-ranking-in-azure-cognitive-search"></a>Azure Bilişsel Arama anlam derecelendirmesi

> [!IMPORTANT]
> Anlamsal arama özellikleri, yalnızca önizleme REST API aracılığıyla kullanılabilen genel önizlemededir. Önizleme özellikleri, olduğu gibi, [ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)altında sunulur ve genel kullanıma sunulmakta olan uygulamanın garantisi yoktur. Bu özellikler faturalandırılabilir. Daha fazla bilgi için bkz. [kullanılabilirlik ve fiyatlandırma](semantic-search-overview.md#availability-and-pricing).

Anlamsal sıralama, bir ilk sonuç kümesinin en üst eşleşmelerini yeniden vererek duyarlık ve geri çağırma işlemlerini artıran sorgu yürütme işlem hattının bir uzantısıdır. Anlam derecelendirmesi, anahtar kelimelerle eşleşen dile karşılık gelen, doğal dilde ifade edilen sorgular için eğitilen, son derece makine okuma anlama modelleri tarafından desteklenir. [Varsayılan benzerlik derecelendirme algoritmasındaki](index-ranking-similarity.md)aksine, semantik derecelendiricisini ilgiyi anlamak için, sözcüklerin bağlamını ve anlamını kullanır.

Anlamsal sıralama, kaynak ve zaman yoğunluğu olur. Bir sorgu işleminin beklenen gecikme süresi içinde işlemeyi tamamlayabilmeniz için, Özet ve analiz 'nin mümkün olduğunca çabuk tamamlanabilmesi için girişler birleştirilir ve basitleştirilir.

## <a name="preparation-for-semantic-ranking"></a>Anlamsal sıralama hazırlığı

Yakınlık açısından Puanlama öncesinde, içerik anlamsal derecelendirmeden verimli bir şekilde işlenebilen bir parametre miktarına düşürülmelidir. İçerik azaltma aşağıdaki adım dizisini içerir.

1. İçerik azaltma, anahtar sözcük araması için kullanılan varsayılan [benzerlik derecelendirmesi algoritması](index-ranking-similarity.md) tarafından döndürülen ilk sonuçları kullanarak başlar. Arama sonuçları en fazla 1.000 eşleşme içerebilir, ancak anlam derecelendirmesi yalnızca ilk 50 ' i işleyebilir. 

   Sorgu verildiğinde ilk sonuçlar, kaç eşleşme bulundığına bağlı olarak 50 ' den çok daha az olabilir. Belge sayısı ne olursa olsun, ilk sonuç kümesi, anlam derecelendirmesi için belge yapı olur.

1. Belge Corpus 'da, "searchFields" içindeki her alanın içeriği ayıklanır ve uzun bir dizeye birleştirilir.

1. Genel uzunluğunun özetleme modelinin giriş gereksinimlerini karşıladığından emin olmak için, aşırı uzun olan tüm dizeler kırpılır. Bu kırpma işlemi, dizeye dahil olduklarından emin olmak için "searchFields" içindeki kısa alanları ilk olarak konumlandırmak önemlidir. Metin ağır alanları olan çok büyük belgeleriniz varsa, en fazla sınır yoksayıldıktan sonra herhangi bir şey yok sayılır.

Her belge artık tek bir uzun dizeyle temsil edilir.

> [!NOTE]
> Modellere yönelik parametre girişleri karakter veya sözcük olmayan belirteçlerdir. Simgeleştirme, aranabilir alanlarda çözümleyici ataması tarafından kısmen belirlenir. Dizelerin nasıl simgelendirilebilen hakkında Öngörüler için [Test çözümleyici REST API](/rest/api/searchservice/test-analyzer)kullanarak bir çözümleyici 'nin belirteç çıkışını inceleyebilirsiniz.
>
> Şu anda bu önizlemede, uzun dizeler boyut olarak en fazla 8.000 belirteç olabilir. Arama, bir belgede derin bir şekilde beklenen bir yanıt sunamazsa, içerik kırpma hakkında bilinmesi, nedenini anlamanıza yardımcı olur. 

## <a name="summarization"></a>Özetleme

Dize azaltmasından sonra, hangi Tümcelerin ve tümceciklerin sorguya göre en iyi şekilde özetlendiğini belirleyebilmek için, parametreleri makine okuma kavrama ve dil gösterimi aracılığıyla geçirmek mümkündür.

Özetleme girişleri, hazırlama aşamasından uzun dizedir. Bu girişten, özetleme modeli, en çok temsili olan metinlerin bulmak için içeriği değerlendirir.

Çıktı, düz metin olarak ve vurgularla bir [anlamsal başlıktır](semantic-how-to-query-request.md). Başlık, genellikle belge başına 200 sözcükten az olan uzun dizeden daha küçüktür ve belge temsilcisi olarak kabul edilir. 

"Yanıtlar" parametresini belirttiyseniz, sorgu bir soru olarak ortaya anıyorken ve bu, soruya yönelik olabilecek bir yanıt gibi görünen uzun dizede bir işlem bulunursa, [anlamsal bir yanıt](semantic-answers.md) da döndürülür.

## <a name="scoring-and-ranking"></a>Puanlama ve derecelendirme

Bu noktada, artık her belge için açıklamalı alt yazılar vardır. Açıklamalı alt yazılar sorguyla ilgisi olarak değerlendirilir.

1. Puanlama, girilen sorguya göre kavramsal ve anlamsal ilgi için her bir açıklamalı alt yazı hesaplanarak belirlenir.

   Aşağıdaki diyagramda "anlam uygunluğu" ne anlama geldiğini gösteren bir çizim sunulmaktadır. Finans, hukuk, Coğrafya veya dilbilgisi bağlamında kullanılabilecek "sermaye" terimini göz önünde bulundurun. Bir sorgu aynı vektör alanından terimler içeriyorsa (örneğin, "sermaye" ve "yatırım"), aynı küme içindeki belirteçleri de içeren bir belge, bunlardan daha yüksek puan verir.

   :::image type="content" source="media/semantic-search-overview/semantic-vector-representation.png" alt-text="Bağlam için vektör temsili" border="true":::

1. Bu aşamanın çıktısı @search.rerankerScore her belgeye atanır. Tüm belgeler puanlandıktan sonra, Bunlar azalan sırada listelenir ve sorgu yanıt yüküne dahil edilir. Yük, yanıtlar, düz metin ve vurgulanmış açıklamalı alt yazılar ve bir SELECT yan tümcesinde alınabilir veya belirtilen olarak işaretlediğiniz tüm alanları içerir.

## <a name="next-steps"></a>Sonraki adımlar

Anlamsal sıralama, belirli bölgelerde Standart katmanlarda sunulur. Kullanılabilir ve kaydolma hakkında daha fazla bilgi için bkz. [kullanılabilirlik ve fiyatlandırma](semantic-search-overview.md#availability-and-pricing). Yeni bir sorgu türü, anlamsal aramanın ilgi derecesini ve yanıt yapılarını mümkün bir şekilde sunar. Başlamak için [bir anlam sorgusu oluşturun](semantic-how-to-query-request.md).

Alternatif olarak, varsayılan derecelendirmeden aşağıdaki makalelere göz atın. Anlam derecelendirmesi, ilk sonuçları döndürmek için benzerlik derecelendirmesine bağlıdır. Sorgu yürütme ve derecelendirme hakkında bilinmesi, işlemin tamamının nasıl çalıştığına ilişkin ayrıntılı bilgiler sunar.

+ [Azure Bilişsel Arama 'de tam metin araması](search-lucene-query-architecture.md)
+ [Azure Bilişsel Arama benzerlik ve Puanlama](index-similarity-and-scoring.md)
+ [Azure Bilişsel Arama metin işleme için çözümleyiciler](search-analyzers.md)