---
title: Azure Time Series Insights Gen2 'de uzun veri türü desteği | Microsoft Docs
description: Azure Time Series Insights Gen2 ' de uzun veri türü desteği.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/25/2020
ms.custom: dpalled
ms.openlocfilehash: 8d9b166d4ac9290a0389eac23f9d6882f667e1a0
ms.sourcegitcommit: b33c9ad17598d7e4d66fe11d511daa78b4b8b330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88856098"
---
# <a name="adding-support-for-long-data-type-in-azure-time-series-insights-gen2"></a>Azure Time Series Insights Gen2 ' de uzun veri türü desteği ekleniyor

Uzun veri türü desteğinin eklenmesi, sayısal verileri yalnızca Azure Time Series Insights Gen2 ortamlarında depolamanıza ve dizinliyoruz. Bir Gen1 ortamınız varsa, bu değişiklikleri yoksayabilirsiniz.

Bölgenize bağlı olarak, 29 Haziran veya 30 Haziran 2020 ' den başlayarak, verileriniz **uzun** ve **çift**olarak dizinlenir.  Bu değişiklik hakkında sorularınız veya endişeleriniz varsa Azure portal aracılığıyla bir destek bileti iletin ve bu iletişimin bahsetmesi gerekir.

Aşağıdaki durumlardan herhangi biriyle etkileniyorsanız önerilen değişiklikleri yapın:

- **Durum 1**: Şu anda zaman serisi model değişkenlerini kullanıyorsunuz ve telemetri verilerinizde yalnızca integral veri türleri gönderiyorsunuz.
- **Durum 2**: Şu anda zaman serisi model değişkenlerini kullanıyorsunuz ve telemetri verilerinizde hem integral hem de tamsayı olmayan veri türleri gönderiyorsunuz.
- **Durum 3**: tamsayı değerlerini kategorilere eşlemek için kategorik değişkenler kullanırsınız.
- **Durum 4**: JavaScript SDK 'sını kullanarak özel bir ön uç uygulaması oluşturursunuz.
- **Durum 5**: sıcak mağazadaki 1.000-Özellik adı sınırına yaklaşıyor ve hem integral hem de İntegral olmayan verileri gönderiyoruz. Özellik sayısı [Azure Portal](https://portal.azure.com/)bir ölçüm olarak görüntülenebilir.

Herhangi bir servis talebi sizin için geçerlidir, modelinizde değişiklik yapın. Değişken tanımınızdaki zaman serisi Ifadesini (TSX) önerilen değişikliklerle güncelleştirin. Her ikisini de güncelleştir:

- Azure Time Series Insights TSI Gezgini
- API 'lerimizi kullanan tüm özel istemciler

IoT çözümünüze ve kısıtlamalarına bağlı olarak, Azure Time Series Insights Gen2 ortamınıza gönderilen verilere ilişkin görünürlüğe sahip olmayabilirsiniz. Verilerinizin yalnızca integral veya integral ve İntegral olmayan bir değer mi olduğunu bilmiyorsanız, birkaç seçeneğiniz vardır:

- Özelliğin serbest bırakılacağını bekleyebilirsiniz. Ardından, hangi özelliklerin iki ayrı sütunda kaydedildiğini anlamak için Gezgin Kullanıcı arabirimindeki ham olaylarınızı keşfedebilirsiniz.
- Preemptively tüm sayısal etiketler için önerilen değişiklikleri yapabilirsiniz.
- Şemayı daha iyi anlamak ve araştırmak için bir olay alt kümesini geçici olarak depolamaya yönlendirebilirsiniz.

Olayları depolamak için Azure Event Hubs [olay yakalamayı](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview) açın veya IoT Hub Azure Blob depolama alanına [yönlendirin](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c#azure-storage) .

Veriler ayrıca [Olay Hub 'ı Gezgini](https://marketplace.visualstudio.com/items?itemName=Summer.azure-event-hub-explorer)aracılığıyla veya [olay işleyicisi ana bilgisayarı](https://docs.microsoft.com/azure/event-hubs/event-hubs-dotnet-standard-getstarted-send#receive-events)kullanılarak da gözlemlenebilir.

IoT Hub kullanıyorsanız yerleşik uç noktaya nasıl erişildiği [yerleşik uç noktadan cihazdan buluta Iletileri oku](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-read-builtin) bölümüne gidin.

> [!NOTE]
> Önerilen değişiklikleri yapmazsanız kesintiye uğramazsınız. Örneğin, sorgu API 'Leri veya Time Series Insights Gezgini aracılığıyla erişilen etkilenen Time Series Insights değişkenleri **null** döndürür (yani, Gezgin 'de veri göstermez).

## <a name="recommended-changes"></a>Önerilen değişiklikler

### <a name="case-1-using-time-series-model-variables-and-sending-only-integral-data-types-in-telemetry-data"></a>Durum 1: telemetri verilerinde zaman serisi model değişkenlerini kullanma ve yalnızca integral veri türleri gönderme

1. durum için önerilen değişiklikler, durum 2 ' deki ile aynıdır. Durum 2 için bölümündeki yönergeleri izleyin.

### <a name="case-2-using-time-series-model-variables-and-sending-both-integral-and-nonintegral-types-in-telemetry-data"></a>Durum 2: telemetri verilerinde zaman serisi model değişkenlerini kullanma ve hem integral hem de İntegral olmayan türler gönderme

Şu anda tamsayı telemetri verileri gönderiyorsanız, verileriniz iki sütuna bölünecektir:

- **propertyValue_double**
- **propertyValue_long**

Tamsayı verileriniz **propertyValue_long**yazar. **PropertyValue_double** ' de önceden alınan (ve gelecek) sayısal veriler, üzerine kopyalanmaz.

**PropertyValue** özelliği için bu iki sütun arasında verileri sorgulamak istiyorsanız, TSX 'inizdeki **birleşim ()** skaler işlevini kullanmanız gerekir. İşlev, aynı **veri türünün** bağımsız değişkenlerini kabul eder ve bağımsız değişken listesindeki ilk null olmayan değeri döndürür. Daha fazla bilgi için bkz. [Gen2 Data Access concepts Azure Time Series Insights](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax#other-functions).

#### <a name="variable-definition-in-tsx---numeric"></a>TSX 'de değişken tanımı-sayısal

*Önceki değişken tanımı:*

[![Önceki değişken tanımı](media/time-series-insights-long-data-type/var-def-previous.png)](media/time-series-insights-long-data-type/var-def-previous.png#lightbox)

*Yeni değişken tanımı:*

[![Yeni değişken tanımı](media/time-series-insights-long-data-type/var-def.png)](media/time-series-insights-long-data-type/var-def.png#lightbox)

**Birleşim ($Event. PropertyValue. Double, toDouble ($Event. PropertyValue. Long))** özel [zaman serisi ifadesi](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax)olarak da kullanabilirsiniz.

#### <a name="inline-variable-definition-using-tsx-query-apis---numeric"></a>TSX sorgu API 'Leri kullanarak satır içi değişken tanımı-sayısal

*Önceki değişken tanımı:*

```JSON
"PropertyValueVariable": {

    "kind": "numeric",

    "value": {

        "tsx": "$event.propertyValue.Double"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
    }
}
```

*Yeni değişken tanımı:*

```JSON
"PropertyValueVariable ": {

    "kind": "numeric",

    "value": {

        "tsx": "coalesce($event.propertyValue.Long, toLong($event.propertyValue.Double))"

    },

    "filter": null,

    "aggregation": {

        "tsx": "avg($value)"
    }
}
```

**Birleşim ($Event. PropertyValue. Double, toDouble ($Event. PropertyValue. Long))** özel [zaman serisi ifadesi](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax)olarak da kullanabilirsiniz.

> [!NOTE]
> Bu değişkenleri, kullanılabilecek her yerde güncelleştirmenizi öneririz. Bu konumlar zaman serisi modeli, Kaydedilen sorgular ve Power BI bağlayıcı sorgularını içerir.

### <a name="case-3-using-categorical-variables-to-map-integer-values-to-categories"></a>Durum 3: tamsayı değerlerini kategorilere eşlemek için kategorik değişkenler kullanma

Şu anda, tamsayı değerlerini kategorilere eşleştiren kategorik değişkenler kullanıyorsanız, verileri **çift** türden **uzun** türe dönüştürmek Için büyük olasılıkla **tolong** işlevini kullanıyorsunuz demektir. 1 ve 2. durumlarda olduğu gibi, **Double** ve **Long** **DataType** sütunlarını da eklemeniz gerekir.

#### <a name="variable-definition-in-time-series-explorer---categorical"></a>Zaman serisi Gezgininde değişken tanımı-kategorik

*Önceki değişken tanımı:*

[![Önceki değişken tanımı](media/time-series-insights-long-data-type/var-def-cat-previous.png)](media/time-series-insights-long-data-type/var-def-cat-previous.png#lightbox)

*Yeni değişken tanımı:*

[![Yeni değişken tanımı](media/time-series-insights-long-data-type/var-def-cat.png)](media/time-series-insights-long-data-type/var-def-cat.png#lightbox)

**Birleşim ($Event. PropertyValue. Double, toDouble ($Event. PropertyValue. Long))** özel [zaman serisi ifadesi](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax)olarak da kullanabilirsiniz.

Kategorik değişkenler yine de değerin bir tamsayı türünde olmasını gerektirir. Özel [zaman serisi ifadesinde](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax) , **birleşim ()** Içindeki tüm bağımsız değişkenlerin **veri** türü **Long** türünde olmalıdır.

#### <a name="inline-variable-definition-using-tsx-query-apis---categorical"></a>TSX sorgu API 'Leri kullanarak satır içi değişken tanımı-kategorik

*Önceki değişken tanımı:*

```JSON
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

```JSON
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

Kategorik değişkenler yine de değerin bir tamsayı türünde olmasını gerektirir. Özel [zaman serisi ifadesinde](https://docs.microsoft.com/rest/api/time-series-insights/reference-time-series-expression-syntax), **birleşim ()** Içindeki tüm bağımsız değişkenlerin **veri** türü **Long** türünde olmalıdır.

> [!NOTE]
> Bu değişkenleri, kullanılabilecek her yerde güncelleştirmenizi öneririz. Bu konumlar zaman serisi modeli, Kaydedilen sorgular ve Power BI bağlayıcı sorgularını içerir.

### <a name="case-4-using-the-javascript-sdk-to-build-a-custom-front-end-application"></a>Durum 4: özel bir ön uç uygulaması derlemek için JavaScript SDK 'sını kullanma

1 ile 3 arası durumlardan etkileniyorsanız ve özel uygulamalar oluşturursanız, önceki örneklerde gösterildiği gibi sorguları, **birleşim ()** işlevini kullanacak şekilde güncelleştirmeniz gerekir.

### <a name="case-5-nearing-warm-store-1000-property-limit"></a>Durum 5: ısınma süresi 1.000 Özellik sınırına yaklaşıyor

Çok sayıda özelliği olan bir yarı bir mağaza kullanıcılarınız varsa ve bu değişikliğin ortamınızı 1.000 ısınma deposu özelliği-adı sınırına göndermesini düşünüyorsanız, Azure portal bir destek bileti gönderin ve bu iletişimin bahsetmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Desteklenen veri türlerinin](concepts-supported-data-types.md)tam listesini görüntüleyin.
