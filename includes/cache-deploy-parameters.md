---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 04/02/2019
ms.author: wesmc
ms.openlocfilehash: 498a7ee28b9404d0733e4615f4df635a8c904b51
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67188180"
---
### <a name="cacheskuname"></a>cacheSKUName

Redin için yeni Azure önbelleğinin fiyatlandırma katmanı.

```json
    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard",
        "Premium"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Cache for Redis."
      }
    },
```

Şablon, bu parametre için izin verilen değerleri tanımlar (temel, standart veya Premium) ve hiçbir değer belirtilmemişse varsayılan bir değer (temel) atar. Temel, 53 GB 'a kadar kullanılabilir birden çok boyutu olan tek bir düğüm sağlar. Standart, 53 GB ve% 99,9 SLA kadar kullanılabilir birden çok boyut ile iki düğümlü birincil/çoğaltma sağlar.

### <a name="cacheskufamily"></a>cacheSKUFamily

SKU için aile.

```json
    "cacheSKUFamily": {
      "type": "string",
      "allowedValue/s": [
        "C",
        "P"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },
```

### <a name="cacheskucapacity"></a>cacheSKUCapacity

Redsıs örneği için yeni Azure önbelleğinin boyutu.

Temel ve standart aileler için:

```json
    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Cache for Redis instance. "
      }
    }
```

Premium değer önbelleği kapasitesi, izin verilen değerler 0 ile 6 yerine 1 ile 5 arasında çalışacak şekilde tanımlanır.

Şablon, bu parametre için izin verilen tamsayı değerlerini tanımlar (temel ve standart aileler için 0 ila 6); Premium Aile için 1 ila 5). Hiçbir değer belirtilmemişse, şablon temel ve standart için varsayılan değeri 0, Premium için 1 olarak atar.

Değerler aşağıdaki önbellek boyutlarına karşılık gelir:

| Değer | Temel ve standart<br>önbellek boyutu | Premium<br>önbellek boyutu |
| :---: | :------------------------------: | :-------------------: |
| 0     | 250 MB (varsayılan)                 | yok                   |
| 1     | 1 GB                             | 6 GB (varsayılan)        |
| 2     | 2,5 GB                           | 13 GB                 |
| 3     | 6 GB                             | 26 GB                 |
| 4     | 13 GB                            | 53 GB                 |
| 5     | 26 GB                            | 120 GB                |
| 6     | 53 GB                            | yok                   |
