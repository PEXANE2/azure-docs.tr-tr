---
title: Veri akışını eşlemede türemiş sütun dönüşümü
description: Veri akışı Türetilmiş Sütun dönüşümeşle azure veri fabrikasında ölçekte verileri nasıl dönüştüreceklerini öğrenin.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/15/2019
ms.openlocfilehash: 06cb868071612ae2825e86cac32734dcd279c99f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81413857"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Veri akışını eşlemede türemiş sütun dönüşümü

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Veri akışınızda yeni sütunlar oluşturmak veya varolan alanları değiştirmek için türetilen sütun dönüşümunu kullanın.

## <a name="derived-column-settings"></a>Türetilmiş sütun ayarları

Varolan bir sütunu geçersiz kılmak için sütun açılır yoluyla sütunu seçin. Aksi takdirde, sütun seçim alanını textbox olarak kullanın ve yeni sütunun adını yazın. Türetilen sütunun ifadesini oluşturmak için, [Veri Akışı İfade oluşturucusu](concepts-data-flow-expression-builder.md)açmak için 'İfade gir' kutusunu tıklatın.

![Türetilmiş sütun ayarları](media/data-flow/dc1.png "Türetilmiş sütun ayarları")

Türetilen sütunlar eklemek için varolan bir sütunun üzerine dokunun ve artı simgesini tıklatın. Sütun **ekle** veya sütun **deseni ekle'yi**seçin. Sütun adlarınız kaynaklarınızdan değişkense sütun desenleri kullanışlı olabilir. Daha fazla bilgi için [sütun desenleri'ne](concepts-data-flow-column-pattern.md)bakın.

![Yeni türetilmiş sütun seçimi](media/data-flow/columnpattern.png "Yeni türetilmiş sütun seçimi")

## <a name="build-schemas-in-output-schema-pane"></a>Output Schema bölmesinde şema lar oluşturun

Şemanıza eklediğiniz ve eklediğiniz sütunlar Output Schema bölmesinde listelenir. Burada basit ve karmaşık veri yapıları oluşturabilirsiniz. Ek alanlar eklemek için **sütun ekle'yi**seçin. Hiyerarşioluşturmak için **alt sütun ekle'yi**seçin.

![Alt sütun ekleme](media/data-flow/addsubcolumn.png "Alt Sütun Ekle")

Veri akışında karmaşık türleri işleme hakkında daha fazla bilgi için, [veri akışını eşlemede JSON işleme'ye](format-json.md#mapping-data-flow-properties)bakın.

![Karmaşık sütun ekleme](media/data-flow/complexcolumn.png "Sütun ekleme")

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

Aşağıdaki örnek, gelen akışı `CleanData` alan `MoviesYear` ve iki türetilmiş sütun oluşturan türemiş bir sütundur. İlk türetilmiş sütun, noter türü olarak Derecelendirme değeriyle sütun `Rating` değiştirir. İkinci türetilmiş sütun, adı 'filmler' ile başlayan her sütunla eşleşen bir desendir. Eşleşen her sütun için, 'movie_' ile önceden belirlenmiş eşleşen sütunun değerine eşit bir sütun `movie` oluşturur. 

Veri Fabrikası UX,bu dönüşüm aşağıdaki resim gibi görünür:

![Türetme örneği](media/data-flow/derive-script1.png "Türetme örneği")

Bu dönüşüm için veri akışı komut dosyası aşağıdaki snippet bulunmaktadır:

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

- Veri Akışı [İfade dilieşleme](data-flow-expression-functions.md)hakkında daha fazla bilgi edinin.
