---
title: Anlam derecelendirmesi
titleSuffix: Azure Cognitive Search
description: Anlamsal derecelendirme algoritmasının Azure Bilişsel Arama 'da nasıl çalıştığını öğrenin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/22/2021
ms.openlocfilehash: c3a0a8bd5805757b92e3f5b046335c8883b4ba72
ms.sourcegitcommit: a67b972d655a5a2d5e909faa2ea0911912f6a828
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2021
ms.locfileid: "104888932"
---
# <a name="semantic-ranking-in-azure-cognitive-search"></a>Azure Bilişsel Arama anlam derecelendirmesi

> [!IMPORTANT]
> Anlamsal arama özellikleri, önizleme REST API ve Portal aracılığıyla kullanılabilen genel önizlemededir. Önizleme özellikleri, olduğu gibi, [ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)altında sunulur ve genel kullanıma sunulmakta olan uygulamanın garantisi yoktur. Bu özellikler faturalandırılabilir. Daha fazla bilgi için bkz. [kullanılabilirlik ve fiyatlandırma](semantic-search-overview.md#availability-and-pricing).

Anlamsal sıralama, bir ilk sonuç kümesinin en üst eşleşmelerini yeniden vererek duyarlık artıran sorgu yürütme işlem hattının bir uzantısıdır. Anlam derecelendirmesi, anahtar kelimelerinde dil eşleştirmesinin aksine, sorgu koşullarının anlam anlamı yakalanarak, büyük transformatör tabanlı ağlar tarafından desteklenir. [Varsayılan benzerlik derecelendirme algoritmasındaki](index-ranking-similarity.md)aksine, semantik derecelendiricisini ilgiyi anlamak için, sözcüklerin bağlamını ve anlamını kullanır.

Anlamsal sıralama, kaynak ve zaman yoğunluğu olur. Bir sorgu işleminin beklenen gecikme süresi içinde işlemeyi tamamlayabilmeniz için, semantik derecelendirmede yapılan girişler birleştirilir ve azaltılır, böylece temeldeki özetleme ve yeniden yönlendirme adımları mümkün olduğunca hızlı bir şekilde tamamlanabilir.

## <a name="preparation-for-semantic-ranking"></a>Anlamsal sıralama hazırlığı

Yakınlık açısından Puanlama öncesinde, içerik anlamsal derecelendirmeden verimli bir şekilde işlenebilen bir dizi girişe düşürülmelidir. İçerik azaltma aşağıdaki adım dizisini içerir.

1. İçerik azaltma, anahtar sözcük araması için kullanılan varsayılan [benzerlik derecelendirmesi algoritması](index-ranking-similarity.md) tarafından döndürülen ilk sonuçlar kümesi kullanılarak başlar. Arama sonuçları en fazla 1.000 eşleşme içerebilir, ancak anlam derecelendirmesi yalnızca ilk 50 ' i işleyebilir. 

   Sorgu verildiğinde ilk sonuçlar, kaç eşleşme bulundığına bağlı olarak 50 ' den çok daha az olabilir. Belge sayısı ne olursa olsun, ilk sonuç kümesi, anlam derecelendirmesi için belge yapı olur.

1. Belge Corpus 'da, "searchFields" içindeki her alanın içeriği ayıklanır ve uzun bir dizeye birleştirilir.

1. Genel uzunluğunun özetleme adımının giriş gereksinimlerini karşıladığından emin olmak için, aşırı uzun olan tüm dizeler kırpılır. Bu kırpma işlemi, dizeye dahil olduklarından emin olmak için "searchFields" içindeki kısa alanları ilk olarak konumlandırmak önemlidir. Metin ağır alanları olan çok büyük belgeleriniz varsa, en fazla sınır yoksayıldıktan sonra herhangi bir şey yok sayılır.

Her belge artık tek bir uzun dizeyle temsil edilir.

> [!NOTE]
> Modellere yönelik parametre girişleri, karakterler veya sözcükler değil belirteçlerdir. Simgeleştirme, aranabilir alanlarda çözümleyici ataması tarafından kısmen belirlenir. Dizelerin nasıl simgelendirilebilen hakkında Öngörüler için [Test çözümleyici REST API](/rest/api/searchservice/test-analyzer)kullanarak bir çözümleyici 'nin belirteç çıkışını inceleyebilirsiniz.

## <a name="summarization"></a>Özetleme

Dize azaltmasından sonra, sorguya göre hangi Tümcelerin ve deyimlerin belgeyi en iyi şekilde özetleyeceğini belirleyen, artık makine okuma kavrama ve dil temsili modelleriyle sınırlı girdileri geçirmek mümkündür.

Özetleme girişleri, hazırlama aşamasındaki her belge için elde edilen uzun dizelerdir. Belirli bir girişten, özetleme modeli, ilgili belgeyi en iyi şekilde temsil eden bir belge bulur. Bu, belge için anlamsal bir [Açıklama yazısı](semantic-how-to-query-request.md) da oluşturur. Her bir başlık düz metin olarak kullanılabilir ve vurgular ve belge başına 200 sözcükten azdır.

"Yanıtlar" parametresini belirttiyseniz, sorgu bir soru olarak ortaya anıdıysa ve bu, soruya yanıt veren uzun dizede bir işlem bulunursa, [anlamsal bir yanıt](semantic-answers.md) da döndürülür.

## <a name="scoring-and-ranking"></a>Puanlama ve derecelendirme

1. Açıklamalı alt yazılar, girilen sorguya göre kavramsal ve anlamsal ilgi için değerlendirilir.

   Aşağıdaki diyagramda "anlam uygunluğu" ne anlama geldiğini gösteren bir çizim sunulmaktadır. Finans, hukuk, Coğrafya veya dilbilgisi bağlamında kullanılabilecek "sermaye" terimini göz önünde bulundurun. Bir sorgu aynı vektör alanından terimler içeriyorsa (örneğin, "sermaye" ve "yatırım"), aynı küme içindeki belirteçleri de içeren bir belge, bunlardan daha yüksek puan verir.

   :::image type="content" source="media/semantic-search-overview/semantic-vector-representation.png" alt-text="Bağlam için vektör temsili" border="true":::

1. , @search.rerankerScore Her belgeye, açıklamalı alt yazısının anlam derecesine göre atanır.

1. Tüm belgeler puanlandıktan sonra, puanla azalan sırada listelenir ve sorgu yanıt yüküne dahil edilir. Yük, yanıtlar, düz metin ve vurgulanmış açıklamalı alt yazılar ve bir SELECT yan tümcesinde alınabilir veya belirtilen olarak işaretlediğiniz tüm alanları içerir.

## <a name="next-steps"></a>Sonraki adımlar

Anlamsal sıralama, belirli bölgelerde Standart katmanlarda sunulur. Kullanılabilirlik ve kaydolma hakkında daha fazla bilgi için bkz. [kullanılabilirlik ve fiyatlandırma](semantic-search-overview.md#availability-and-pricing). Yeni bir sorgu türü, anlamsal aramanın derecelendirme ve yanıt yapılarını mümkün bir şekilde sunar. Başlamak için [bir anlam sorgusu oluşturun](semantic-how-to-query-request.md).

Alternatif olarak, varsayılan derecelendirmeden aşağıdaki makalelere göz atın. Anlam derecelendirmesi, ilk sonuçları döndürmek için benzerlik derecelendirmesine bağlıdır. Sorgu yürütme ve derecelendirme hakkında bilinmesi, işlemin tamamının nasıl çalıştığına ilişkin ayrıntılı bilgiler sunar.

+ [Azure Bilişsel Arama 'de tam metin araması](search-lucene-query-architecture.md)
+ [Azure Bilişsel Arama benzerlik ve Puanlama](index-similarity-and-scoring.md)
+ [Azure Bilişsel Arama metin işleme için çözümleyiciler](search-analyzers.md)