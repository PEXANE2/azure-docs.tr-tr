---
title: V3 API sorgu dizesi parametreleri
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: include
ms.date: 06/30/2020
ms.openlocfilehash: 14380dd781fc1eebfe7edb0a816ff8af0f2f17b4
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "91309446"
---
V3 API sorgu dizesi parametreleri şunları içerir:

|Sorgu parametresi|LUSıS portalı adı|Tür|Sürüm|Varsayılan|Amaç|
|--|--|--|--|--|--|
|`log`|Günlükleri Kaydet|boolean|V2 & V3|yanlış|Sorguyu günlük dosyasında sakla. Varsayılan değer false 'dur.|
|`query`|-|string|Yalnızca v3|Varsayılan değer yok-GET isteğinde gerekli|**V2 'de**, tahmin edilecek olan söylenişi `q` parametresi. <br><br>**V3 'de**, işlev `query` parametreye geçirilir.|
|`show-all-intents`|Tüm amaçlar için puanları dahil et|boolean|Yalnızca v3|yanlış|**Tahmin. amaçlar** nesnesindeki karşılık gelen puanı içeren tüm hedefleri döndürün. Amaçlar bir üst nesnede nesneler olarak döndürülür `intents` . Bu, bir dizide amacı bulmaya gerek kalmadan programlı erişime izin verir: `prediction.intents.give` . V2 'de, bunlar bir dizide döndürülür. |
|`verbose`|Daha fazla varlık ayrıntısı ekleyin|boolean|V2 & V3|yanlış|**V2 sürümünde**, true olarak ayarlandığında, tüm tahmin edilen amaçlar döndürülür. Tahmin edilen tüm amaçlar için ihtiyaç duyuyorsanız, v3 param ' ı kullanın `show-all-intents` .<br><br>**V3 'de**, bu parametre yalnızca varlık tahmini varlık meta veri ayrıntılarını sağlar.  |
|`timezoneOffset`|-|string|V2|-|DatetimeV2 varlıklara uygulanan saat dilimi.|
|`datetimeReference`|-|string|Yüklemesinde|-|DatetimeV2 varlıklara uygulanan [saat dilimi](../luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) . `timezoneOffset`V2 'den değiştirilir.|