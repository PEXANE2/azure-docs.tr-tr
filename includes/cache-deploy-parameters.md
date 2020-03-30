---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 04/02/2019
ms.author: wesmc
ms.openlocfilehash: 498a7ee28b9404d0733e4615f4df635a8c904b51
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188180"
---
### <a name="cacheskuname"></a>cacheSKUName

Redis için yeni Azure Önbelleğinin fiyatlandırma katmanı.

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

Şablon, bu parametre (Temel, Standart veya Premium) için izin verilen değerleri tanımlar ve değer belirtilmemişse varsayılan bir değer (Temel) atar. Basic, 53 GB'a kadar birden fazla boyuta sahip tek bir düğüm sağlar. Standart, 53 GB'a kadar birden fazla boyut ve %99,9 SLA ile iki düğümlü Birincil/Çoğaltma sağlar.

### <a name="cacheskufamily"></a>cacheSKUFamily

Sku için aile.

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

### <a name="cacheskucapacity"></a>önbellekSKUCapacity

Redis örneği için yeni Azure Önbelleği boyutu.

Temel ve Standart aileler için:

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

İzin verilen değerler 0'dan 6'ya kadar değil, 1'den 5'e kadar çalıştırılan lar dışında Premium değer önbellek kapasitesi aynı şekilde tanımlanır.

Şablon, bu parametre için izin verilen tamsayı değerlerini tanımlar (Temel ve Standart aileler için 0'dan 6'ya, Premium ailesi için 1'den 5'e kadar). Değer belirtilmemişse, şablon Temel ve Standart için 0 varsayılan değer, Premium için 1 atar.

Değerler aşağıdaki önbellek boyutlarına karşılık gelir:

| Değer | Temel ve Standart<br>önbellek boyutu | Premium<br>önbellek boyutu |
| :---: | :------------------------------: | :-------------------: |
| 0     | 250 MB (varsayılan)                 | yok                   |
| 1     | 1 GB                             | 6 GB (varsayılan)        |
| 2     | 2,5 GB                           | 13 GB                 |
| 3     | 6 GB                             | 26 GB                 |
| 4     | 13 GB                            | 53 GB                 |
| 5     | 26 GB                            | 120 GB                |
| 6     | 53 GB                            | yok                   |
