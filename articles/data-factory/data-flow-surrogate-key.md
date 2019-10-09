---
title: Azure Data Factory eşleme veri akışı yedek anahtar dönüştürmesi
description: Azure Data Factory eşleme veri akışı yedek anahtar dönüşümü kullanarak sıralı anahtar değerleri oluşturma
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 45e2d35a3b0a3f3c89913bbe70d7c43c17cbcee0
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72029184"
---
# <a name="mapping-data-flow-surrogate-key-transformation"></a>Veri akışı yedek anahtar dönüşümünü eşleme



Veri akışı satır kümesine artan iş dışı bir anahtar değeri eklemek için vekil anahtar dönüşümünü kullanın. Bu, boyut tablolarınızdaki her üyenin, Kıbol DW metodolojisinin bir parçası olan iş dışı bir anahtar olan benzersiz bir anahtara sahip olması gereken bir yıldız şeması analitik veri modelinde, boyut tabloları tasarlarken yararlıdır.

![Vekil anahtar dönüştürme](media/data-flow/surrogate.png "yedek anahtar dönüşümü")

"Anahtar sütun", yeni yedek anahtar sütununuzu verdiğiniz addır.

"Başlangıç değeri", artımlı değerin başlangıç noktasıdır.

## <a name="increment-keys-from-existing-sources"></a>Mevcut kaynaklardan anahtarları artırma

Dizinizi bir kaynakta bulunan bir değerden başlatmak isterseniz, yedek anahtar dönüşümünüzü hemen izleyerek türetilmiş bir sütun dönüştürmesi kullanabilir ve iki değeri birlikte ekleyebilirsiniz:

![SK en fazla](media/data-flow/sk006.png "yedek anahtar dönüşümü en fazla Ekle")

Anahtar değerini önceki üst sınır ile temel almak için kullanabileceğiniz iki teknik vardır:

### <a name="database-sources"></a>Veritabanı kaynakları

Kaynak dönüşümünü kullanarak kaynağınızdan MAX () ' i seçmek için "sorgu" seçeneğini kullanın:

![Vekil anahtar sorgusu](media/data-flow/sk002.png "vekil anahtar dönüştürme sorgusu")

### <a name="file-sources"></a>Dosya kaynakları

Önceki en büyük değer bir dosya içinde ise, kaynak dönüşümünüzü bir toplam dönüşümle birlikte kullanabilir ve en büyük () ifade işlevini kullanarak önceki en büyük değeri alabilirsiniz:

![Vekil anahtar dosya](media/data-flow/sk008.png "yedek anahtar dosyası")

Her iki durumda da, gelen yeni verilerinizi önceki en büyük değeri içeren kaynakla birlikte katılmanız gerekir:

![Vekil anahtar birleşimi](media/data-flow/sk004.png "yedek anahtar birleşimi")

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekler, [JOIN](data-flow-join.md) ve [türetilmiş sütun](data-flow-derived-column.md) dönüşümlerini kullanır.
