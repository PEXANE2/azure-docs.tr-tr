---
title: Uzun veri türü desteği ekleniyor | Microsoft Docs
description: Uzun veri türü desteği
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/16/2020
ms.custom: dpalled
ms.openlocfilehash: a0b53749a32e79d690cf4412fdac82b18dfe2f2e
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86183639"
---
# <a name="adding-support-for-long-data-type"></a>Uzun veri türü için destek ekleme

Bu değişiklikler yalnızca Önizleme (PAYG) ortamları için de geçerlidir. Standart (ler) SKU TSI ortamınız varsa, bu değişiklikleri yoksayabilirsiniz.

Sayısal verileri depolama ve dizin oluşturma konusunda sizi etkileyebilecek Azure Time Series Insights önizlemede değişiklik yaptık. Aşağıdaki durumlardan herhangi biri tarafından etkilenirse, gerekli değişiklikleri mümkün olan en kısa sürede yapın. Verileriniz, bölgenize bağlı olarak, 29 Haziran ve 30 Haziran 2020 arasında uzun ve çift olarak dizin oluşturmaya başlar. Bu değişiklik hakkında sorularınız veya endişeleriniz varsa Azure portal aracılığıyla bir destek bileti iletin ve bu iletişimin bahsetmesi gerekir.

Bu değişiklik, aşağıdaki durumlarda sizi etkiler:

1. Şu anda zaman serisi model değişkenleri kullanıyor ve telemetri verilerinizde yalnızca integral veri türleri gönderiyorsunuz.
1. Şu anda zaman serisi model değişkenleri kullanıyorsanız ve telemetri verilerinize hem integral hem de tam olmayan veri türleri gönderirseniz.
1. Tamsayı değerlerini kategorilere eşlemek için kategorik değişkenler kullanırsanız.
1. Özel bir ön uç uygulaması derlemek için JavaScript SDK 'sını kullanın.
1. Sıcak mağaza 'da (WS) 1.000-Özellik adı sınırına yaklaşdıysanız ve hem integral hem de tamsayı olmayan verileri gönderirseniz, özellik sayısı [Azure Portal](https://portal.azure.com/)bir ölçüm olarak görüntülenebilir.

Yukarıdaki durumlardan herhangi biri sizin için geçerlidir, bu değişikliğe uyum sağlamak için modelinizde değişiklikler yapmanız gerekir. Her iki TSI Explorer 'daki değişken tanımınızda ve önerilen değişikliklerle API 'lerimizi kullanarak herhangi bir özel istemcide zaman serisi Ifadesini güncelleştirin. Ayrıntılar için aşağıya bakın.

IoT çözümünüze ve kısıtlamalarına bağlı olarak, TSI PAYG ortamınıza gönderilen verilerin görünürlüğüne sahip olmayabilirsiniz. Verilerinizin yalnızca integral veya integral ve İntegral olmayan bir değer mi olduğunu bilmiyorsanız, birkaç seçeneğiniz vardır. Özelliğin serbest bırakılacağını bekleyebilir ve sonra iki ayrı sütunda hangi özelliklerin kaydedildiğini anlamak için Gezgin Kullanıcı arabirimindeki ham olaylarınızı keşfedebilirsiniz. Preemptively tüm sayısal etiketler için aşağıdaki değişiklikleri yapabilir veya şemayı daha iyi anlamak ve araştırmak için bir olay alt kümesini geçici olarak depolamaya yönlendirebilirsiniz. Olayları depolamak için, Event Hubs için [olay yakalamayı](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) açın veya IoT Hub Azure Blob depolama alanına [yönlendirin](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c#azure-storage) . Veriler ayrıca [Olay Hub 'ı Gezgini](https://marketplace.visualstudio.com/items?itemName=Summer.azure-event-hub-explorer)aracılığıyla veya [olay işleyicisi ana bilgisayarı](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send#receive-events)kullanılarak da gözlemlenebilir. IoT Hub kullanıyorsanız, yerleşik uç noktaya nasıl erişildiği [hakkındaki belgelere bakın](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin) .

Bu değişikliklerden etkileniyorsanız ve bunları yukarıdaki tarihlere göre yapamadıysanız, sorgu API 'Leri veya Time Series Insights Gezgini aracılığıyla erişilen etkilenen zaman serisi değişkenlerinin *null* döndürecek bir kesinti yaşayabilirsiniz (yani, Explorer 'da veri göstermez).

## <a name="recommended-changes"></a>Önerilen değişiklikler

Durum 1 & 2: **zaman serisi model değişkenlerini kullanma ve yalnızca integral veri türlerini gönderme ya da telemetri verilerinde hem integral hem de İntegral olmayan türler gönderme.**

Şu anda tamsayı telemetri verileri gönderiyorsanız, verileriniz iki sütuna bölünecektir: "propertyValue_double" ve "propertyValue_long".

Değişiklikler yürürlüğe geçtiğinizde ve daha önce alınan (ve gelecek) "propertyValue_double" içindeki sayısal veriler üzerine kopyalanmamışsa, tamsayı verileriniz "propertyValue_long" adresine yazılır.

"PropertyValue" özelliği için bu iki sütundaki verileri sorgulamak istiyorsanız, TSX 'inizdeki *birleşim ()* skaler işlevini kullanmanız gerekir. İşlev aynı veri türünün bağımsız değişkenlerini kabul eder ve bağımsız değişken listesindeki ilk null olmayan değeri döndürür ( [buradaki](https://docs.microsoft.com/rest/api/time-series-insights/preview#other-functions)kullanım hakkında daha fazla bilgi edinin).

### <a name="variable-definition-in-time-series-explorer---numeric"></a>Zaman serisi Gezgininde değişken tanımı-sayısal

*Önceki değişken tanımı:*

[![Önceki değişken tanımı](media/time-series-insights-long-data-type/var-def-previous.png)](media/time-series-insights-long-data-type/var-def-previous.png#lightbox)

*Yeni değişken tanımı:*

[![Yeni değişken tanımı](media/time-series-insights-long-data-type/var-def.png)](media/time-series-insights-long-data-type/var-def.png#lightbox)

Özel [zaman serisi ifadesi](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) olarak *"birleşim ($Event. PropertyValue. Double, todouble ($Event. PropertyValue. Long))"* öğesini de kullanabilirsiniz.

### <a name="inline-variable-definition-using-time-series-query-apis---numeric"></a>Zaman serisi sorgu API 'Leri kullanan satır içi değişken tanımı-sayısal

*Önceki değişken tanımı:*

```tsx
"PropertyValueVariable": {

    "kind": "numeric",

    "value": {

        "tsx": "$event.propertyValue.Double"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
```

*Yeni değişken tanımı:*

```tsx
"PropertyValueVariable ": {

    "kind": "numeric",

    "value": {

        "tsx": "coalesce($event.propertyValue.Long, toLong($event.propertyValue.Double))"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
```

Özel [zaman serisi ifadesi](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) olarak *"birleşim ($Event. PropertyValue. Double, todouble ($Event. PropertyValue. Long))"* öğesini de kullanabilirsiniz.

> [!NOTE]
> Bu değişkenleri, kullanılabilecek her yerde güncelleştirmenizi öneririz (zaman serisi modeli, Kaydedilen sorgular, Power BI bağlayıcı sorguları).

Durum 3: **tamsayı değerlerini kategorilere eşlemek için kategorik değişkenler kullanma**

Şu anda tamsayı değerlerini kategorilere eşleştiren kategorik değişkenler kullanıyorsanız, verileri çift türden uzun türe dönüştürmek için büyük olasılıkla toLong işlevini kullanıyorsunuz. Yukarıdaki durumlarda olduğu gibi, Double ve Long DataType sütunlarını da eklemeniz gerekecektir.

### <a name="variable-definition-in-time-series-explorer---categorical"></a>Zaman serisi Gezgininde değişken tanımı-kategorik

*Önceki değişken tanımı:*

[![Önceki değişken tanımı](media/time-series-insights-long-data-type/var-def-cat-previous.png)](media/time-series-insights-long-data-type/var-def-cat-previous.png#lightbox)

*Yeni değişken tanımı:*

[![Yeni değişken tanımı](media/time-series-insights-long-data-type/var-def-cat.png)](media/time-series-insights-long-data-type/var-def-cat.png#lightbox)

Özel [zaman serisi ifadesi](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) olarak *"birleşim ($Event. PropertyValue. Double, todouble ($Event. PropertyValue. Long))"* öğesini de kullanabilirsiniz.

Kategorik değişkenler yine de değerin bir tamsayı türünde olmasını gerektirir. Özel [zaman serisi ifadesinde](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) , birleşim () içindeki tüm bağımsız değişkenlerin veri türü Long türünde olmalıdır.

### <a name="inline-variable-definition-using-time-series-query-apis---categorical"></a>Zaman serisi sorgu API 'Leri kullanan satır içi değişken tanımı-kategorik

*Önceki değişken tanımı:*

```tsx
"PropertyValueVariable_Long": {

    "kind": "categorical",

    "value": {

        "tsx": "tolong($event.propertyValue.Double)"

    },

    "categories": [

    {
        "label": "Good",

        "values": [0, 1, 2 ]

    },

    {

        "label": "Bad",

        "values": [ 3, 4 ]

    } ],

    "defaultCategory": {

        "label": "Unknown"

    }

}
```

*Yeni değişken tanımı:*

```tsx
"PropertyValueVariable_Long": {

    "kind": "categorical",

    "value": {

        "tsx": "coalesce($event.propertyValue.Long, tolong($event.propertyValue.Double))"

    },

    "categories": [

    {
        "label": "Good",

        "values": [0, 1, 2 ]

    },

    {

        "label": "Bad",

        "values": [ 3, 4 ]

    } ],

    "defaultCategory": {

        "label": "Unknown"

    }

}
```

Kategorik değişkenler yine de değerin bir tamsayı türünde olmasını gerektirir. Özel [zaman serisi ifadesinde](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) , birleşim () içindeki tüm bağımsız değişkenlerin veri türü Long türünde olmalıdır.

> [!NOTE]
> Bu değişkenleri, kullanılabilecek her yerde güncelleştirmenizi öneririz (zaman serisi modeli, Kaydedilen sorgular, Power BI bağlayıcı sorguları).

Durum 4: **özel bir ön uç uygulaması derlemek Için JavaScript SDK 'Sını kullanma**

Yukarıdaki 1-3 servis taleplerine etkilenmiş ve özel uygulamalar oluşturursanız, sorgularınızı, Yukarıdaki örneklerde gösterildiği gibi, *birleşim ()* işlevini kullanacak şekilde güncelleştirmeniz gerekir.

Durum 5: **Isınma süresi 1.000 Özellik sınırına yaklaşıyor**

Çok sayıda özelliği olan bir yarı bir mağaza kullanıcılarınız varsa ve bu değişikliğin ortamınızı 1.000 WS Özellik adı sınırı üzerinden göndermesini düşünüyorsanız, Azure portal bir destek bileti gönderin ve bu iletişimin bahsetmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

* Desteklenen veri türlerinin tam listesini görüntülemek için [Bu](concepts-supported-data-types.md) makaleye bakın.
