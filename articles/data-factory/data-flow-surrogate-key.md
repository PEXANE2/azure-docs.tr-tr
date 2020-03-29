---
title: Haritalama veri akışı Surrogate Anahtar Dönüşümü
description: Sıralı anahtar değerleri oluşturmak için Azure Veri Fabrikası'nın haritalama veri akışı Surrogate Anahtar Dönüşümü nasıl kullanılır?
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 02/12/2019
ms.openlocfilehash: bab48aa9079c1b8020bb828a6bb91bd244a78cf1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74930212"
---
# <a name="mapping-data-flow-surrogate-key-transformation"></a>Haritalama veri akışı Surrogate Anahtar Dönüşümü



Veri akışı satır kümenize iş dışı rasgele bir anahtar değeri eklemek için Surrogate Anahtar Dönüşüm'üni kullanın. Bu, boyut tablolarınızdaki her üyenin Kimball DW metodolojisinin bir parçası olan benzersiz bir anahtara sahip olması gereken bir yıldız şema analitik veri modelinde boyut tabloları tasarlarken kullanışlıdır.

![Vekil Anahtar Dönüşümü](media/data-flow/surrogate.png "Vekil Anahtar Dönüşümü")

"Anahtar Sütun" yeni vekil anahtar sütununuza vereceğiniz addır.

"Başlangıç Değeri" artımlı değerin başlangıç noktasıdır.

## <a name="increment-keys-from-existing-sources"></a>Varolan kaynaklardan artan anahtarlar

Dizinizi Kaynak'ta bulunan bir değerden başlatmak istiyorsanız, Vekil Anahtar dönüşümünüzü hemen izleyen türetilmiş sütun dönüşümunu kullanabilir ve iki değeri bir araya ekleyebilirsiniz:

![SK Max eklemek](media/data-flow/sk006.png "Surrogate Anahtar Dönüşümü Max ekle")

Anahtar değerini önceki max ile tohumlamak için kullanabileceğiniz iki teknik vardır:

### <a name="database-sources"></a>Veritabanı kaynakları

Kaynak dönüşümünü kullanarak kaynağınızdan MAX() seçeneğini seçmek için "Sorgula" seçeneğini kullanın:

![Vekil Anahtar Sorgusu](media/data-flow/sk002.png "Vekil Anahtar Dönüşüm Sorgusu")

### <a name="file-sources"></a>Dosya kaynakları

Önceki maksimum değeriniz bir dosyadaysa, Kaynak dönüşümünüzü Bir Toplu dönüşümle birlikte kullanabilir ve önceki maksimum değeri elde etmek için MAX() ifade işlevini kullanabilirsiniz:

![Vekil Anahtar Dosyası](media/data-flow/sk008.png "Vekil Anahtar Dosyası")

Her iki durumda da, gelen yeni verilerinizi önceki maksimum değeri içeren kaynağınızla birlikte birleştirmeniz gerekir:

![Vekil Anahtar Birleştirme](media/data-flow/sk004.png "Vekil Anahtar Birleştirme")

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekler, [Birleştirme](data-flow-join.md) ve [Türemiş Sütun](data-flow-derived-column.md) dönüşümlerini kullanır.
