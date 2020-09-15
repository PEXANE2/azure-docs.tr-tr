---
title: Azure Data Factory eşleme veri akışındaki sütun desenleri
description: Azure Data Factory eşleme veri akışlarında sütun desenleri kullanarak Genelleştirilmiş veri dönüştürme desenleri oluşturma
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 09/14/2020
ms.openlocfilehash: c6a2d38644d844cb1231a24465478b7f70a85111
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90531175"
---
# <a name="using-column-patterns-in-mapping-data-flow"></a>Eşleme veri akışında sütun düzenlerini kullanma

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Birçok eşleme veri akışı dönüştürmesi, sabit kodlanmış sütun adları yerine, şablon sütunlarına göre bir düzen ile başvuru yapmanıza olanak sağlar. Bu eşleştirme *sütun desenleri*olarak bilinir. Sütunları ad, veri türü, akış veya konuma göre eşleştirmek için, tam alan adları gerektirmek yerine desenler tanımlayabilirsiniz. Sütun desenlerinin yararlı olduğu iki senaryo vardır:

* Gelen kaynak alanları, metin dosyalarındaki veya NoSQL veritabanlarındaki sütunları değiştirme durumu gibi genellikle değiştirilirse. Bu senaryo, [şema dradı](concepts-data-flow-schema-drift.md)olarak bilinir.
* Büyük bir sütun grubu üzerinde ortak bir işlem yapmak istiyorsanız. Örneğin, sütun adında ' Total ' olan her sütunu Double olarak atama.

Sütun desenleri Şu anda türetilmiş sütunda, toplama, seçme ve havuz dönüşümlerinde kullanılabilir.

## <a name="column-patterns-in-derived-column-and-aggregate"></a>Türetilmiş sütunda ve toplamada sütun desenleri

Türetilmiş bir sütuna, toplamaya veya pencere dönüşümüne bir sütun stili eklemek için, sütun listesinin üstüne **Ekle** ' ye veya varolan bir türetilmiş sütunun yanındaki artı simgesine tıklayın. **Sütun Ekle kalıbı**seçin.

![sütun desenleri](media/data-flow/add-column-pattern.png "Sütun Desenleri")

Eşleştirme koşulunu girmek için [ifade oluşturucusunu](concepts-data-flow-expression-builder.md) kullanın. Sütunu,,, ve ' ı temel alarak sütunlarla eşleşen bir Boole ifadesi oluşturun `name` `type` `stream` `position` . Bu model, koşulun true döndüğü düzeltebilecekler veya tanımlanmış tüm sütunları etkiler.

Eşleştirme koşulunun altındaki iki ifade kutusu, etkilenen sütunların yeni adlarını ve değerlerini belirtir. `$$`Eşleşen alanın varolan değerine başvurmak için kullanın. Sol ifade kutusu, adı tanımlar ve sağ ifade kutusu değeri tanımlar.

![sütun desenleri](media/data-flow/edit-column-pattern.png "Sütun Desenleri")

Yukarıdaki sütun deseninin Double türü her sütunla eşleşiyor ve eşleşme başına bir türetilmiş sütun oluşturuyor. `$$`Sütun adı alanı olarak belirterek, eşleşen her sütun aynı adla güncelleştirilir. Her sütunun değeri, varolan değerin iki ondalık noktalara yuvarlanmasını sağlar.

Eşleştirme koşullarınızın doğru olduğunu doğrulamak için **İnceleme** sekmesinde tanımlı sütunların çıktı şemasını doğrulayabilir veya **veri önizleme** sekmesindeki verilerin bir anlık görüntüsünü alabilirsiniz. 

![sütun desenleri](media/data-flow/columnpattern3.png "Sütun Desenleri")

## <a name="rule-based-mapping-in-select-and-sink"></a>Select ve Sink içinde kural tabanlı eşleme

Kaynaktaki sütunları eşlerken ve dönüşümleri seçtiğinizde, sabit eşleme veya kural tabanlı eşlemeler ekleyebilirsiniz. ,,, Ve sütunlarını temel alarak eşleştirin `name` `type` `stream` `position` . Sabit ve kural tabanlı eşlemelerin herhangi bir birleşimini kullanabilirsiniz. Varsayılan olarak, 50 'den büyük sütunları olan tüm projeksiyonlar varsayılan olarak, her sütunda eşleşen ve giriş yapan adı izleyen kural tabanlı bir eşleme olur. 

Kural tabanlı eşleme eklemek için **eşleme Ekle** ' ye tıklayın ve **kural tabanlı eşleme**' yi seçin.

![kural tabanlı eşleme](media/data-flow/rule2.png "Kural tabanlı eşleme")

Her kural tabanlı eşleme için iki giriş gerekir: ile eşleşmesi gereken durum ve her eşlenmiş sütunun adı. Her iki değer de [ifade Oluşturucusu](concepts-data-flow-expression-builder.md)aracılığıyla yapılır. Sol ifade kutusunda, Boolean eşleşme koşulunuz girin. Sağ ifade kutusunda, eşleşen sütunun ne eşleştirileceği belirtin.

![kural tabanlı eşleme](media/data-flow/rule-based-mapping.png "Kural tabanlı eşleme")

`$$`Eşleşen bir sütunun giriş adına başvurmak için söz dizimini kullanın. Yukarıdaki görüntünün bir örnek olarak kullanılması, bir kullanıcının adı altı karakterden kısa olan tüm dize sütunlarında eşleştirmek istediğini varsayalım. Gelen bir sütun adlandırıldıysa `test` , ifadesi `$$ + '_short'` sütunu yeniden adlandırır `test_short` . Var olan tek eşleme varsa, koşulu karşılamayan tüm sütunlar, outputfrom verilerinden bırakılır.

Desenler hem düzeltebilecekler hem de tanımlı sütunlarla eşleşir. Hangi tanımlı sütunların bir kuralla eşlendiğini görmek için kuralın yanındaki göz gözlük simgesine tıklayın. Veri önizlemeyi kullanarak çıktlarınızı doğrulayın.

### <a name="regex-mapping"></a>Regex eşleme

Aşağı köşeli çift ayraç simgesine tıklarsanız, bir Regex-Mapping koşulu belirtebilirsiniz. Bir Regex eşleme koşulu, belirtilen Regex koşuluyla eşleşen tüm sütun adlarıyla eşleşir. Bu, standart kural tabanlı eşlemelerle birlikte kullanılabilir.

![kural tabanlı eşleme](media/data-flow/regex-matching.png "Kural tabanlı eşleme")

Yukarıdaki örnek, Regex düzeniyle `(r)` veya küçük harf içeren bir sütun adı ile eşleşir. Standart kural tabanlı eşlemeye benzer şekilde, eşleşen tüm sütunlar, sözdizimi kullanılarak sağdaki koşul tarafından değiştirilir `$$` .

### <a name="rule-based-hierarchies"></a>Kural tabanlı hiyerarşiler

Tanımlı projeksiyonda bir hiyerarşisi varsa, hiyerarşiler alt sütunlarını eşlemek için kural tabanlı eşleme kullanabilirsiniz. Eşleşen bir koşul ve alt sütunlarını eşlemek istediğiniz karmaşık sütunu belirtin. Sağ tarafta belirtilen ' ad As ' kuralı kullanılarak eşleşen her alt sütun silinir.

![kural tabanlı eşleme](media/data-flow/rule-based-hierarchy.png "Kural tabanlı eşleme")

Yukarıdaki örnek, karmaşık sütunun tüm alt sütunlarında eşleşir `a` . `a` iki alt sütun `b` ve içerir `c` . Çıkış şeması iki sütun içerir `b` ve `c` ' ad As ' koşulu olur `$$` .

## <a name="pattern-matching-expression-values"></a>Model eşleştirme ifadesi değerleri.

* `$$` çalışma zamanında her eşleşmenin adına veya değerine çevirir
* `name` Her gelen sütunun adını temsil eder
* `type` Her gelen sütunun veri türünü temsil eder
* `stream` akışdaki her bir akışla veya dönüşümle ilişkilendirilen adı temsil eder
* `position` , veri akışındaki sütunların sıralı konumudur

## <a name="next-steps"></a>Sonraki adımlar
* Veri dönüştürmeleri için eşleme veri akışı [ifade dili](data-flow-expression-functions.md) hakkında daha fazla bilgi edinin
* [Havuz dönüşümünde](data-flow-sink.md) sütun düzenlerini kullanın ve kural tabanlı eşleme ile [dönüştürme seçeneğini belirleyin](data-flow-select.md)
