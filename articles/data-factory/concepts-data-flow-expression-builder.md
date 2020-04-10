---
title: Veri akışını nakışlamada ifade oluşturucu
description: Azure Veri Fabrikası'ndaki veri akışlarını eşlemede Expression Builder'ı kullanarak ifadeler oluşturma
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 04/08/2020
ms.openlocfilehash: 196d917d31eb08af80587bba30d9f7e67bf8cbea
ms.sourcegitcommit: a53fe6e9e4a4c153e9ac1a93e9335f8cf762c604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80991714"
---
# <a name="build-expressions-in-mapping-data-flow"></a>Veri akışını eşlemede ifadeler oluşturma

Veri akışının eşlemesinde, birçok dönüştürme özelliği ifade olarak girilir. Bu ifadeler, çalışma zamanında bir Spark veri türüne değerlendiren sütun değerleri, parametreler, işlevler, işleçler ve gerçek lerden oluşur.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tkur]

## <a name="open-expression-builder"></a>Açık İfade Oluşturucu

Azure Veri Fabrikası kullanıcı deneyimindeki ifade düzenleme arabirimi, İfade Oluşturucusu olarak bilinir. İfade mantığınızı girdiğinizde, Veri Fabrikası vurgulama, sözdizimi denetimi ve otomatik tamamlama için [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense?view=vs-2019) kod tamamlamayı kullanır.

![İfade Oluşturucu](media/data-flow/xpb1.png "İfade Oluşturucusu")

İfadelerin zorunlu olduğu türemiş sütun ve filtre gibi dönüşümlerde, mavi ifade kutusunu seçerek İfade Oluşturucu'yu açın.

![Mavi ifade kutusu](media/data-flow/expressionbox.png "İfade Oluşturucusu")

Bir eşleşen veya grup-by koşulda sütunbaşvuruyorsanız, bir ifade sütunlardan değerleri ayıklayabilir. İfade oluşturmak **için, Hesaplanmış sütunu**seçin.

![Hesaplanmış sütun seçeneği](media/data-flow/computedcolumn.png "İfade Oluşturucusu")

İfade veya gerçek değerin geçerli girdiler olduğu durumlarda, gerçek bir değere değer biçen bir ifade oluşturmak için **dinamik içerik ekle'yi** seçin.

![Dinamik içerik seçeneği ekleme](media/data-flow/add-dynamic-content.png "İfade Oluşturucusu")

## <a name="expression-language-reference"></a>İfade dili başvurusu

Veri akışlarını eşlemede, ifadelerde kullanılabilecek yerleşik işlevler ve işleçler vardır. Kullanılabilir işlevlerin listesi için, [eşleme veri akışındaki İfade işlevlerine](data-flow-expression-functions.md)bakın.

## <a name="column-names-with-special-characters"></a>Özel karakterlere sahip sütun adları

Özel karakterler veya boşluklar içeren sütun adlarınız varsa, bir ifadede başvurmak için adı kıvırcık ayraçlarla çevreleyin.

```{[dbo].this_is my complex name$$$}```

## <a name="preview-expression-results"></a>İfade sonuçlarını önizleme

[Hata ayıklama modu](concepts-data-flow-debug-mode.md) açıksa, ifadenizin değerlendirdiğiniz şeyin devam eden önizlemesini görmek için canlı Spark kümesini kullanabilirsiniz. Mantığınızı oluştururken, ifadenizi gerçek zamanlı olarak hata ayıklayabilirsiniz. 

![Devam halindeönizleme](media/data-flow/exp4b.png "İfade Veri Önizleme")

İfadenizin sonuçlarını kaynağınızın canlı bir örneğine karşı güncelleştirmek için **Yenile'yi** seçin.

![Yenile düğmesi](media/data-flow/exp5.png "İfade Veri Önizleme")

## <a name="string-interpolation"></a>Dize ilişkilendirme

Gerçek dize metnini ifadelerle birleştirmek için tırnak işaretlerini kullanın. İfade işlevlerini, sütunları ve parametreleri ekleyebilirsiniz. Dize enterpolasyonu, parametreler sorgu dizelerine dahil edildiğinde dize eklemenin yaygın kullanımını önlemek için yararlıdır. İfade sözdizimini kullanmak için kıvırcık ayraçlara bünyesine

Dize enterpolasyonuna bazı örnekler:

* ```"My favorite movie is {iif(instr(title,', The')>0,"The {split(title,', The')[1]}",title)}"```

* ```"select * from {$tablename} where orderyear > {$year}"```

* ```"Total cost with sales tax is {round(totalcost * 1.08,2)}"```

## <a name="comment-expressions"></a>Açıklama ifadeleri

Tek satırlı ve çok satırlı yorum sözdizimini kullanarak ifadelerinize yorum ekleyin.

![Tek satırlı ve çok satırlı açıklama sözdizimi](media/data-flow/comments.png "Yorumlar")

Aşağıdaki örnekler geçerli yorumlardır:

* ```/* This is my comment */```

* ```/* This is a```
*   ```multi-line comment */```
   
* ```// This is a single line comment```

İfadenizin en üstüne bir açıklama koyarsanız, dönüşüm ifadelerinizi belgelemek için dönüşüm metin kutusunda görünür.

![Dönüşüm metin kutusunda açıklama](media/data-flow/comments2.png "Yorumlar")

## <a name="regular-expressions"></a>Normal ifadeler

Birçok ifade dili işlevleri düzenli ifade sözdizimini kullanır. Normal ifade işlevleri kullandığınızda, İfade Oluşturucu bir\\ters eğik çizgiyi ( ) kaçış karakter dizisi olarak yorumlamaya çalışır. Normal ifadenizde ters eğik çizgi kullandığınızda, tüm regex'i\`backticks () içine veya çift eğik çizgi kullanın.

Backticks kullanan bir örnek:

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

Çift eğik çizgi kullanan bir örnek:

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="address-array-indexes"></a>Adres dizi dizileri dizileri

Dizileri döndüren ifade işlevleriyle, dizi nesnelerinin içindeki belirli dizinleri gidermek için parantez ([]) kullanın. Dizi olanlara dayanır.

![İfade Oluşturucu dizisi](media/data-flow/expb2.png "İfade Veri Önizleme")

## <a name="keyboard-shortcuts"></a>Klavye kısayolları

* Ctrl+K Ctrl+C: Tüm satırı yorumla.
* Ctrl+K Ctrl+U: Yorumsuz.
* F1: Editör yardım komutları sağlayın.
* Alt+Aşağı ok tuşu: Geçerli satırı aşağı taşıyın.
* Alt+Yukarı ok tuşu: Geçerli satırı yukarı taşıyın.
* Ctrl+Spacebar: Bağlam yardımLarını göster.

## <a name="convert-to-dates-or-timestamps"></a>Tarihlere veya zaman damgalarına dönüştürme

Dize literallerini zaman damgası çıktınıza eklemek ```toString()```için dönüşümünüzü '' olarak sarın

```toString(toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss'), 'MM/dd /yyyy\'T\'HH:mm:ss')```

Milisaniyeleri çağdan bir tarihe veya zaman `toTimestamp(<number of milliseconds>)`damgasına dönüştürmek için . Eğer zaman saniyeler içinde geliyorsa, 1000 ile çarpın.

```toTimestamp(1574127407*1000l)```

Önceki ifadenin sonundaki "l" bitişi, satır altı sözdizimi olarak uzun bir türe dönüştürmeanlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Veri dönüştürme ifadeleri oluşturmaya başlayın](data-flow-expression-functions.md)
