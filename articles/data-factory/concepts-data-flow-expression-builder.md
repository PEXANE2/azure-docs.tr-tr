---
title: Eşleme veri akışı 'nda ifade Oluşturucu
description: Azure Data Factory veri akışlarını eşleme bölümünde Ifade Oluşturucu kullanarak ifadeler oluşturun
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 06/20/2020
ms.openlocfilehash: 7e2b655b344af90c4555beb0af85fa11cbc6d1c8
ms.sourcegitcommit: 398fecceba133d90aa8f6f1f2af58899f613d1e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2020
ms.locfileid: "85126170"
---
# <a name="build-expressions-in-mapping-data-flow"></a>Eşleme veri akışında derleme ifadeleri

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Eşleme veri akışı ' nda birçok dönüştürme özelliği ifadeler olarak girilir. Bu ifadeler, çalışma zamanında Spark veri türü sonucunu değerlendiren sütun değerleri, parametreler, işlevler, işleçler ve değişmez değerlerdir.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4tkur]

## <a name="open-expression-builder"></a>Ifade Oluşturucuyu Aç

Azure Data Factory kullanıcı deneyiminde ifade düzenlemesi arabirimi Ifade Oluşturucusu olarak bilinir. İfade mantığınızı girerken, Data Factory vurgulama, sözdizimi denetimi ve otomatik tamamlama için [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense?view=vs-2019) kod tamamlamayı kullanır.

![İfade Oluşturucusu](media/data-flow/xpb1.png "İfade Oluşturucusu")

Türetilmiş sütun ve filtre gibi dönüşümlerde, ifadelerin zorunlu olduğu, mavi ifade kutusunu seçerek Ifade oluşturucuyu açın.

![Mavi ifade kutusu](media/data-flow/expressionbox.png "İfade Oluşturucusu")

Bir eşleşen veya gruplandırma koşulunda sütunlara başvuru yaptığınızda bir ifade, sütunlardaki değerleri ayıklayabilir. Bir ifade oluşturmak için, **hesaplanan sütun**' u seçin.

![Hesaplanan sütun seçeneği](media/data-flow/computedcolumn.png "İfade Oluşturucusu")

Bir ifadenin veya değişmez değerin geçerli girişler olduğu durumlarda, değişmez değer olarak değerlendirilen bir ifade oluşturmak için **dinamik Içerik Ekle** ' yi seçin.

![Dinamik içerik Ekle seçeneği](media/data-flow/add-dynamic-content.png "İfade Oluşturucusu")

## <a name="expression-language-reference"></a>İfade dili başvurusu

Veri akışları eşleme, ifadelerde kullanılabilecek yerleşik işlevlere ve işleçlere sahiptir. Kullanılabilir işlevlerin listesi için, bkz. [eşleme veri akışındaki ifade işlevleri](data-flow-expression-functions.md).

## <a name="column-names-with-special-characters"></a>Özel karakterlerle sütun adları

Özel karakterler veya boşluk içeren sütun adlarınız varsa, bu adı bir ifadede başvurmak için küme ayraçları ile çevreleyin.

```{[dbo].this_is my complex name$$$}```

## <a name="preview-expression-results"></a>Önizleme ifadesi sonuçları

[Hata ayıklama modu](concepts-data-flow-debug-mode.md) açık ise, ifadenizde ne kadar değerlendirdiği hakkında devam eden bir önizleme görmek Için Live Spark kümesini kullanabilirsiniz. Mantığınızı oluştururken, ifadenizde gerçek zamanlı olarak hata ayıklaması yapabilirsiniz. 

![Devam eden Önizleme](media/data-flow/exp4b.png "İfade verileri önizlemesi")

Deyimizin sonuçlarını kaynağınızın canlı bir örneğine göre güncelleştirmek için **Yenile** ' yi seçin.

![Yenile düğmesi](media/data-flow/exp5.png "İfade verileri önizlemesi")

## <a name="string-interpolation"></a>Dize ilişkilendirme

Değişmez dize metnini ifadelerle birlikte kapsamak için tırnak işaretleri kullanın. İfade işlevleri, sütunlar ve parametreler ekleyebilirsiniz. Dize ilişkilendirme, parametreler sorgu dizelerine dahil edildiğinde dize bitişinin kapsamlı kullanımını önlemek için yararlıdır. İfade sözdizimini kullanmak için, küme ayraçları içine alın

Dize ilişkilendirme için bazı örnekler:

* ```"My favorite movie is {iif(instr(title,', The')>0,"The {split(title,', The')[1]}",title)}"```

* ```"select * from {$tablename} where orderyear > {$year}"```

* ```"Total cost with sales tax is {round(totalcost * 1.08,2)}"```

## <a name="comment-expressions"></a>Açıklama ifadeleri

Tek satırlı ve çok satırlı açıklama söz dizimini kullanarak deyimlerinizi açıklama ekleyin.

Aşağıdaki örnekler geçerli açıklamalardır:

* ```/* This is my comment */```

* ```/* This is a```
*   ```multi-line comment */```

İfadeniz üzerine bir yorum koyarsanız, dönüştürme ifadelerinizi belgelemek için dönüşüm metin kutusunda görünür.

![Dönüştürme metin kutusunda açıklama](media/data-flow/comments2.png "Yorumlar")

## <a name="regular-expressions"></a>Normal ifadeler

Birçok ifade dili işlevi normal ifade söz dizimini kullanır. Normal ifade işlevleri kullandığınızda, Ifade Oluşturucusu bir ters eğik çizgi ( \\ ) karakterini bir kaçış karakter sırası olarak yorumlamaya çalışır. Normal ifadenizde ters eğik çizgi kullandığınızda, tüm Regex geri işaretleri () içine alın ya da \` çift ters eğik çizgi kullanın.

Geri işaretleri kullanan bir örnek:

```
regex_replace('100 and 200', `(\d+)`, 'digits')
```

Çift eğik çizgi kullanan bir örnek:

```
regex_replace('100 and 200', '(\\d+)', 'digits')
```

## <a name="address-array-indexes"></a>Adres dizisi dizinleri

Dizileri döndüren ifade işlevleri ile, döndürülen dizi nesneleri içindeki belirli dizinleri adresleyen köşeli ayraç ([]) kullanın. Dizi, bunları temel alır.

![İfade Oluşturucu dizisi](media/data-flow/expb2.png "İfade verileri önizlemesi")

## <a name="keyboard-shortcuts"></a>Klavye kısayolları

* CTRL + K CTRL + C: tüm satırı açıklama.
* CTRL + K Ctrl + U: Açıklama kaldır.
* F1: Düzenleyici Yardım komutları sağlar.
* Alt + aşağı ok tuşu: geçerli satırı aşağı taşı.
* Alt + yukarı ok tuşu: geçerli satırı yukarı taşı.
* CTRL + Ara çubuğu: bağlam yardımını göster.

## <a name="convert-to-dates-or-timestamps"></a>Tarihlere veya zaman damgalarına Dönüştür

Zaman damgası çıkışındaki dize sabit değerlerini dahil etmek için, dönüştürmeyi ' de sarın ```toString()``` .

```toString(toTimestamp('12/31/2016T00:12:00', 'MM/dd/yyyy\'T\'HH:mm:ss'), 'MM/dd /yyyy\'T\'HH:mm:ss')```

Süreyi dönem 'den bir tarih veya zaman damgasına dönüştürmek için kullanın `toTimestamp(<number of milliseconds>)` . Süre saniyeler içinde geliyorsa 1.000 ile çarpın.

```toTimestamp(1574127407*1000l)```

Önceki ifadenin sonundaki "l" ifadesi, uzun bir tür için satır içi sözdizimi olarak dönüştürmeyi belirtir.

## <a name="find-time-from-epoch-or-unix-time"></a>Dönem veya Unix saatinden zaman bulma

toLong (currentTimestamp ()-toTimestamp (' 1970-01-01 00:00:00.000 ', ' yyyy-AA-GG SS: DD: ss. SSS ')) * 1000L

## <a name="next-steps"></a>Sonraki adımlar

[Veri dönüştürme ifadeleri oluşturmaya başla](data-flow-expression-functions.md)
