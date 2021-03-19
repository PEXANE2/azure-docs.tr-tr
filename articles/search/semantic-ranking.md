---
title: Anlam derecelendirmesi
titleSuffix: Azure Cognitive Search
description: Bilişsel Arama anlam derecelendirme algoritmasını açıklar.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 03/12/2021
ms.openlocfilehash: 01c4d6475ec23b8a55d91e18f49cab27760aa907
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "104604296"
---
# <a name="semantic-ranking-in-azure-cognitive-search"></a>Azure Bilişsel Arama anlam derecelendirmesi

> [!IMPORTANT]
> Anlamsal arama özellikleri, yalnızca önizleme REST API aracılığıyla kullanılabilen genel önizlemededir. Önizleme özellikleri, olduğu gibi, [ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)altında sunulur ve genel kullanıma sunulmakta olan uygulamanın garantisi yoktur. Daha fazla bilgi için bkz. [kullanılabilirlik ve fiyatlandırma](semantic-search-overview.md#availability-and-pricing).

Anlamsal sıralama, bir ilk sonuç kümesinin en üst eşleşmelerini yeniden vererek duyarlık ve geri çağırma işlemlerini artıran sorgu yürütme işlem hattının bir uzantısıdır. Anlam derecelendirmesi, anahtar kelimelerle eşleşen dile karşılık olarak doğal dilde ifade edilen, son derece ayrıntılı makine okuma kavrama modelleri tarafından desteklenir. [Varsayılan benzerlik derecelendirme algoritmasındaki](index-ranking-similarity.md)aksine, semantik derecelendiricisini ilgiyi anlamak için, sözcüklerin bağlamını ve anlamını kullanır.

## <a name="how-semantic-ranking-works"></a>Anlam derecelendirmesi nasıl kullanılır?

Anlamsal sıralama, kaynak ve zaman yoğunluğu olur. Bir sorgu işleminin beklenen gecikme süresi içinde işleme tamamlanabilmesi için, model yalnızca varsayılan [benzerlik derecelendirme algoritmasından](index-ranking-similarity.md)döndürülen ilk 50 belgeyi giriş olarak alır. İlk derecelendirmeden elde edilecek sonuçlar 50 'den fazla eşleşme içerebilir, ancak yalnızca ilk 50 yeniden ayarlanabilir anlamsal olur. 

Anlam derecelendirmesi için model, her birinin sorgu amacına ne kadar iyi eşleştiğini temel alarak belgeleri yeniden Puanlama için hem makine okuma kavrama hem de aktarım öğrenimini kullanır.

### <a name="preparation-passage-extraction-phase"></a>Hazırlık (paszu ayıklama) aşaması

İlk sonuçlardaki her belge için, anahtar paslarını tanımlayan bir paszu ayıklama alıştırması vardır. Bu, içeriğe hafif bir şekilde işlenebilirler.

1. 50 belgelerinin her biri için, searchFields parametresindeki her bir alan ardışık sırayla değerlendirilir. Her bir alandan içerik tek bir Long dize halinde birleştirilir. 

1. Uzun dize daha sonra, genel uzunluğun 8.000 belirteçten fazla olmamasını sağlamak için kırpılır. Bu nedenle, dize içine dahil olmaları için önce kısa alanları konumlandırmanızın kullanılması önerilir. Metin ağır alanları olan çok büyük belgeleriniz varsa, belirteç limitinin ardından herhangi bir şey yok sayılır.

1. Her belge artık 8.000 belirtece kadar olan tek bir Long dizesiyle temsil edilir. Bu dizeler özetleme modeline gönderilir, bu da dizeyi daha da azaltır. Özetleme modeli, belgeyi en iyi şekilde özetleyen veya soruyu yanıtlayan önemli tümceler veya paslar için uzun dizeyi değerlendirir.

1. Bu aşamanın çıktısı bir başlık (ve isteğe bağlı olarak bir yanıt). Başlık, belge başına en çok 128 belirtece sahiptir ve belgenin en fazla temsilcisi olarak kabul edilir.

### <a name="scoring-and-ranking-phases"></a>Puanlama ve derecelendirme aşamaları

Bu aşamada, ilgiyi değerlendirmek için tüm 50 açıklamalı alt yazılar değerlendirilir.

1. Puanlama, girilen sorguya göre kavramsal ve anlamsal ilgi için her bir açıklamalı alt yazı hesaplanarak belirlenir.

   Aşağıdaki diyagramda "anlam uygunluğu" ne anlama geldiğini gösteren bir çizim sunulmaktadır. Finans, hukuk, Coğrafya veya dilbilgisi bağlamında kullanılabilecek "sermaye" terimini göz önünde bulundurun. Bir sorgu aynı vektör alanından terimler içeriyorsa (örneğin, "sermaye" ve "yatırım"), aynı küme içindeki belirteçleri de içeren bir belge, bunlardan daha yüksek puan verir.

   :::image type="content" source="media/semantic-search-overview/semantic-vector-representation.png" alt-text="Bağlam için vektör temsili" border="true":::

1. Bu aşamanın çıktısı @search.rerankerScore her belgeye atanır. Tüm belgeler puanlandıktan sonra, Bunlar azalan sırada listelenir ve sorgu yanıt yüküne dahil edilir.

## <a name="next-steps"></a>Sonraki adımlar

Anlamsal sıralama, belirli bölgelerde Standart katmanlarda sunulur. Daha fazla bilgi edinmek ve kaydolmak için bkz. [kullanılabilirlik ve fiyatlandırma](semantic-search-overview.md#availability-and-pricing). Yeni bir sorgu türü, anlamsal aramanın ilgi derecesini ve yanıt yapılarını mümkün bir şekilde sunar. Başlamak için [bir anlam sorgusu oluşturun](semantic-how-to-query-request.md).

Alternatif olarak, ilgili bilgiler için aşağıdaki makalelerden birini gözden geçirin.

+ [Anlamsal aramaya genel bakış](semantic-search-overview.md)
+ [Anlam yanıtı döndürme](semantic-answers.md)