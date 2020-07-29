---
title: Eşleme veri akışında vekil anahtar dönüşümü
description: Azure Data Factory eşleme veri akışı yedek anahtar dönüşümü kullanarak sıralı anahtar değerleri oluşturma
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/08/2020
ms.openlocfilehash: ade2fd6011bbcdaed4ce31ce70bfb4235429bb0d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81606290"
---
# <a name="surrogate-key-transformation-in-mapping-data-flow"></a>Eşleme veri akışında vekil anahtar dönüşümü 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Her bir veri satırına bir artırma anahtarı değeri eklemek için vekil anahtar dönüşümünü kullanın. Bu, bir yıldız şeması analitik veri modelinde boyut tabloları tasarlarken yararlı olur. Bir yıldız şemasında, boyut tablolarınızdaki her üye, iş dışı bir anahtar olan benzersiz bir anahtar gerektirir.

## <a name="configuration"></a>Yapılandırma

![Vekil anahtar dönüşümü](media/data-flow/surrogate.png "Vekil anahtar dönüşümü")

**Anahtar sütun:** Oluşturulan vekil anahtar sütununun adı.

**Başlangıç değeri:** Oluşturulacak en düşük anahtar değeri.

## <a name="increment-keys-from-existing-sources"></a>Mevcut kaynaklardan anahtarları artırma

Bir kaynakta bulunan bir değerden dizinizi başlatmak için, yedek anahtar dönüşümünüzü takip eden bir türetilmiş sütun dönüşümü kullanarak iki değeri birlikte ekleyin:

![SK en fazla Ekle](media/data-flow/sk006.png "Vekil anahtar dönüştürme en fazla Ekle")

### <a name="increment-from-existing-maximum-value"></a>Varolan en büyük değerden artış

Anahtar değerini önceki maksimum ile temel almak için, kaynak verilerinizin bulunduğu yere göre kullanabileceğiniz iki teknik vardır.

#### <a name="database-sources"></a>Veritabanı kaynakları

Kaynağınızdan MAX () ' i seçmek için bir SQL sorgu seçeneği kullanın. Örneğin,`Select MAX(<surrogateKeyName>) as maxval from <sourceTable>`/

![Vekil anahtar sorgusu](media/data-flow/sk002.png "Vekil anahtar dönüştürme sorgusu")

#### <a name="file-sources"></a>Dosya kaynakları

Önceki en büyük değer bir dosya içinde ise, `max()` önceki en büyük değeri almak için toplu dönüşümde işlevini kullanın:

![Vekil anahtar dosyası](media/data-flow/sk008.png "Vekil anahtar dosyası")

Her iki durumda da, gelen yeni verilerinizi önceki maksimum değeri içeren kaynakla birlikte katılmanız gerekir.

![Vekil anahtar birleşimi](media/data-flow/sk004.png "Vekil anahtar birleşimi")

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Syntax

```
<incomingStream> 
    keyGenerate(
        output(<surrogateColumnName> as long),
        startAt: <number>L
    ) ~> <surrogateKeyTransformationName>
```

### <a name="example"></a>Örnek

![Vekil anahtar dönüşümü](media/data-flow/surrogate.png "Vekil anahtar dönüşümü")

Yukarıdaki yedek anahtar yapılandırması için veri akışı betiği aşağıdaki kod parçacığında bulunur.

```
AggregateDayStats
    keyGenerate(
        output(key as long),
        startAt: 1L
    ) ~> SurrogateKey1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekler, [JOIN](data-flow-join.md) ve [türetilmiş sütun](data-flow-derived-column.md) dönüşümlerini kullanır.
