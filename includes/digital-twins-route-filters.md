---
author: baanders
description: Azure Digital TWINS yol filtresi seçenekleri için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 12/04/2020
ms.author: baanders
ms.openlocfilehash: e06e660a43aaa0ff5eb79bc00bd8a5d2c61c6580
ms.sourcegitcommit: 8dd8d2caeb38236f79fe5bfc6909cb1a8b609f4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2021
ms.locfileid: "98045328"
---
| Filtre adı | Açıklama | Filtre metni şeması | Desteklenen değerler | 
| --- | --- | --- | --- |
| Doğru/yanlış | Filtre olmadan bir yol oluşturulmasına veya hiçbir olay gönderilmeden bir yolu devre dışı bırakmaya izin verir | `<true/false>` | `true` = Route filtre olmadan etkinleştirildi <br> `false` = yol devre dışı |
| Tür | Dijital ikizi örneğiniz aracılığıyla akan [olay türü](../articles/digital-twins/concepts-route-events.md#types-of-event-messages) | `type = '<eventType>'` | Olası olay türü değerleri şunlardır: <br>`Microsoft.DigitalTwins.Twin.Create` <br> `Microsoft.DigitalTwins.Twin.Delete` <br> `Microsoft.DigitalTwins.Twin.Update`<br>`Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br> `Microsoft.DigitalTwins.Relationship.Delete` <br> `microsoft.iot.telemetry`  |
| Kaynak | Azure dijital TWINS örneğinin adı | `source = '<hostname>'`| Olası ana bilgisayar adı değerleri şunlardır: <br> **Bildirimler için**: `<yourDigitalTwinInstance>.api.<yourRegion>.digitaltwins.azure.net` <br> **Telemetri için**: `<yourDigitalTwinInstance>.api.<yourRegion>.digitaltwins.azure.net/<twinId>`|
| Konu | Yukarıdaki olay kaynağı bağlamındaki olay açıklaması | `subject = '<subject>'` | Olası konu değerleri şunlardır: <br>**Bildirimler için**: konu `<twinid>` <br> ya da birden çok parça veya kimlik tarafından benzersiz şekilde tanımlanan konular için URI biçimi:<br>`<twinid>/relationships/<relationshipid>`<br> **Telemetri için**: konu bileşen yoludur (telemetri bir ikizi bileşeninden yayıldıysanız), gibi `comp1.comp2` . Telemetri bir bileşenden yayılmadığından, konu alanı boş olur. |
| Veri şeması | DTDL model KIMLIĞI | `dataschema = '<model-dtmi-ID>'` | **Telemetri için**: veri şeması, ikizi veya Telemetriyi gösteren BILEŞENIN model kimliğidir. Örneğin, `dtmi:example:com:floor4;2` <br>**Bildirimler için (oluşturma/silme)**: veri şemasına, konumundaki bildirim gövdesinden erişilebilir `$body.$metadata.$model` . <br>**Bildirimler (güncelleştirme) için**: şu adreste bulunan bildirim gövdesinden veri şemasına erişilebilir: `$body.modelId`|
| İçerik türü | Veri değerinin içerik türü | `datacontenttype = '<contentType>'` | İçerik türü `application/json` |
| Spec sürümü | Kullandığınız olay şemasının sürümü | `specversion = '<version>'` | Sürüm olmalıdır `1.0` . Bu, CloudEvents şema sürüm 1,0 ' i gösterir |
| Bildirim gövdesi | Bildirim alanındaki herhangi bir özelliğe başvur `data` | `$body.<property>` | Bildirim örnekleri için bkz. [*nasıl yapılır: olay verilerini anlama*](../articles/digital-twins/how-to-interpret-event-data.md) . Alanındaki herhangi bir özelliğe, `data` kullanılarak başvurulabilir `$body`

Aşağıdakine benzer bir isteğe birden çok filtre ekleyebileceğinizi unutmayın: 

:::code language="json" source="~/digital-twins-docs-samples/api-requests/filter-multiple.json":::

Aşağıdaki veri türleri, yukarıdaki verilere başvurular tarafından döndürülen değerler olarak desteklenir:

| Veri türü | Örnek |
|-|-|-|
|**Dize**| `STARTS_WITH($body.$metadata.$model, 'dtmi:example:com:floor')` <br> `CONTAINS(subject, '<twinID>')`|
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