---
author: baanders
description: Azure Digital TWINS yol filtresi seçenekleri için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 8/3/2020
ms.author: baanders
ms.openlocfilehash: a1098088a38b23ec1074434e5424e261e60bcd55
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "91779399"
---
| Filtre adı | Açıklama | Filtre metni şeması | Desteklenen değerler | 
| --- | --- | --- | --- |
| Doğru/yanlış | Filtre olmadan bir yol oluşturulmasına veya hiçbir olay gönderilmeden bir yolu devre dışı bırakmaya izin verir | `<true/false>` | `true` = Route filtre olmadan etkinleştirildi <br> `false` = yol devre dışı |
| Tür | Dijital ikizi örneğiniz aracılığıyla akan [olay türü](../articles/digital-twins/concepts-route-events.md#types-of-event-messages) | `type = '<eventType>'` | Olası olay türü değerleri şunlardır: <br>`Microsoft.DigitalTwins.Twin.Create` <br> `Microsoft.DigitalTwins.Twin.Delete` <br> `Microsoft.DigitalTwins.Twin.Update`<br>`Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br> `Microsoft.DigitalTwins.Relationship.Delete` <br> `microsoft.iot.telemetry`  |
| Kaynak | Azure dijital TWINS örneğinin adı | `source = '<hostname>'`| Olası ana bilgisayar adı değerleri şunlardır: <br> **Bildirimler için**: `<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net` <br> **Telemetri için**: `<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net/digitaltwins/<twinId>`|
| Konu | Yukarıdaki olay kaynağı bağlamındaki olay açıklaması | `subject = '<subject>'` | Olası konu değerleri şunlardır: <br>**Bildirimler için**: konu `<twinid>` <br> ya da birden çok parça veya kimlik tarafından benzersiz şekilde tanımlanan konular için URI biçimi:<br>`<twinid>/relationships/<relationshipid>`<br> **Telemetri için**: konu bileşen yoludur (telemetri bir ikizi bileşeninden yayıldıysanız), gibi `comp1.comp2` . Telemetri bir bileşenden yayılmadığından, konu alanı boş olur. |
| Veri şeması | DTDL model KIMLIĞI | `dataschema = '<model-dtmi-ID>'` | **Telemetri için**: veri şeması, ikizi veya Telemetriyi gösteren BILEŞENIN model kimliğidir. Örneğin, `dtmi:example:com:floor4;2` <br>**Bildirimler için**: şu adreste bulunan bildirim gövdesinden veri şemasına erişilebilir `$body.$metadata.$model`|
| İçerik türü | Veri değerinin içerik türü | `datacontenttype = '<contentType>'` | İçerik türü `application/json` |
| Spec sürümü | Kullandığınız olay şemasının sürümü | `specversion = '<version>'` | Sürüm olmalıdır `1.0` . Bu, CloudEvents şema sürüm 1,0 ' i gösterir |
| Bildirim gövdesi | Bildirim alanındaki herhangi bir özelliğe başvur `data` | `$body.<property>` | Bildirim örnekleri için bkz. [*nasıl yapılır: olay verilerini anlama*](https://docs.microsoft.com/azure/digital-twins/how-to-interpret-event-data) . Alanındaki herhangi bir özelliğe, `data` kullanılarak başvurulabilir `$body`

Aşağıdaki veri türleri, yukarıdaki verilere başvurular tarafından döndürülen değerler olarak desteklenir:

| Veri türü | Örnek |
|-|-|-|
|**Dize**| `STARTS_WITH($body.$metadate.$model, 'dtmi:example:com:floor')` <br> `CONTAINS(subject, '<twinID>')`|
|**Tamsayı**|`$body.errorCode > 200`|
|**Çift**|`$body.temperature <= 5.5`|
|**Bool**|`$body.poweredOn = true`|
|**Null**|`$body.prop != null`|

Yol filtreleri tanımlanırken aşağıdaki işleçler desteklenir:

|Family (Aile)|İşleçler|Örnek|
|-|-|-|
|Mantıksal|VE, VEYA, ()|`(type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json') OR (specversion != '1.0')`|
|Karşılaştırma|<, <=, >, >=, =,! =|`$body.temperature <= 5.5`

Yol filtreleri tanımlanırken aşağıdaki işlevler desteklenir:

|İşlev|Açıklama|Örnek|
|--|--|--|
|STARTS_WITH (x, y)|Değer dizeyle başlıyorsa true döndürür `x` `y` .|`STARTS_WITH($body.$metadata.$model, 'dtmi:example:com:floor')`|
|ENDS_WITH (x, y) | Değer `x` dize ile bitiyorsa true değerini döndürür `y` .|`ENDS_WITH($body.$metadata.$model, 'floor;1')`|
|IÇERIR (x, y)| Değer dizeyi içeriyorsa, true döndürür `x` `y` .|`CONTAINS(subject, '<twinID>')`|

Bir filtreyi uyguladığınızda veya güncelleştirdiğinizde, değişikliğin veri ardışık düzeninde yansıtılması birkaç dakika sürebilir.
