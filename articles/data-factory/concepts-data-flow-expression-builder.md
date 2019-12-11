---
title: Eşleme veri akışı 'nda ifade Oluşturucu
description: Azure Data Factory veri akışlarını eşleme bölümünde ifade oluşturucuyu kullanarak ifadeler oluşturun
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 12/9/2019
ms.openlocfilehash: 01aa2574ac6edd1ce5e1b209eac3e43bbed82fce
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74969404"
---
# <a name="building-expressions-in-mapping-data-flow"></a>Eşleme veri akışında ifadeler oluşturma

Eşleme veri akışı ' nda birçok dönüştürme özelliği ifadeler olarak girilir. Bu ifadeler, çalışma zamanında Spark veri türü sonucunu değerlendiren sütun değerleri, parametreler, işlevler, işleçler ve değişmez değerlerdir.

## <a name="opening-the-expression-builder"></a>İfade oluşturucuyu açma

Data Factory UX içindeki ifade düzenlemesi arabirimi **Ifade Oluşturucusu**olarak bilinir. İfade mantığınızı girerken, Data Factory, vurgulama, sözdizimi denetimi ve otomatik tamamlama için [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense?view=vs-2019) kod tamamlamayı kullanır.

![İfade Oluşturucusu](media/data-flow/xpb1.png "İfade Oluşturucusu")

Türetilmiş sütun ve filtre gibi dönüşümlerde, ifadelerin zorunlu olduğu durumlarda, mavi ifade kutusuna tıklayarak ifade oluşturucuyu açın.

![İfade Oluşturucusu](media/data-flow/expressionbox.png "İfade Oluşturucusu")

Koşula göre eşleşen veya gruptaki sütunlara başvurulduğunda bir ifade, sütunlardaki değerleri ayıklayabilir. Bir ifade oluşturmak için ' hesaplanan sütun ' seçeneğini belirleyin.

![İfade Oluşturucusu](media/data-flow/computedcolumn.png "İfade Oluşturucusu")

Bir ifadenin veya değişmez değerin geçerli girişler olduğu durumlarda, ' dinamik içerik Ekle ' değeri, bir hazır değer değerlendiren bir ifade oluşturmanıza izin verir.

![İfade Oluşturucusu](media/data-flow/add-dynamic-content.png "İfade Oluşturucusu")

## <a name="expression-language-reference"></a>İfade dili başvurusu

Veri akışları eşleme, ifadelerde kullanılabilecek yerleşik işlevlere ve işleçlere sahiptir. Kullanılabilir işlevlerin listesi, [veri akışı ifade dili başvurusunu eşleme](data-flow-expression-functions.md) sayfasında bulunur.

## <a name="column-names-with-special-characters"></a>Özel karakterlerle sütun adları

Özel karakterler veya boşluk içeren sütun adlarınız varsa, bu adı bir ifadede başvurmak için küme ayraçları ile çevreleyin.

```{[dbo].this_is my complex name$$$}```

## <a name="previewing-expression-results"></a>İfade sonuçlarının önizlemesi

[Hata ayıklama modu](concepts-data-flow-debug-mode.md) açık ise, ifadenizde ne kadar değerlendirdiği hakkında devam eden bir önizleme görmek için Live Spark kümesini kullanabilirsiniz. Mantığınızı oluştururken, ifadenizde gerçek zamanlı olarak hata ayıklaması yapabilirsiniz. 

![İfade Oluşturucusu](media/data-flow/exp4b.png "İfade verileri önizlemesi")

Deyimizin sonuçlarını kaynağınızın canlı bir örneğine göre güncelleştirmek için Yenile düğmesine tıklayın.

![İfade Oluşturucusu](media/data-flow/exp5.png "İfade verileri önizlemesi")

## <a name="string-interpolation"></a>Dize ilişkilendirme

Değişmez dize metnini ifadelerle birlikte tırnak içine almak için çift tırnak işareti kullanın. İfade işlevleri, sütunlar ve parametreler ekleyebilirsiniz. Dize ilişkilendirme, sorgu dizelerine parametreler dahil edildiğinde dize bitişinin kapsamlı kullanımını önlemek için yararlıdır. İfade sözdizimini kullanmak için, küme ayraçları içine alın

Dize ilişkilendirme için bazı örnekler:

* ```"My favorite movie is {iif(instr(title,', The')>0,"The {split(title,', The')[1]}",title)}"```

* ```"select * from {$tablename} where orderyear > {$year}"```

* ```"Total cost with sales tax is {round(totalcost * 1.08,2)}"```

## <a name="commenting-expressions"></a>Açıklama ifadeleri

Tek satırlı ve çok satırlı açıklama söz dizimini kullanarak ifadelerinizi açıklama ekleyin:

![Açıklamalar](media/data-flow/comments.png "Yorumlar")

Aşağıda geçerli açıklamaların örnekleri verilmiştir:

* ```/* This is my comment */```

* ```/* This is a```
*   ```multi-line comment */```
   
* ```// This is a single line comment```

İfadeniz üzerine bir yorum koyarsanız, dönüştürme ifadelerinizi belgelemek için dönüşüm metin kutusunda görünür:

![Açıklamalar](media/data-flow/comments2.png "Yorumlar")

## <a name="regular-expressions"></a>Normal ifadeler

Birçok ifade dili işlevi normal ifade söz dizimini kullanır. Normal ifade işlevleri kullanılırken, Ifade Oluşturucusu ters eğik çizgiyi (\\) bir kaçış karakter sırası olarak yorumlamaya çalışır. Normal ifadenizde ters eğik çizgi kullandığınızda, tüm Regex ' ı (\`) içine alın ya da çift ters eğik çizgi kullanın.

Tick kullanarak örnek

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

veya çift eğik çizgi kullanma

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="addressing-array-indexes"></a>Dizi dizinlerini adresleme

Dizi döndüren ifade işlevleri ile, dönüş dizisi nesnesinin içindeki belirli dizinleri adresleyen köşeli parantezleri [] kullanın. Dizi, tabanlıdır.

![İfade Oluşturucu dizisi](media/data-flow/expb2.png "İfade verileri önizlemesi")

## <a name="keyboard-shortcuts"></a>Klavye kısayolları

* ```Ctrl-K Ctrl-C```: tüm satır açıklamaları
* ```Ctrl-K Ctrl-U```: Açıklama
* ```F1```: Düzenleyici Yardım komutlarını sağlama
* ```Alt-Down Arrow```: geçerli satırı aşağı taşı
* ```Alt-Up Arrow```: geçerli satırı yukarı taşı
* ```Cntrl-Space```: bağlam yardımını göster

## <a name="convert-to-dates-or-timestamps"></a>Tarihlere veya zaman damgalarına Dönüştür

Zaman damgası çıkışındaki dize sabit değerlerini dahil etmek için, ```toString()```dönüştürme kodunuzu sarmalıdır.

```toString(toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss'), 'MM/dd /yyyy\'T\'HH:mm:ss')```

Süreyi dönem 'den bir tarih veya zaman damgasına dönüştürmek için `toTimestamp(<number of milliseconds>)`kullanın. Süre saniyeler içinde geliyorsa 1000 ile çarpın.

```toTimestamp(1574127407*1000l)```

Yukarıdaki ifadenin sonundaki "l" sonunda, satır içi sözdizimi olarak uzun bir türe dönüştürme anlamına gelir.

## <a name="next-steps"></a>Sonraki adımlar

[Veri dönüştürme ifadeleri oluşturmaya başla](data-flow-expression-functions.md)
