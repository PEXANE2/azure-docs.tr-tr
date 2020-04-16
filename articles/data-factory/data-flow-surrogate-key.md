---
title: Veri akışını nakışlamada vekil anahtar dönüşümü
description: Sıralı anahtar değerleri oluşturmak için Azure Veri Fabrikası'nın haritalama veri akışı Surrogate Anahtar Dönüşümü nasıl kullanılır?
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/08/2020
ms.openlocfilehash: 6f0310f13ab23b5b3d4fba649d0faf0da4dad5b8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81413152"
---
# <a name="surrogate-key-transformation-in-mapping-data-flow"></a>Veri akışını nakışlamada vekil anahtar dönüşümü 

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Her veri satırına artışlı anahtar değeri eklemek için vekil anahtar dönüşümünün kullanın. Bu, yıldız şema analitik veri modelinde boyut tabloları tasarlarken yararlıdır. Bir yıldız şemasında, boyut tablolarınızdaki her üye, iş yeri olmayan bir anahtar olan benzersiz bir anahtar gerektirir.

## <a name="configuration"></a>Yapılandırma

![Vekil Anahtar Dönüşümü](media/data-flow/surrogate.png "Vekil Anahtar Dönüşümü")

**Anahtar sütun:** Oluşturulan vekil anahtar sütununun adı.

**Başlangıç değeri:** Oluşturulacak en düşük anahtar değeri.

## <a name="increment-keys-from-existing-sources"></a>Varolan kaynaklardan artan anahtarlar

Dizinizi kaynakta bulunan bir değerden başlatmak için, iki değeri bir araya getirmek için vekil anahtar dönüşümünüzü izleyen türetilmiş sütun dönüşümlerini kullanın:

![SK Max eklemek](media/data-flow/sk006.png "Surrogate Anahtar Dönüşümü Max ekle")

### <a name="increment-from-existing-maximum-value"></a>Varolan maksimum değerden artış

Anahtar değerini önceki max ile tohumlamak için, kaynak verilerinizin nerede olduğuna bağlı olarak kullanabileceğiniz iki teknik vardır.

#### <a name="database-sources"></a>Veritabanı kaynakları

Kaynağınızdan MAX() seçeneğini seçmek için bir SQL sorgusu seçeneği kullanın. Örneğin,`Select MAX(<surrogateKeyName>) as maxval from <sourceTable>`/

![Vekil Anahtar Sorgusu](media/data-flow/sk002.png "Vekil Anahtar Dönüşüm Sorgusu")

#### <a name="file-sources"></a>Dosya kaynakları

Önceki maksimum değeriniz bir dosyadaysa, önceki maksimum değeri almak için toplu dönüştürme işlevini kullanın: `max()`

![Vekil Anahtar Dosyası](media/data-flow/sk008.png "Vekil Anahtar Dosyası")

Her iki durumda da, gelen yeni verilerinizi önceki maksimum değeri içeren kaynağınızla birlikte birleştirmeniz gerekir.

![Vekil Anahtar Birleştirme](media/data-flow/sk004.png "Vekil Anahtar Birleştirme")

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<incomingStream> 
    keyGenerate(
        output(<surrogateColumnName> as long),
        startAt: <number>L
    ) ~> <surrogateKeyTransformationName>
```

### <a name="example"></a>Örnek

![Vekil Anahtar Dönüşümü](media/data-flow/surrogate.png "Vekil Anahtar Dönüşümü")

Yukarıdaki vekil anahtar yapılandırması için veri akışı komut dosyası aşağıdaki kod snippet bulunmaktadır.

```
AggregateDayStats
    keyGenerate(
        output(key as long),
        startAt: 1L
    ) ~> SurrogateKey1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekler, [Birleştirme](data-flow-join.md) ve [Türemiş Sütun](data-flow-derived-column.md) dönüşümlerini kullanır.
