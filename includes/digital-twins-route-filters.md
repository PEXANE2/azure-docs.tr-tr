---
author: baanders
description: Azure Digital TWINS yol filtresi seçenekleri için dosya ekleme
ms.service: digital-twins
ms.topic: include
ms.date: 8/3/2020
ms.author: baanders
ms.openlocfilehash: c5b78d80b1aa958af50b81dc152b4a746ce85f70
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87902223"
---
| Filtre adı | Açıklama | Filtre metni şeması | Desteklenen değerler | 
| --- | --- | --- | --- |
| Doğru/yanlış | Filtre olmadan bir yol oluşturulmasına veya hiçbir olay gönderilmeden bir yolu devre dışı bırakmaya izin verir | `<true/false>` | `true`= Route filtre olmadan etkinleştirildi <br> `false`= yol devre dışı |
| Tür | Dijital ikizi örneğiniz aracılığıyla akan [olay türü](../articles/digital-twins/concepts-route-events.md#types-of-event-messages) | `type = '<eventType>'` | Olası olay türü değerleri şunlardır: <br>`Microsoft.DigitalTwins.Twin.Create` <br> `Microsoft.DigitalTwins.Twin.Delete` <br> `Microsoft.DigitalTwins.Twin.Update`<br>`Microsoft.DigitalTwins.Relationship.Create`<br>`Microsoft.DigitalTwins.Relationship.Update`<br> `Microsoft.DigitalTwins.Relationship.Delete` <br> `microsoft.iot.telemetry`  |
| Kaynak | Azure dijital TWINS örneğinin adı | `source = '<hostname>'`| Olası ana bilgisayar adı değerleri şunlardır: <br> **Bildirimler için**:`<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net` <br> **Telemetri için**:`<yourDigitalTwinInstance>.<yourRegion>.azuredigitaltwins.net/digitaltwins/<twinId>`|
| Konu | Yukarıdaki olay kaynağı bağlamındaki olay açıklaması | `subject = '<subject>'` | Olası konu değerleri şunlardır: <br>**Bildirimler için**: konu`<twinid>` <br> ya da birden çok parça veya kimlik tarafından benzersiz şekilde tanımlanan konular için URI biçimi:<br>`<twinid>/relationships/<relationshipid>`<br> **Telemetri için**: konu bileşen yoludur (telemetri bir ikizi bileşeninden yayıldıysanız), gibi `comp1.comp2` . Telemetri bir bileşenden yayılmadığından, konu alanı boş olur. |
| Veri şeması | DTDL model KIMLIĞI | `dataschema = '<model-dtmi-ID>'` | **Telemetri için**: veri şeması, ikizi veya Telemetriyi gösteren BILEŞENIN model kimliğidir. Örneğin, `dtmi:example:com:floor4;2` <br>**Bildirimler için**: veri şeması desteklenmez|
| İçerik türü | Veri değerinin içerik türü | `datacontenttype = '<contentType>'` | İçerik türü`application/json` |
| Spec sürümü | Kullandığınız olay şemasının sürümü | `specversion = '<version>'` | Sürüm olmalıdır `1.0` . Bu, CloudEvents şema sürüm 1,0 ' i gösterir |

Filtreleri aşağıdaki işlemleri kullanarak birleştirmek de mümkündür:

| Filtre adı | Filtre metni şeması | Örnek | 
| --- | --- | --- |
| VE/VEYA | `<filter1> AND <filter2>` | `type != 'microsoft.iot.telemetry' AND datacontenttype = 'application/json'` |
| VE/VEYA | `<filter1> OR <filter2>` | `type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json'` |
| İç içe işlemler | `(<Comparison1>) AND (<Comparison2>)` | `(type != 'microsoft.iot.telemetry' OR datacontenttype = 'application/json') OR (specversion != '1.0')` |

> [!NOTE]
> Önizleme sırasında yalnızca dize eşitlik desteklenir (=,! =).

Bir filtreyi uyguladığınızda veya güncelleştirdiğinizde, değişikliğin veri ardışık düzeninde yansıtılması birkaç dakika sürebilir.
