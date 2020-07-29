---
title: Eşleme veri akışında türetilmiş sütun dönüşümü
description: Eşleme veri akışı türetilmiş sütun dönüşümüyle birlikte Azure Data Factory ölçekli verileri nasıl dönüştürebileceğinizi öğrenin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/15/2019
ms.openlocfilehash: 38ec2d4619f47bf9fc4d1815cb6e9990cef72dcf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81606495"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Eşleme veri akışında türetilmiş sütun dönüşümü

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Veri akışınızda yeni sütunlar oluşturmak veya mevcut alanları değiştirmek için türetilmiş sütun dönüşümünü kullanın.

## <a name="derived-column-settings"></a>Türetilmiş sütun ayarları

Varolan bir sütunu geçersiz kılmak için sütun açılan listesinden seçin. Aksi takdirde, sütun seçim alanını bir metin kutusu olarak kullanın ve yeni sütunun adını yazın. Türetilmiş sütunun ifadesini oluşturmak için, ' ifadeyi girin ' kutusuna tıklayarak [veri akışı Ifade oluşturucuyu](concepts-data-flow-expression-builder.md)açın.

![Türetilmiş sütun ayarları](media/data-flow/dc1.png "Türetilmiş sütun ayarları")

Başka türetilmiş sütunlar eklemek için, varolan bir türetilmiş sütunun üzerine gelin ve artı simgesine tıklayın. **Sütun Ekle** veya **sütun Ekle düzenlerini**seçin. Sütun adları kaynaklarınızdan değişken ise, sütun desenleri yararlı olabilir. Daha fazla bilgi için bkz. [sütun desenleri](concepts-data-flow-column-pattern.md).

![Yeni türetilmiş sütun seçimi](media/data-flow/columnpattern.png "Yeni türetilmiş sütun seçimi")

## <a name="build-schemas-in-output-schema-pane"></a>Çıktı şeması bölmesinde şema oluşturma

Değiştirmekte olduğunuz ve şemanıza eklemekte olduğunuz sütunlar çıktı şeması bölmesinde listelenir. Etkileşimli olarak basit ve karmaşık veri yapıları oluşturabilirsiniz. Ek alanlar eklemek için **sütun Ekle**' yi seçin. Hiyerarşileri derlemek için, **alt sütun Ekle**' yi seçin.

![Alt sütun Ekle](media/data-flow/addsubcolumn.png "Alt sütun Ekle")

Veri akışındaki karmaşık türleri işleme hakkında daha fazla bilgi için bkz. [eşleme veri akışında JSON işleme](format-json.md#mapping-data-flow-properties).

![Karmaşık sütun Ekle](media/data-flow/complexcolumn.png "Sütun ekleme")

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Syntax

```
<incomingStream>
    derive(
           <columnName1> = <expression1>,
           <columnName2> = <expression2>,
           each(
                match(matchExpression),
                <metadataColumn1> = <metadataExpression1>,
                <metadataColumn2> = <metadataExpression2>
               )
          ) ~> <deriveTransformationName>
```

### <a name="example"></a>Örnek

Aşağıdaki örnek, `CleanData` gelen akış alan `MoviesYear` ve iki türetilmiş sütun oluşturan adlı bir türetilmiş sütundur. İlk türetilmiş sütun, sütunun `Rating` , derecelendirme değeri olan sütunu bir tamsayı türü olarak değiştirir. İkinci türetilmiş sütun, adı ' Filmler ' ile başlayan her sütunla eşleşen bir modeldir. Eşleşen her sütun için, `movie` ' movie_ ' önekli eşleşen sütunun değerine eşit olan bir sütun oluşturur. 

Data Factory UX 'de, bu dönüşüm aşağıdaki görüntüye benzer şekilde görünür:

![Örnek türet](media/data-flow/derive-script1.png "Örnek türet")

Bu dönüşüm için veri akışı betiği aşağıdaki kod parçacığında verilmiştir:

```
MoviesYear derive(
                Rating = toInteger(Rating),
                each(
                    match(startsWith(name,'movies')),
                    'movie' = 'movie_' + toString($$)
                )
            ) ~> CleanData
```

## <a name="next-steps"></a>Sonraki adımlar

- [Eşleme veri akışı ifade dili](data-flow-expression-functions.md)hakkında daha fazla bilgi edinin.
