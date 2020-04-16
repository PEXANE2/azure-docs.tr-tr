---
title: Veri akışını eşlemede dönüşümü sıralama
description: Azure Veri Fabrikası Haritalama Veri Sıralama Dönüşümü
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 04/14/2020
ms.openlocfilehash: 381c6573dff1b3f1638af9090a535d9a1e59b2b5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81413164"
---
# <a name="sort-transformation-in-mapping-data-flow"></a>Veri akışını eşlemede dönüşümü sıralama

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Sıralama dönüştürme, geçerli veri akışında gelen satırları sıralamanızı sağlar. Tek tek sütunları seçebilir ve bunları artan veya azalan sırada sıralayabilirsiniz.

> [!NOTE]
> Eşleme veri akışları, verileri birden çok düğüm ve bölüme dağıtan kıvılcım kümelerinde yürütülür. Sonraki bir dönüşümde verilerinizi yeniden bölmeyi seçerseniz, verilerin değiştirilmesi nedeniyle sıralamanızı kaybedebilirsiniz.

## <a name="configuration"></a>Yapılandırma

![Ayarları sıralama](media/data-flow/sort.png "Sırala")

**Büyük/küçük harf duyarsız:** Dize veya metin alanlarını sıralarken büyük/küçük/küçük harf göz ardı etmek isteyip istemediğiniz

**Yalnızca Bölümler İçinde Sırala:** Veri akışları kıvılcım üzerinde çalıştırılırken, her veri akışı bölümlere ayrılır. Bu ayar, tüm veri akışını sıralamak yerine verileri yalnızca gelen bölümlerin içinde sıralar. 

**Sıralama koşulları:** Sıralamanın hangi sırayla gerçekleşeceğini seçin. Sipariş sıralama önceliğini belirler. Veri akışının başında veya sonunda nulls görünüp görünmeyeceğini seçin.

### <a name="computed-columns"></a>Hesaplanmış sütunlar

Sıralamayı uygulamadan önce bir sütun değerini değiştirmek veya ayıklamak için sütunun üzerine gidin ve "hesaplanmış sütun"u seçin. Bu, sütun değeri kullanmak yerine sıralama işlemi için bir ifade oluşturmak için ifade oluşturucuyu açar.

## <a name="data-flow-script"></a>Veri akışı betiği

### <a name="syntax"></a>Sözdizimi

```
<incomingStream>
    sort(
        desc(<sortColumn1>, { true | false }),
        asc(<sortColumn2>, { true | false }),
        ...
    ) ~> <sortTransformationName<>
```

### <a name="example"></a>Örnek

![Ayarları sıralama](media/data-flow/sort.png "Sırala")

Yukarıdaki sıralama yapılandırması için veri akışı komut dosyası aşağıdaki kod snippet bulunmaktadır.

```
BasketballStats sort(desc(PTS, true),
    asc(Age, true)) ~> Sort1
```

## <a name="next-steps"></a>Sonraki adımlar

Sıralamadan [sonra, Toplu Dönüştürme'yi](data-flow-aggregate.md) kullanmak isteyebilirsiniz
