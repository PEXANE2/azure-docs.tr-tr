---
title: Azure Data Factory eşleme veri akışında türetilmiş sütun dönüşümü | Microsoft Docs
description: Eşleme veri akışı türetilmiş sütun dönüşümüyle birlikte Azure Data Factory ölçekli verileri nasıl dönüştürebileceğinizi öğrenin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 60451fa6152590ed0fde51be436c867f39906acf
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72514822"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Eşleme veri akışında türetilmiş sütun dönüşümü

Veri akışınızda yeni sütunlar oluşturmak veya mevcut alanları değiştirmek için türetilmiş sütun dönüşümünü kullanın.

## <a name="derived-column-settings"></a>Türetilmiş sütun ayarları

Varolan bir sütunu geçersiz kılmak için sütun açılan listesinden seçin. Aksi takdirde, sütun seçim alanını bir metin kutusu olarak kullanın ve yeni sütunun adını yazın. Türetilmiş sütunun ifadesini oluşturmak için, ' ifadeyi girin ' kutusuna tıklayarak [veri akışı Ifade oluşturucuyu](concepts-data-flow-expression-builder.md)açın.

![Türetilmiş sütun ayarları](media/data-flow/dc1.png "Türetilmiş sütun ayarları")

Başka türetilmiş sütunlar eklemek için, varolan bir türetilmiş sütunun üzerine gelin ve artı simgesine tıklayın. **Sütun Ekle** veya **sütun Ekle düzenlerini**seçin. Sütun adları kaynaklarınızdan değişken ise, sütun desenleri yararlı olabilir. Daha fazla bilgi için bkz. [sütun desenleri](concepts-data-flow-column-pattern.md).

![Yeni türetilmiş sütun seçimi](media/data-flow/columnpattern.png "Yeni türetilmiş sütun seçimi")

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

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

Aşağıdaki örnek, `CleanData` adlı ve gelen akış `MoviesYear` ' i alan ve iki türetilmiş sütun oluşturan bir türetilmiş sütundur. İlk türetilmiş sütun, `Rating` sütununun değerini, derecelendirme değeri olan bir tamsayı türü olarak değiştirir. İkinci türetilmiş sütun, adı ' Filmler ' ile başlayan her sütunla eşleşen bir modeldir. Eşleşen her sütun için, ' movie_ ' önekli eşleşen sütunun değerine eşit olan bir sütun `movie` oluşturur. 

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
