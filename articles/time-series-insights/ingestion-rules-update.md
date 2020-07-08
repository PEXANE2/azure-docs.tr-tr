---
title: Azure Time Series Insights ' deki giriş ve düzleştirme kurallarında yaklaşan değişiklikler | Microsoft Docs
description: Giriş kuralı değişiklikleri
ms.service: time-series-insights
services: time-series-insights
author: lyrana
ms.author: lyhughes
manager: dpalled
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/16/2020
ms.custom: lyhughes
ms.openlocfilehash: 067244aa40256e3cc76239343790974bc3c06481
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85919027"
---
# <a name="upcoming-changes-to-the-json-flattening-and-escaping-rules-for-new-environments"></a>Yeni ortamlar için JSON düzleştirme ve kaçış kurallarında yakında yapılan değişiklikler

Bu değişiklikler yalnızca *yeni* Azure Time Series Insights Kullandıkça Öde (PAYG) ortamları için de geçerlidir. Bu değişiklikler standart (ler) SKU ortamları için geçerlidir.

Azure Time Series Insights ortamınız, belirli bir adlandırma kuralları kümesini izleyerek depolama sütunlarınızı dinamik olarak oluşturur. Bir olay tamamlandığında, JSON yüküne ve özellik adlarına bir dizi kural uygulanır. JSON verilerinin düzleştirildiğinde ve depolanmasındaki değişiklikler, 2020 Temmuz 'da yeni Azure Time Series Insights Kullandıkça Öde ortamları için geçerli olacaktır. Bu değişiklik, aşağıdaki durumlarda sizi etkiler:

* JSON yükünüzü iç içe nesneler içeriyorsa
*  JSON yükünüzü diziler içeriyorsa
*  JSON özelliği adında aşağıdaki dört özel karakterden birini kullanıyorsanız: [\. '
*  Bir veya daha fazla TS KIMLIĞINIZ özelliği iç içe yerleştirilmiş bir nesne içindeyse.

Yeni bir ortam oluşturursanız ve yukarıdaki durumlardan biri veya daha fazlası olay yükünüzü geçerliyse, verilerinizi düzleştirilmiş ve farklı şekilde depoladığınız şekilde görürsünüz. Değişikliklerin özeti aşağıda verilmiştir:

| Geçerli kural | Yeni Kural | Örnek JSON | Önceki sütun adı | Yeni sütun adı
|---|---| ---| ---|  ---|
| İç içe geçmiş JSON, ayırıcı olarak alt çizgi kullanılarak düzleştirilir |İç içe geçmiş JSON, ayırıcı olarak bir nokta kullanılarak düzleştirilir  | ``{"series" : { "value" : 19.338 }}`` | `series_value_double` |`series.value_double` |
| Özel karakterler atlanmaz | Özel karakterleri içeren JSON özelliği adları. [\ ve ', [' ve '] kullanılarak atlanmalıdır. [' Ve '] içinde, tek tırnak ve ters eğik çizgiler için ek kaçış var. Tek bir teklif olarak yazılacak \' ve bir ters eğik çizgi şu şekilde yazılacak\\\  | ```"Foo's Law Value": "17.139999389648"``` | `Foo's Law Value_double` | `['Foo\'s Law Value']_double` | 
| Temel elemanlar dizileri bir dize olarak depolanır | Temel türlerin dizileri dinamik bir tür olarak depolanır  | `"values": [154, 149, 147]` | `values_string`  | `values_dynamic` |
Nesne dizileri her zaman düzleştirilir, birden çok olay üretir | Bir dizideki nesneler TS KIMLIĞI veya zaman damgası özellikleri (lar) yoksa, nesne dizisi dinamik bir tür olarak tamamen saklanır | `"values": [{"foo" : 140}, {"bar" : 149}]` | `values_foo_long | values_bar_long` | `values_dynamic` |

## <a name="recommended-changes-for-new-environments"></a>Yeni ortamlar için önerilen değişiklikler

#### <a name="if-your-ts-id-andor-timestamp-property-is-nested-within-an-object"></a>TS KIMLIĞINIZ ve/veya timestamp özelliği bir nesne içinde iç içe ise:

*  Yeni dağıtımların yeni alım kurallarıyla eşleşmesi gerekecektir. Örneğin, TS KIMLIĞINIZ ise, `telemetry_tagId` ortam kimliği olarak yapılandırmak için tüm ARM şablonlarını veya Otomatik Dağıtım betiklerini güncelleştirmeniz gerekir `telemetry.tagId` . Bu değişiklik, iç içe geçmiş JSON 'daki olay kaynak zaman damgaları için de gereklidir.

 #### <a name="if-your-payload-contains-nested-json-or-special-characters-and-you-automate-authoring-time-series-model-variable-expressions"></a>Yükleriniz iç içe geçmiş JSON veya özel karakterler içeriyorsa ve yazma [zaman serisi model](.\time-series-insights-update-tsm.md) değişkeni ifadelerini otomatikleştirin

*  Yeni giriş kurallarını eşleştirmek için [Typesbatchput](https://docs.microsoft.com/rest/api/time-series-insights/dataaccess(preview)/timeseriestypes/executebatch#typesbatchput) öğesini yürüten istemci kodunuzu güncelleştirin. Örneğin, önceki bir [zaman serisi ifadesinin](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) `"value": {"tsx": "$event.series_value.Double"}` aşağıdaki seçeneklerden birine güncelleştirilmeleri gerekir:
    * `"value": {"tsx": "$event.series.value.Double"}`
    * `"value": {"tsx": "$event['series']['value'].Double"}`



## <a name="next-steps"></a>Sonraki adımlar

- [Uzun veri türü Için destek ekleme](./time-series-insights-long-data-type.md)makalesini okuyun.

- [Azure Time Series Insights önizleme depolama ve](./time-series-insights-update-storage-ingress.md)giriş makalesini okuyun.

