---
title: Veri akışını haritalamada toplu dönüşüm
description: Haritalama veri akışı Toplu dönüştürme ile Azure Veri Fabrikası'nda ölçekte veri toplama yı öğrenin.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/24/2020
ms.openlocfilehash: 871f2b49e2dce9d762ef8a54923da04b0f24e4be
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606526"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Veri akışını haritalamada toplu dönüşüm

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Toplu dönüştürme, veri akışlarınızdaki sütun toplamalarını tanımlar. İfade Oluşturucusu'nu kullanarak, varolan veya hesaplanan sütunlara göre gruplanan SUM, MIN, MAX ve COUNT gibi farklı türde toplamaları tanımlayabilirsiniz.

## <a name="group-by"></a>Gruplandırma ölçütü:

Varolan bir sütun seçin veya toplama nız için grup olarak kullanmak üzere yeni bir işlem sütunu oluşturun. Varolan bir sütunu kullanmak için açılır dosyadan seçin. Yeni bir hesaplanmış sütun oluşturmak için, yan tümcenin üzerine gidin ve **Hesaplanan sütunu**tıklatın. Bu [veri akışı ifade oluşturucu](concepts-data-flow-expression-builder.md)açar. Hesaplanan sütununuzu oluşturduktan sonra, alan olarak **Ad'ın** altındaki çıktı sütun adını girin. Yan tümceye göre ek bir grup eklemek isterseniz, varolan bir yan tümcenin üzerine tıklayın ve artı simgesini tıklatın.

![Ayarlara göre toplu dönüşüm grubu](media/data-flow/agg.png "Ayarlara göre toplu dönüşüm grubu")

Bir grup yan tümcesi bir Toplu dönüştürme isteğe bağlıdır.

## <a name="aggregate-column"></a>Toplam sütun 

Toplama ifadeleri oluşturmak için **Toplamlar** sekmesine gidin. Varolan bir sütunun üzerine toplama içeren bir sütunun üzerine yazabilir veya yeni bir ada sahip yeni bir alan oluşturabilirsiniz. Toplama ifadesi, sütun adı seçicinin yanındaki sağ kutuya girilir. İfadeyi yeniden yapmak için, ifade oluşturucuyu açmak için metin kutusunu tıklatın. Ek toplamalar eklemek için, varolan bir ifadenin üzerine gidip yeni bir toplama sütunu veya [sütun deseni](concepts-data-flow-column-pattern.md)oluşturmak için artı simgesini tıklatın.

Her toplama ifadesi en az bir toplama işlevi içermelidir.

![Toplam dönüşüm toplamı ayarları](media/data-flow/agg2.png "Toplam dönüşüm toplamı ayarları")


> [!NOTE]
> Hata Ayıklama modunda, ifade oluşturucu toplu işlevleri olan veri önizlemeleri üretemez. Toplu dönüşümler için veri önizlemelerini görüntülemek için ifade oluşturucuyu kapatın ve 'Veri Önizlemesi' sekmesi aracılığıyla verileri görüntüleyin.

## <a name="reconnect-rows-and-columns"></a>Satırları ve sütunları yeniden bağlama

Toplu dönüşümler, SQL toplu seçme sorgularına benzer. Yan tümce veya toplu işlevlere göre grubunuza dahil olmayan sütunlar, toplu dönüşümün çıktısına akmaz. Birleştirilmiş çıktınıza diğer sütunları eklemek istiyorsanız, aşağıdaki yöntemlerden birini yapın:

* Bu ek sütun `last()` gibi `first()` bir toplu işlev kullanın.
* [Kendi kendine birleştirme deseni](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/)kullanarak sütunları çıktı akışınıza yeniden birleştirin.

## <a name="removing-duplicate-rows"></a>Yinelenen satırları kaldırma

Toplu dönüştürmenin yaygın kullanımı, kaynak verilerdeki yinelenen girişleri kaldırmak veya tanımlamaktır. Bu işlem çoğaltma olarak bilinir. Anahtarlara göre bir grup kümesine bağlı olarak, hangi yinelenen satırı tutacağınızı belirlemek için seçtiğiniz buluşsal bir buluşsal kullanın. Yaygın buluşsal olanlar `first()` `last()`, `max()`, `min()`, ve . Sütunlara göre grup dışında her sütuna kuralı uygulamak için [sütun desenlerini](concepts-data-flow-column-pattern.md) kullanın.

![Yinelenenleri kaldırma](media/data-flow/agg-dedupe.png "Yinelenenleri kaldırma")

Yukarıdaki örnekte, `ProductID` sütunlar ve `Name` gruplandırma için kullanılmaktadır. İki satır, bu iki sütun için aynı değerlere sahipse, bunlar yinelenenler olarak kabul edilir. Bu toplu dönüştürmede, eşleşen ilk satırın değerleri tutulur ve diğerleri bırakılır. Sütun deseni sözdizimini kullanarak, adları `ProductID` `Name` varolan sütun adlarına eşlenen ve ilk eşleşen satırların değeri verilen tüm sütunlar. Çıkış şeması giriş şeması ile aynıdır.

Veri doğrulama senaryoları için `count()` işlev, kaç yinelenen olduğunu saymak için kullanılabilir.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<incomingStream>
    aggregate(
           groupBy(
                <groupByColumnName> = <groupByExpression1>,
                <groupByExpression2>
               ),
           <aggregateColumn1> = <aggregateExpression1>,
           <aggregateColumn2> = <aggregateExpression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <aggregateTransformationName>
```

### <a name="example"></a>Örnek

Aşağıdaki örnek, gelen akışı `MoviesYear` alır ve satırları `year`sütuna göre grupla. Dönüştürme, sütun `Rating`ortalamasına `avgrating` göre değerlendiren bir toplu sütun oluşturur. Bu toplu dönüştürme `AvgComedyRatingsByYear`adı .

Veri Fabrikası UX,bu dönüşüm aşağıdaki resim gibi görünür:

![Örneğe göre gruplandırma](media/data-flow/agg-script1.png "Örneğe göre gruplandırma")

![Toplu örnek](media/data-flow/agg-script2.png "Toplu örnek")

Bu dönüşüm için veri akışı komut dosyası aşağıdaki snippet bulunmaktadır.

```
MoviesYear aggregate(
                groupBy(year),
                avgrating = avg(toInteger(Rating))
            ) ~> AvgComedyRatingByYear
```

![Toplam veri akışı komut dosyası](media/data-flow/aggdfs1.png "Toplam veri akışı komut dosyası")

```MoviesYear```: Türemiş Sütun tanımlayan ```AvgComedyRatingByYear```yıl ve başlık sütunları : Yıla ```avgrating```göre gruplanan komedilerin ortalama derecelendirmesi için toplu dönüştürme : Toplanan değeri tutmak için oluşturulan yeni sütunun adı

```
MoviesYear aggregate(groupBy(year),
    avgrating = avg(toInteger(Rating))) ~> AvgComedyRatingByYear
```

## <a name="next-steps"></a>Sonraki adımlar

* [Pencere dönüşümlerini](data-flow-window.md) kullanarak pencere tabanlı toplamayı tanımlama
