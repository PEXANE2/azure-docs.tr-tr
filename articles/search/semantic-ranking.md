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
ms.openlocfilehash: e3078c8f71f8862cacad552bb3176c08530e79bb
ms.sourcegitcommit: df1930c9fa3d8f6592f812c42ec611043e817b3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2021
ms.locfileid: "103418853"
---
# <a name="semantic-ranking-in-azure-cognitive-search"></a>Azure Bilişsel Arama anlam derecelendirmesi

> [!IMPORTANT]
> Anlamsal arama özellikleri, yalnızca önizleme REST API aracılığıyla kullanılabilen genel önizlemededir. Önizleme özellikleri, olduğu gibi, [ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)altında sunulur ve genel kullanıma sunulmakta olan uygulamanın garantisi yoktur. Daha fazla bilgi için bkz. [kullanılabilirlik ve fiyatlandırma](semantic-search-overview.md#availability-and-pricing).

Anlamsal sıralama, bir ilk sonuç kümesinin en üst eşleşmelerini yeniden vererek duyarlık ve geri çağırma işlemlerini artıran sorgu yürütme işlem hattının bir uzantısıdır. Anlam derecelendirmesi, anahtar kelimelerle eşleşen dile karşılık olarak doğal dilde ifade edilen, son derece ayrıntılı makine okuma kavrama modelleri tarafından desteklenir. [Varsayılan benzerlik derecelendirme algoritmasındaki](index-ranking-similarity.md)aksine, semantik derecelendiricisini ilgiyi anlamak için, sözcüklerin bağlamını ve anlamını kullanır.

## <a name="how-semantic-ranking-works"></a>Anlam derecelendirmesi nasıl kullanılır?

Anlamsal sıralama, kaynak ve zaman yoğunluğu olur. Bir sorgu işleminin beklenen gecikme süresi içinde işleme tamamlanabilmesi için, model yalnızca varsayılan [benzerlik derecelendirme algoritmasından](index-ranking-similarity.md)döndürülen ilk 50 belgeyi giriş olarak alır. İlk derecelendirmeden elde edilecek sonuçlar 50 'den fazla eşleşme içerebilir, ancak yalnızca ilk 50 yeniden ayarlanabilir anlamsal olur. 

Anlam derecelendirmesi için model, her birinin sorgu amacına ne kadar iyi eşleştiğini temel alarak belgeleri yeniden Puanlama için hem makine okuma kavrama hem de aktarım öğrenimini kullanır.

1. Her belge için, semantik derecelendiricisini searchfields parametresindeki alanları, içeriği bir büyük dize halinde birleştirerek değerlendirir.

1. Daha sonra dize, genel uzunluğunun 8.000 belirteçten fazla olmamasını sağlamak için kırpılır. Çok büyük belgeleriniz varsa, çok sayıda içeriğe sahip bir içerik alanı veya merged_content alanı varsa, belirteç sınırından sonra herhangi bir şey yok sayılır.

1. 50 belgelerinin her biri artık tek bir Long dizesiyle temsil edilir. Bu dize özetleme modeline gönderilir. Özetleme modeli, içeriği özetlemek veya soruyu yanıtlamak için görüntülenen pasları belirlemek için makine okuma kavrama kullanarak açıklamalı alt yazılar (ve yanıtlar) oluşturur. Özetleme modelinin çıktısı, en fazla 128 belirteç olan daha azaltılmış bir dizedir.

1. Daha küçük dize, belgenin açıklamalı alt yazı haline gelir ve daha büyük dizede bulunan en ilgili pasalleri temsil eder. 50 (veya daha az) açıklamalı alt yazı kümesi daha sonra sipariş ilgisiyle derecelendirilir. 

Kavramsal ve anlam ilgisi vektör temsili ve terim kümeleri aracılığıyla oluşturulur. Anahtar sözcük benzerliği algoritması, sorgudaki herhangi bir terime eşit ağırlığa izin verebilir, anlam modeli, başka bir şekilde yüzeyinde bulunmayan sözcükler arasındaki bağımlılıkları ve ilişkileri tanımak üzere eğitilmiştir. Sonuç olarak, bir sorgu dizesi aynı kümeden terimler içeriyorsa, her ikisini de içeren bir belge, bunlardan daha yüksek bir değere sahip olur.

:::image type="content" source="media/semantic-search-overview/semantic-vector-representation.png" alt-text="Bağlam için vektör temsili" border="true":::

## <a name="next-steps"></a>Sonraki adımlar

Anlamsal sıralama, belirli bölgelerde Standart katmanlarda sunulur. Daha fazla bilgi edinmek ve kaydolmak için bkz. [kullanılabilirlik ve fiyatlandırma](semantic-search-overview.md#availability-and-pricing).

Yeni bir sorgu türü, anlamsal aramanın ilgi derecesini ve yanıt yapılarını mümkün bir şekilde sunar. Başlamak için [bir anlam sorgusu oluşturun](semantic-how-to-query-request.md) .

Alternatif olarak, ilgili bilgiler için aşağıdaki makalelerden birini gözden geçirin.

+ [Sorgu koşullarına yazım denetimi Ekle](speller-how-to-add.md)
+ [Anlam yanıtı döndürme](semantic-answers.md)