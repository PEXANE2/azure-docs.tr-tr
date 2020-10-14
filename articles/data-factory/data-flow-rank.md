---
title: Eşleme veri akışında derecelendirme dönüşümü
description: Azure Data Factory eşleme veri akışı derecelendirme dönüşümü nasıl kullanılır bir derecelendirme sütunu oluştur
author: djpmsft
ms.author: daperlov
ms.reviewer: makromer
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 10/05/2020
ms.openlocfilehash: 8584d1b64191cc5258c6eeeef9ae4125bf1a2c65
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92044743"
---
# <a name="rank-transformation-in-mapping-data-flow"></a>Eşleme veri akışında derecelendirme dönüşümü 

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Kullanıcı tarafından belirtilen sıralama koşullarına bağlı olarak sıralı bir sıralama oluşturmak için sıralama dönüşümünü kullanın. 

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4GGJo]

## <a name="configuration"></a>Yapılandırma

![Sıralama ayarları](media/data-flow/rank-configuration.png "Sıralama ayarları")

**Büyük/küçük harf duyarsız:** Sıralama sütunu tür dizeyse, büyük/küçük harf derecelendirmelidir. 

**Yoğun:** Etkinleştirilirse, derece sütunu yoğun olarak derecelendirilir. Her bir sıra sayısı ardışık bir sayı olacak ve bir bağlama sonrasında sıralama değerleri atlanmaz.

**Sıralama sütunu:** Oluşturulan sıralama sütununun adı. Bu sütun Long türünde olacaktır.

**Sıralama koşulları:** Sıralama yaptığınız sütunları ve sıralamanın ne sırada olacağını seçin. Sıra sıralama önceliğini belirler.

Yukarıdaki yapılandırma gelen basketbol verilerini alır ve ' Pointsrank ' adlı bir derecelendirme sütunu oluşturur. En yüksek değer olan satır, 1 ' in *Pointsderecelendirme* değerine *sahip olacaktır.*

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Syntax

```
<incomingStream>
    rank(
        desc(<sortColumn1>),
        asc(<sortColumn2>),
        ...,
        caseInsensitive: { true | false }
        dense: { true | false }
        output(<rankColumn> as long)
    ) ~> <sortTransformationName<>
```

### <a name="example"></a>Örnek

![Sıralama ayarları](media/data-flow/rank-configuration.png "Sıralama ayarları")

Yukarıdaki sıra yapılandırması için veri akışı betiği aşağıdaki kod parçacığında bulunur.

```
PruneColumns
    rank(
        desc(PTS, true),
        caseInsensitive: false,
        output(pointsRanking as long),
        dense: false
    ) ~> RankByPoints
```

## <a name="next-steps"></a>Sonraki adımlar

[Filtre dönüşümünü](data-flow-filter.md)kullanarak sıralama değerlerine göre satırları filtreleyin.
