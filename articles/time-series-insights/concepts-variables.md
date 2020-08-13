---
title: Model değişkenleri-Azure Time Series Insights Gen2 | Microsoft Docs
description: Model değişkenleri
author: shreyasharmamsft
ms.author: shresha
ms.service: time-series-insights
ms.topic: conceptual
ms.date: 08/12/2020
ms.openlocfilehash: d0a5b48e93e839b0a0adaf185700d7f60fec7948
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88140692"
---
# <a name="time-series-model-variables"></a>Zaman serisi model değişkenleri

Bu makalede, olaylar üzerinde formül ve hesaplama kurallarını belirleyen zaman serisi model değişkenleri açıklanır.

Her değişken üç türden biri olabilir: *sayısal*, *kategorik*ve *Toplam*.

* **Sayısal** türleri sürekli sayısal değerlerle çalışır.
* **Kategorik** türleri tanımlanmış bir dizi farklı değer kümesiyle çalışır.
* **Toplama** türleri, tek bir türdeki birden çok değişkeni birleştirir (tüm sayısal veya tüm kategorik).

Aşağıdaki tabloda, her değişken türü için uygun olan özellikler görüntülenmektedir.

[![Zaman serisi modeli değişken tablosu](media/v2-update-tsm/time-series-model-variable-table.png)](media/v2-update-tsm/time-series-model-variable-table.png#lightbox)

## <a name="numeric-variables"></a>Sayısal değişkenler

| Variable Özelliği | Açıklama |
| --- | ---|
| Değişken filtresi | Filtreler, hesaplama için göz önünde bulundurulmakta olan satır sayısını kısıtlamak için isteğe bağlı Koşullu yan tümcelerdir. |
| Değişken değeri | Cihaz veya sensörlerden gelen ve zaman serisi Ifadeleri kullanılarak dönüştürülen hesaplama için kullanılan telemetri değerleri. Sayısal tür değişkenleri *Double*türünde olmalıdır.|
| Değişken ilişkilendirme | Enterpolasyon, var olan verileri kullanarak bir sinyalin nasıl yeniden oluşturulacağını belirtir. *Adım* ve *Doğrusal* enterpolasyon seçenekleri sayısal değişkenler için kullanılabilir. |
| Değişken toplama | [Sayısal değişken türleri için desteklenen toplama işlevleri](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax#numeric-variable-kind)aracılığıyla hesaplamalar gerçekleştirin. |

Değişkenler aşağıdaki JSON örneğine uyar:

```JSON
"Interpolated Speed": {
  "kind": "numeric",
  "value": {
    "tsx": "$event['Speed-Sensor'].Double"
  },
  "filter": null,
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span": "P1D"
    }
  },
  "aggregation": {
    "tsx": "right($value)"
  }
}
```

## <a name="categorical-variables"></a>Kategorik değişkenler

| Variable Özelliği | Açıklama |
| --- | ---|
| Değişken filtresi | Filtreler, hesaplama için göz önünde bulundurulmakta olan satır sayısını kısıtlamak için isteğe bağlı Koşullu yan tümcelerdir. |
| Değişken değeri | Cihazdan veya sensörlerden gelen hesaplama için kullanılan telemetri değerleri. Kategorik tür değişkenleri *Long* veya *String*olmalıdır. |
| Değişken ilişkilendirme | Enterpolasyon, var olan verileri kullanarak bir sinyalin nasıl yeniden oluşturulacağını belirtir. *Adım* ilişkilendirme seçeneği kategorik değişkenler için kullanılabilir. |
| Değişken kategorileri | Kategoriler cihazdan veya algılayıcılardan bir etikete gelen değerler arasında bir eşleme oluşturur. |
| Değişken varsayılan kategorisi | Varsayılan kategori, "Categories" özelliğinde eşlenmemiş tüm değerler içindir. |

Değişkenler aşağıdaki JSON örneğine uyar:

```JSON
"Status": {
  "kind": "categorical",
  "value": {
     "tsx": "$event.Status.Long"
},
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span" : "PT1M"
    }
  },
  "categories": [
    {
      "values": [0, 1, 2, 3],
      "label": "Good"
    },
    {
      "values": [4],
      "label": "Bad"
    }
  ],
  "defaultCategory": {
    "label": "Not Applicable"
  }
}
```

## <a name="aggregate-variables"></a>Toplam değişkenleri

| Variable Özelliği | Açıklama |
| --- | ---|
| Değişken filtresi | Filtreler, hesaplama için göz önünde bulundurulmakta olan satır sayısını kısıtlamak için isteğe bağlı Koşullu yan tümcelerdir. |
| Değişken toplama | [Toplu değişken türleri için desteklenen toplama işlevleri](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax#aggregate-variable-kind)aracılığıyla hesaplamalar gerçekleştirin. |

Değişkenler aşağıdaki JSON örneğine uyar:

```JSON
"Speed Range": {
  "kind": "aggregate",
  "filter": null,
  "aggregation": {
    "tsx": "max($event.Speed.Double) - min($event.Speed.Double)"
  }
}
```

Değişkenler, zaman serisi modelinin tür tanımında depolanır ve depolanan tanımı geçersiz kılmak ya da tamamlamak için API 'Ler aracılığıyla satır içi olarak sağlanabilecek.

## <a name="next-steps"></a>Sonraki adımlar

* [Zaman serisi modeli](./concepts-model-overview.md)hakkında daha fazla bilgi edinin.

* [Sorgu API 'leri](./concepts-query-overview.md)kullanarak değişkenlerin satır içi tanımlanması hakkında daha fazla bilgi edinin.
