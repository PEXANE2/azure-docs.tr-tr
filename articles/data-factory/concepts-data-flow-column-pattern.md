---
title: Azure Data Factory eşleme veri akışı sütun desenleri
description: Veri akışlarında eşleme Azure Data Factory sütun desenleri kullanarak Genelleştirilmiş veri dönüştürme desenleri oluşturma
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 4df831c1329ab13f19e6ecf979e404d4a90e5f72
ms.sourcegitcommit: 3fa4384af35c64f6674f40e0d4128e1274083487
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71219756"
---
# <a name="mapping-data-flows-column-patterns"></a>Veri akışları eşleme sütun desenleri

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Birçok Azure Data Factory veri akışı dönüştürmesi "sütun desenleri" fikrini destekler, böylece sabit kodlanmış sütun adları yerine desenleri temel alan şablon sütunları oluşturabilirsiniz. Bu özelliği Ifade Oluşturucu içinde, tam, belirli alan adları istemek yerine dönüştürme için sütunlarla eşleşecek desenler tanımlamak üzere kullanabilirsiniz. Desenler, özellikle metin dosyalarındaki veya NoSQL veritabanlarındaki sütunları değiştirmek durumunda, gelen kaynak alanları sık değiştirilirse yararlıdır. Bu durum bazen "Schema drift" olarak adlandırılır.

Bu "esnek şema" işleme Şu anda türetilmiş sütunda ve toplam dönüşümlerinde, Select ve Sink dönüştürmelerinin yanı sıra "kural tabanlı eşleme" olarak bulunur.

![sütun desenleri](media/data-flow/columnpattern2.png "Sütun desenleri")

## <a name="column-patterns"></a>Sütun desenleri
Sütun desenleri, hem şema DRFT senaryolarını hem de genel senaryoları işlemek için yararlıdır. Her sütun adını tam olarak bildiğiniz koşullarda iyidir. Sütun adı ve sütun veri türünde bir eşleme deseni oluşturabilir ve bu işlemi, deseniniz `name`  &  `type` ile eşleşen veri akışındaki herhangi bir alana karşı gerçekleştirecek bir dönüştürme ifadesi oluşturabilirsiniz.

Desenleri kabul eden bir dönüşüme ifade eklerken, "sütun düzeni Ekle" yi seçin. Sütun desenleri, şema DRFT sütun eşleştirme desenlerine izin verir.

Şablon sütun desenleri oluştururken, giriş veri `$$` akışından eşleşen her alan için bir başvuruyu temsil etmek üzere ifadesinde kullanın.

Ifade Oluşturucu Regex işlevlerinden birini kullanmayı seçerseniz, daha sonra $1, $2, $3... seçeneğini kullanabilirsiniz. Regex deyiminizden eşleşen alt desenlere başvurmak için.

Sütun deseninin bir örneği, bir dizi gelen alanları içeren TOPLAMı kullanmaktır. Toplam SUM hesaplamaları toplam dönüşümde bulunur. Daha sonra, "integer" ile eşleşen alan türlerinin her biriyle TOPLAMı kullanabilir ve sonra deyiminizdeki her eşleştirmeye başvurmak için $ $ kullanabilirsiniz.

## <a name="match-columns"></a>Sütunları Eşleştir
![sütun stili türleri](media/data-flow/pattern2.png "Model türleri")

Sütunları temel alan desenler oluşturmak için, sütun adı, tür, akış veya konum üzerinde eşleştirebilir ve ifade işlevleri ve normal ifadelerle bunların herhangi bir birleşimini kullanabilirsiniz.

![sütun konumu](media/data-flow/position.png "Sütun konumu")

## <a name="rule-based-mapping"></a>Kural tabanlı eşleme
Kaynaktaki sütunları eşlerken ve dönüşümleri seçtiğinizde, "sabit eşleme" veya "kural tabanlı eşleme" seçeneğini belirleyebilirsiniz. Verilerinizin şemasını bildiğiniz ve kaynak veri kümesinden her zaman belirli statik adlarıyla eşleşen belirli sütunlar beklendiğinde, sabit eşlemeyi kullanabilirsiniz. Ancak esnek şemalar ile çalışırken kural tabanlı eşleme kullanın. Yukarıda açıklanan kuralları kullanarak bir model eşleşmesi derleyebilirsiniz.

![kural tabanlı eşleme](media/data-flow/rule2.png "Kural tabanlı eşleme")

Deyim oluşturucuyu kullanarak kurallarınızı oluşturun. Deyimleriniz sütunlar (true) veya dışlama sütunları (false) için bir Boole değeri döndürür.

## <a name="pattern-matching-special-columns"></a>Özel sütunlar ile eşleşen desenler

* `$$`, hata ayıklama modundaki tasarım zamanında ve çalışma zamanında yürütme sonrasında her eşleşmenin adına çevrilecek
* `name`Her gelen sütunun adını temsil eder
* `type`Her gelen sütunun veri türünü temsil eder
* `stream`akışdaki her bir Stream veya dönüşümle ilişkilendirilen adı temsil eder
* `position`, veri akışındaki sütunların sıralı konumudur

## <a name="next-steps"></a>Sonraki adımlar
* Veri dönüştürmeleri için ADF eşleme veri akışı [ifade dili](https://aka.ms/dataflowexpressions) hakkında daha fazla bilgi edinin
* [Havuz dönüşümünde](data-flow-sink.md) sütun düzenlerini kullanın ve kural tabanlı eşleme Ile [dönüştürme seçeneğini belirleyin](data-flow-select.md)
