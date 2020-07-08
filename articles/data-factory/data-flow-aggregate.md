---
title: Eşleme veri akışında toplama dönüştürmesi
description: Veri akışı toplama dönüştürmesiyle Azure Data Factory verileri ölçekte nasıl toplayacağınızı öğrenin.
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 03/24/2020
ms.openlocfilehash: 871f2b49e2dce9d762ef8a54923da04b0f24e4be
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81606526"
---
# <a name="aggregate-transformation-in-mapping-data-flow"></a>Eşleme veri akışında toplama dönüştürmesi

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Toplam dönüşümü, veri akışlarınızdan sütunların toplamalarının tanımlar. Ifade oluşturucuyu kullanarak, var olan veya hesaplanan sütunlara göre gruplanmış SUM, MIN, MAX ve COUNT gibi farklı toplamalar türlerini tanımlayabilirsiniz.

## <a name="group-by"></a>Gruplandırma ölçütü:

Toplama için bir Group by yan tümcesi olarak kullanılacak yeni bir hesaplanmış sütun oluşturun veya var olan bir sütunu seçin. Varolan bir sütunu kullanmak için açılan listeden seçin. Yeni bir hesaplanan sütun oluşturmak için, yan tümcesinin üzerine gelin ve **hesaplanan sütun**' a tıklayın. Bu, [veri akışı ifade Oluşturucusu](concepts-data-flow-expression-builder.md)'nu açar. Hesaplanan sütununuzu oluşturduktan sonra **ad** alanının altındaki çıkış sütunu adını girin. Ek bir Group by yan tümcesi eklemek istiyorsanız, var olan bir yan tümcenin üzerine gelin ve artı simgesine tıklayın.

![Ayarlara göre toplama dönüştürme grubu](media/data-flow/agg.png "Ayarlara göre toplama dönüştürme grubu")

Toplama dönüşümünde Group by yan tümcesi isteğe bağlıdır.

## <a name="aggregate-column"></a>Toplama sütunu 

Toplama ifadeleri oluşturmak için **Toplamlar** sekmesine gidin. Bir toplama ile var olan bir sütunun üzerine yazabilir ya da yeni bir adla yeni bir alan oluşturabilirsiniz. Toplama ifadesi, sütun adı seçicisinin yanındaki sağ kutuya girilir. İfadeyi düzenlemek için, metin kutusuna tıklayarak ifade oluşturucuyu açın. Ek toplamalar eklemek için mevcut bir ifadenin üzerine gelin ve artı simgesine tıklayarak yeni bir toplama sütunu veya [sütun stili](concepts-data-flow-column-pattern.md)oluşturun.

Her toplama ifadesi en az bir toplama işlevi içermelidir.

![Toplu dönüştürme toplama ayarları](media/data-flow/agg2.png "Toplu dönüştürme toplama ayarları")


> [!NOTE]
> Hata ayıklama modunda, ifade Oluşturucusu toplama işlevleriyle veri önizlemeleri üretemiyor. Toplama dönüştürmelerinin veri önizlemelerini görüntülemek için, ifade oluşturucuyu kapatın ve verileri ' veri önizleme ' sekmesi aracılığıyla görüntüleyin.

## <a name="reconnect-rows-and-columns"></a>Satırları ve sütunları yeniden bağla

Toplam dönüşümler SQL toplama seçme sorgularıyla benzerdir. Group by yan tümcesine veya toplama işlevlerine dahil olmayan sütunlar, toplu dönüşümünüzün çıkışına kadar akmaz. Toplanan çıkışındaki diğer sütunları eklemek istiyorsanız aşağıdaki yöntemlerden birini yapın:

* `last()` `first()` Bu ek sütunu eklemek için veya gibi bir toplama işlevi kullanın.
* [Kendi kendine JOIN modelini](https://mssqldude.wordpress.com/2018/12/20/adf-data-flows-self-join/)kullanarak sütunları çıkış akışla yeniden katın.

## <a name="removing-duplicate-rows"></a>Yinelenen satırları kaldırma

Toplama dönüşümünün ortak kullanımı, kaynak verilerde yinelenen girdileri kaldırıyor veya tanımlıyor. Bu işlem yinelenenleri kaldırma olarak bilinir. Anahtarlar tarafından bir grup grubuna göre, hangi yinelenen satırın tutulacağını belirlemek için seçtiğiniz bir buluşsal yöntemi kullanın. Ortak buluşsal yöntemler `first()` , `last()` , `max()` ve `min()` . Gruplandırma sütunları hariç her sütuna kuralı uygulamak için [sütun desenleri](concepts-data-flow-column-pattern.md) kullanın.

![Yinelenenleri kaldırma](media/data-flow/agg-dedupe.png "Yinelenenleri kaldırma")

Yukarıdaki örnekte, sütunlarında `ProductID` ve `Name` gruplandırmada kullanılır. İki satır bu iki sütun için aynı değere sahip ise, bunlar yinelenen olarak kabul edilir. Bu toplama dönüşümünde, eşleşen ilk satırın değerleri saklanır ve diğerleri bırakılır. Sütun deseninin sözdizimini kullanarak, adları olmayan `ProductID` ve `Name` var olan sütun adlarına eşlenmiş ve ilk eşleşen satırların değeri verilen tüm sütunlar. Çıktı şeması, giriş şeması ile aynıdır.

Veri doğrulama senaryolarında, `count()` işlevi kaç tane yineleme olduğunu saymak için kullanılabilir.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Syntax

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

Aşağıdaki örnek, gelen bir akışı alır `MoviesYear` ve satırları sütuna göre gruplandırır `year` . Dönüştürme, sütun ortalamasını değerlendiren bir toplama sütunu oluşturur `avgrating` `Rating` . Bu toplama dönüştürmesi olarak adlandırılmıştır `AvgComedyRatingsByYear` .

Data Factory UX 'de, bu dönüşüm aşağıdaki görüntüye benzer şekilde görünür:

![Örneğe göre Gruplandır](media/data-flow/agg-script1.png "Örneğe göre Gruplandır")

![Birleşik örnek](media/data-flow/agg-script2.png "Birleşik örnek")

Bu dönüşüm için veri akışı betiği aşağıdaki kod parçacığında bulunur.

```
MoviesYear aggregate(
                groupBy(year),
                avgrating = avg(toInteger(Rating))
            ) ~> AvgComedyRatingByYear
```

![Veri akışı betiği topla](media/data-flow/aggdfs1.png "Veri akışı betiği topla")

```MoviesYear```: Yıl ve Başlık sütunlarını tanımlayan türetilmiş sütun ```AvgComedyRatingByYear``` : yıla göre gruplanmış ortalama Comedies derecelendirmesi için toplam dönüşüm ```avgrating``` : toplanan değeri tutmak için oluşturulan yeni sütunun adı

```
MoviesYear aggregate(groupBy(year),
    avgrating = avg(toInteger(Rating))) ~> AvgComedyRatingByYear
```

## <a name="next-steps"></a>Sonraki adımlar

* Pencere [dönüşümünü](data-flow-window.md) kullanarak pencere tabanlı toplamayı tanımlama
