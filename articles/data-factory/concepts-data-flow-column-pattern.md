---
title: Azure Data Factory eşleme veri akışındaki sütun desenleri
description: Azure Data Factory eşleme veri akışlarında sütun desenleri kullanarak Genelleştirilmiş veri dönüştürme desenleri oluşturma
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/21/2019
ms.openlocfilehash: 0c9a3c2ef05f4a11933ca7fc81c7c0f87a612293
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78395883"
---
# <a name="using-column-patterns-in-mapping-data-flow"></a>Eşleme veri akışında sütun düzenlerini kullanma

Birçok eşleme veri akışı dönüştürmesi, sabit kodlanmış sütun adları yerine, şablon sütunlarına göre bir düzen ile başvuru yapmanıza olanak sağlar. Bu eşleştirme *sütun desenleri*olarak bilinir. Sütunları ad, veri türü, akış veya konuma göre eşleştirmek için, tam alan adları gerektirmek yerine desenler tanımlayabilirsiniz. Sütun desenlerinin yararlı olduğu iki senaryo vardır:

* Gelen kaynak alanları, metin dosyalarındaki veya NoSQL veritabanlarındaki sütunları değiştirme durumu gibi genellikle değiştirilirse. Bu senaryo, [şema dradı](concepts-data-flow-schema-drift.md)olarak bilinir.
* Büyük bir sütun grubu üzerinde ortak bir işlem yapmak istiyorsanız. Örneğin, sütun adında ' Total ' olan her sütunu Double olarak atama.

Sütun desenleri Şu anda türetilmiş sütunda, toplama, seçme ve havuz dönüşümlerinde kullanılabilir.

## <a name="column-patterns-in-derived-column-and-aggregate"></a>Türetilmiş sütunda ve toplamada sütun desenleri

Bir toplama dönüşümünün türetilmiş bir sütuna veya toplamalar sekmesine bir sütun stili eklemek için, varolan bir sütunun sağ tarafındaki artı simgesine tıklayın. **Sütun Ekle deseninin**seçimini yapın. 

![sütun desenleri](media/data-flow/columnpattern.png "Sütun Desenleri")

Eşleştirme koşulunu girmek için [ifade oluşturucusunu](concepts-data-flow-expression-builder.md) kullanın. Sütunun `name`, `type`, `stream`ve `position` göre sütunlarla eşleşen bir Boole ifadesi oluşturun. Bu model, koşulun true döndüğü düzeltebilecekler veya tanımlanmış tüm sütunları etkiler.

Eşleştirme koşulunun altındaki iki ifade kutusu, etkilenen sütunların yeni adlarını ve değerlerini belirtir. Eşleşen alanın varolan değerine başvurmak için `$$` kullanın. Sol ifade kutusu, adı tanımlar ve sağ ifade kutusu değeri tanımlar.

![sütun desenleri](media/data-flow/columnpattern2.png "Sütun Desenleri")

Yukarıdaki sütun deseninin Double türü her sütunla eşleşiyor ve eşleşme başına bir toplama sütunu oluşturuyor. Yeni sütunun adı, eşleşen sütun adının ' _total ' ile bitiştirildiği addır. Yeni sütunun değeri, var olan çift değerin yuvarlanmış, toplanmış toplamıdır.

Eşleştirme koşullarınızın doğru olduğunu doğrulamak için **İnceleme** sekmesinde tanımlı sütunların çıktı şemasını doğrulayabilir veya **veri önizleme** sekmesindeki verilerin bir anlık görüntüsünü alabilirsiniz. 

![sütun desenleri](media/data-flow/columnpattern3.png "Sütun Desenleri")

## <a name="rule-based-mapping-in-select-and-sink"></a>Select ve Sink içinde kural tabanlı eşleme

Kaynaktaki sütunları eşlerken ve dönüşümleri seçtiğinizde, sabit eşleme veya kural tabanlı eşlemeler ekleyebilirsiniz. Verilerinizin şemasını biliyorsanız ve kaynak veri kümesinden belirli sütunların belirli statik adları her zaman eşleşecek şekilde beklediğinizi, sabit eşleme kullanın. Esnek şemalar ile çalışıyorsanız, sütun `name`, `type`, `stream`ve `position` göre bir kalıp eşleşmesi oluşturmak için kural tabanlı eşleme kullanın. Sabit ve kural tabanlı eşlemelerin herhangi bir birleşimini kullanabilirsiniz. 

Kural tabanlı eşleme eklemek için **eşleme Ekle** ' ye tıklayın ve **kural tabanlı eşleme**' yi seçin.

![kural tabanlı eşleme](media/data-flow/rule2.png "Kural tabanlı eşleme")

Sol ifade kutusunda, Boolean eşleşme koşulunuz girin. Sağ ifade kutusunda, eşleşen sütunun ne eşleştirileceği belirtin. Eşleşen alanın mevcut adına başvurmak için `$$` kullanın.

Aşağı köşeli çift ayraç simgesine tıklarsanız, bir Regex eşleme koşulu belirtebilirsiniz.

Hangi tanımlanmış sütunların eşleştiğini ve ne eşlendiğine bakmak için kural tabanlı eşlemenin yanındaki göz gözlük simgesine tıklayın.

![kural tabanlı eşleme](media/data-flow/rule1.png "Kural tabanlı eşleme")

Yukarıdaki örnekte, iki kural tabanlı eşleme oluşturulur. İlki, ' Movie ' adlı tüm sütunları alır ve bunları mevcut değerleriyle eşler. İkinci kural, ' Movie ' ile başlayan ve bunları ' Movieıd ' sütunuyla eşleyen tüm sütunları eşleştirmek için Regex kullanır.

Kuralınız birden çok özdeş eşleştirmelerle sonuçlanırsa, **yinelenen girişleri atlamayı** etkinleştirin veya **yinelenen çıkışları atlayıp** yinelenenleri önleyin.

## <a name="pattern-matching-expression-values"></a>Model eşleştirme ifadesi değerleri.

* `$$` çalışma zamanında her eşleşmenin adına veya değerine çevirir
* `name` gelen her sütunun adını temsil eder
* `type` gelen her sütunun veri türünü temsil eder
* `stream` her bir akışla veya akışdaki dönüşümle ilişkilendirilen adı temsil eder
* `position`, veri akışındaki sütunların sıralı konumudur

## <a name="next-steps"></a>Sonraki adımlar
* Veri dönüştürmeleri için eşleme veri akışı [ifade dili](data-flow-expression-functions.md) hakkında daha fazla bilgi edinin
* [Havuz dönüşümünde](data-flow-sink.md) sütun düzenlerini kullanın ve kural tabanlı eşleme ile [dönüştürme seçeneğini belirleyin](data-flow-select.md)
