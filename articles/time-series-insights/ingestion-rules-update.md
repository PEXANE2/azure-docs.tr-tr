---
title: Azure Time Series Insights Gen2 ' deki giriş ve düzleştirme kurallarında yaklaşan değişiklikler | Microsoft Docs
description: Giriş kuralı değişiklikleri
ms.service: time-series-insights
services: time-series-insights
author: lyrana
ms.author: lyhughes
manager: dpalled
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/12/2020
ms.custom: lyhughes
ms.openlocfilehash: cdf90614e1f766fc37e04a1416081bd35e19b74e
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88168210"
---
# <a name="upcoming-changes-to-the-json-flattening-and-escaping-rules-for-new-environments"></a>Yeni ortamlar için JSON düzleştirme ve kaçış kurallarında yakında yapılan değişiklikler

**Bu değişiklikler yalnızca *Yeni oluşturulan* Azure Time Series Insights Gen2 ortamlarına uygulanır. Bu değişiklikler Gen1 ortamları için geçerlidir.**

Azure Time Series Insights Gen2 ortamınız, belirli bir adlandırma kuralları kümesini izleyerek depolama sütunlarınızı dinamik olarak oluşturur. Bir olay tamamlandığında, JSON yüküne ve özellik adlarına bir dizi kural uygulanır. JSON verilerinin düzleştirildiğinde ve depolanabileceği değişiklikler 2020 Temmuz 'da yeni Azure Time Series Insights Gen2 ortamları için geçerli olacaktır. Bu değişiklik, aşağıdaki durumlarda sizi etkiler:

* JSON yükünüzü iç içe nesneler içeriyorsa
* JSON yükünüzü diziler içeriyorsa
* JSON özelliği adında aşağıdaki dört özel karakterden birini kullanıyorsanız: [\. '
* Bir veya daha fazla TS KIMLIĞINIZ özelliği iç içe yerleştirilmiş bir nesne içindeyse.

Yeni bir ortam oluşturursanız ve yukarıdaki durumlardan biri veya daha fazlası olay yükünüzü geçerliyse, verilerinizi düzleştirilmiş ve farklı şekilde depoladığınız şekilde görürsünüz. Değişikliklerin özeti aşağıda verilmiştir:

| Geçerli kural | Yeni Kural | Örnek JSON | Önceki sütun adı | Yeni sütun adı
|---|---| ---| ---|  ---|
| İç içe geçmiş JSON, ayırıcı olarak alt çizgi kullanılarak düzleştirilir |İç içe geçmiş JSON, ayırıcı olarak bir nokta kullanılarak düzleştirilir  | ``{"series" : { "value" : 19.338 }}`` | `series_value_double` |`series.value_double` |
| Özel karakterler atlanmaz | Özel karakterleri içeren JSON özelliği adları. [\ ve ', [' ve '] kullanılarak atlanmalıdır. [' Ve '] içinde, tek tırnak ve ters eğik çizgiler için ek kaçış var. Tek bir teklif olarak yazılacak \' ve bir ters eğik çizgi şu şekilde yazılacak\\\  | ```"Foo's Law Value": "17.139999389648"``` | `Foo's Law Value_double` | `['Foo\'s Law Value']_double` |
| Temel elemanlar dizileri bir dize olarak depolanır | Temel türlerin dizileri dinamik bir tür olarak depolanır  | `"values": [154, 149, 147]` | `values_string`  | `values_dynamic` |
Nesne dizileri her zaman düzleştirilir, birden çok olay üretir | Bir dizideki nesneler TS KIMLIĞI veya zaman damgası özellikleri (lar) yoksa, nesne dizisi dinamik bir tür olarak tamamen saklanır | `"values": [{"foo" : 140}, {"bar" : 149}]` | `values_foo_long | values_bar_long` | `values_dynamic` |

## <a name="recommended-changes-for-new-environments"></a>Yeni ortamlar için önerilen değişiklikler

### <a name="if-your-ts-id-andor-timestamp-property-is-nested-within-an-object"></a>TS KIMLIĞINIZ ve/veya timestamp özelliği bir nesne içinde iç içe ise

* Yeni dağıtımların yeni alım kurallarıyla eşleşmesi gerekecektir. Örneğin, TS KIMLIĞINIZ ise, `telemetry_tagId` ortam kimliği olarak yapılandırmak üzere Azure Resource Manager (ARM) şablonları veya otomatik dağıtım betikleri güncelleştirmeniz gerekir `telemetry.tagId` . Bu değişiklik, iç içe geçmiş JSON 'daki olay kaynak zaman damgaları için de gereklidir.

### <a name="if-your-payload-contains-nested-json-or-special-characters-and-you-automate-authoring-time-series-model-variable-expressions"></a>Yükleriniz iç içe geçmiş JSON veya özel karakterler içeriyorsa ve yazma [zaman serisi model](.\time-series-insights-update-tsm.md) değişkeni ifadelerini otomatikleştirin

* Yeni giriş kurallarını eşleştirmek için [Typesbatchput](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriestypes/executebatch#typesbatchput) öğesini yürüten istemci kodunuzu güncelleştirin. Örneğin, önceki bir [zaman serisi ifadesinin](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax) `"value": {"tsx": "$event.series_value.Double"}` aşağıdaki seçeneklerden birine güncelleştirilmeleri gerekir:
  * `"value": {"tsx": "$event.series.value.Double"}`
  * `"value": {"tsx": "$event['series']['value'].Double"}`

## <a name="next-steps"></a>Sonraki adımlar

* [Gen2 depolama ve giriş Azure Time Series Insights](./time-series-insights-update-storage-ingress.md)okuyun.

* [Zaman serisi sorgu API 'lerini](./concepts-query-overview.md)kullanarak verilerinizi sorgulama hakkında daha fazla bilgi edinin.

* [Yeni zaman serisi ifade sözdizimi](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax)hakkında daha fazla bilgi edinin.
