---
title: V3 API sorgu dizesi parametreleri
ms.topic: include
ms.date: 06/30/2020
ms.openlocfilehash: 47727f6df772669fa323a10cd099764a6d35cb6a
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85610803"
---
V3 API sorgu dizesi parametreleri şunları içerir:

|Sorgu parametresi|LUSıS portalı adı|Tür|Sürüm|Varsayılan|Amaç|
|--|--|--|--|--|--|
|`log`|Günlükleri Kaydet|boole|V2 & V3|yanlış|Sorguyu günlük dosyasında sakla. Varsayılan değer false 'dur.|
|`query`|-|string|Yalnızca v3|Varsayılan değer yok-GET isteğinde gerekli|**V2 'de**, tahmin edilecek olan söylenişi `q` parametresi. <br><br>**V3 'de**, işlev `query` parametreye geçirilir.|
|`show-all-intents`|Tüm amaçlar için puanları dahil et|boole|Yalnızca v3|yanlış|**Tahmin. amaçlar** nesnesindeki karşılık gelen puanı içeren tüm hedefleri döndürün. Amaçlar bir üst nesnede nesneler olarak döndürülür `intents` . Bu, bir dizide amacı bulmaya gerek kalmadan programlı erişime izin verir: `prediction.intents.give` . V2 'de, bunlar bir dizide döndürülür. |
|`verbose`|Daha fazla varlık ayrıntısı ekleyin|boole|V2 & V3|yanlış|**V2 sürümünde**, true olarak ayarlandığında, tüm tahmin edilen amaçlar döndürülür. Tahmin edilen tüm amaçlar için ihtiyaç duyuyorsanız, v3 param ' ı kullanın `show-all-intents` .<br><br>**V3 'de**, bu parametre yalnızca varlık tahmini varlık meta veri ayrıntılarını sağlar.  |
|`timezoneOffset`|-|string|V2|-|DatetimeV2 varlıklara uygulanan saat dilimi.|
|`datetimeReference`|-|string|Yüklemesinde|-|DatetimeV2 varlıklara uygulanan [saat dilimi](../luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) . `timezoneOffset`V2 'den değiştirilir.|