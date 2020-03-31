---
title: Zaman Serisi Modeli - Azure Time Serisi Öngörüleri | Microsoft Dokümanlar
description: Azure Time Series Öngörüleri Önizlemesinde Zaman Serisi Modeli hakkında bilgi edinin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 03/16/2020
ms.custom: seodec18
ms.openlocfilehash: 648578563a0e53d3ed5bda6ab47f85c3c6a2a24e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79476663"
---
# <a name="time-series-model-in-azure-time-series-insights-preview"></a>Azure Time Serisi Öngörüler Önizlemezaman Serisi Modeli

Bu makalede, Azure Zaman Serisi Öngörüleri Önizleme ortamında Zaman Serisi Modeli, yetenekleri ve kendi modellerinizi oluşturmaya ve güncelleştirmeye nasıl başlayacağınızı açıklayınız.

> [!TIP]
>  * Canlı Zaman Serisi Modeli örneği için [Contoso Wind Farm demo](https://insights.timeseries.azure.com/preview/samples) ortamına gidin.
> * Zaman Serisi Model UI'nizde nasıl gezineceğimize dair bilgi edinmek için [Azure Zaman Serisi Öngörüleri Önizleme gezgini](time-series-insights-update-explorer.md) hakkında bilgi edinin.
> * Time Series Insights web explorer'ı kullanarak [Time Series Modeli ile nasıl çalışacağınızı](time-series-insights-update-how-to-tsm.md) öğrenin.

## <a name="summary"></a>Özet

Geleneksel olarak, IoT aygıtlarından toplanan veriler bağlamsal bilgilerden yoksunolduğundan, sensörleri hızlı bir şekilde bulmayı ve analiz etmeyi zorlaştırır. Time Serisi Modeli'nin temel motivasyonu, IoT veya Time Series verilerini bulmayı ve analiz etmeyi basitleştirmektir. Tüketiciye hazır veri kümelerini analitik için hazırlamaya yardımcı olmak için zaman serisi verilerinin kürasyonunu, bakımını ve zenginleştirmesini sağlayarak bu amaca ulaşır.

## <a name="scenario-contosos-new-smart-oven"></a>Senaryo: Contoso yeni akıllı fırın

**Contoso akıllı fırının hayali senaryosunu düşünün.** Bu senaryoda, her Contoso akıllı fırın beş sıcaklık sensörleri, dört üst brülör her biri için bir ve fırın kendisi için bir olduğunu varsayalım. Yakın zamana kadar, her Contoso sıcaklık sensörü verilerini tek tek gönderdi, depolandı ve görselleştirdi. Mutfak aletleri nin izlenmesi için Contoso, her bir sensör için bir tane olan temel grafiklere dayanıyordu.

Contoso ilk veri ve görselleştirme çözümünden memnun olsa da, çeşitli sınırlamalar ortaya çıktı:

* Müşteriler, en iyi brülörlerin çoğu açıkken genel fırının ne kadar ısınacağını bilmek istediler. Contoso, genel fırının koşulları hakkında birleşik bir cevap bulmakta ve analiz etmekte daha fazla zorluk çekti.
* Contoso mühendisleri, üst brülörlerin aynı anda çalıştırılmasının verimsiz güç çekmeyle sonuçlanmayacağını doğrulamak istediler. Hangi sıcaklık ve gerilim sensörlerinin birbiriyle ilişkili olduğunu ve mağazada nasıl bulunup bulunabildiğini çapraz yönlendirmede güçlük vardı.
* Contoso kalite güvence ekibi, iki sensör versiyonu arasındaki geçmişi denetlemek ve karşılaştırmak istedi. Hangi verilerin hangi sensör sürümüne ait olduğunu belirlemekte güçlük vardı.

Kapsamlı akıllı fırın zaman serisi modelini yapılandırma, düzenleme ve tanımlama yeteneği olmadan, her sıcaklık sensörü yerinden edilmiş, izole edilmiş ve daha az bilgilendirici veri noktalarına sahip olur. Her veri kümesi diğerlerinden bağımsız olarak yaşadığı için bu veri noktalarını eyleme geçirilebilir öngörülere dönüştürmek daha zordu.

Bu sınırlamalar Contoso'nun yeni fırınına eşlik edecek akıllı veri toplama ve görselleştirme araçlarının önemini ortaya koymuştur:

* Verileri ilişkilendirip uygun bir görünümde birleştirebildiğinizde veri görselleştirme kullanışlıdır. Bir örnek sıcaklık sensörleri ile birlikte voltaj sensörleri gösteriyor.
* Karşılaştırma, yakınlaştırma ve zaman aralığı işlevlerinin yanı sıra birden fazla varlık için çok boyutlu verileri yönetmek zor olabilir.

**Zaman Serisi Modeli,** bu hayali örnekte karşılaşılan senaryoların çoğu için kullanışlı bir çözüm sağlar:

[![Zaman Serisi Model akıllı fırın grafik örneği](media/v2-update-tsm/time-series-model-smart-oven.png)](media/v2-update-tsm/time-series-model-smart-oven.png#lightbox)

* Zaman Serisi Modeli sorgularda ve gezinmede hayati bir rol oynar, çünkü karşılaştırmaların zaman aralıkları arasında ve sensör ve aygıt türleri arasında çizilmesine izin vererek verileri bağlamsallaştırır. (**A**) 
* Zaman Serisi Modeli'nde kalıcı veriler zaman serisi sorgu hesaplamalarını değişken olarak koruduğu ndan ve sorgu zamanında yeniden kullandığından veriler daha da bağlamsallaştırılır.
* Time Series Model, gelişmiş görselleştirme ve yönetim yetenekleri için verileri düzenler ve toplar. (**B**) 

### <a name="key-capabilities"></a>Temel işlevler

Zaman serisi bağlamsallaştırmayı basit ve zahmetsiz hale getirmek amacıyla Time Series Model, Time Series Insights Preview'da aşağıdaki özellikleri sağlar. Size yardımcı olur:

* Skaler işlevler, toplu işlemler ve benzeri nedenlerle hesaplamaları veya formülleri yazar ve yönetin.
* Gezinmeyi, aramayı ve başvuruları etkinleştirmek için üst-alt ilişkilerini tanımlayın.
* *Örnek alanları*olarak tanımlanan örneklerle ilişkili özellikleri tanımlayın ve bunları hiyerarşioluşturmak için kullanın.

### <a name="components"></a>Bileşenler

Zaman Serisi Modeli üç temel bileşene sahiptir:

* [Zaman Serisi Model örnekleri](#time-series-model-instances)
* [Zaman Serisi Model hiyerarşileri](#time-series-model-hierarchies)
* [Zaman Serisi Model türleri](#time-series-model-types)

Bu bileşenler, bir zaman serisi modelini belirtmek ve Azure Zaman Serisi Öngörüleri verilerinizi düzenlemek için birleştirilir.

[![Zaman Serisi Modeli genel bakış grafiği](media/v2-update-tsm/time-series-model-overview.png)](media/v2-update-tsm/time-series-model-overview.png#lightbox)

Zaman serisi [modeli, Zaman Serisi Öngörüleri Önizleme](time-series-insights-update-how-to-tsm.md) arabirimi aracılığıyla oluşturulabilir ve yönetilebilir. Zaman Serisi Model ayarları [Model Ayarları API](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api)üzerinden yönetilebilir.

## <a name="time-series-model-instances"></a>Zaman Serisi Model örnekleri

Zaman Serisi Model *örnekleri,* zaman serisinin sanal gösterimleridir.

Çoğu durumda, örnekler benzersiz **deviceId** veya **assetId**tarafından tanımlanır , hangi zaman serisi kimlikleri olarak kaydedilir.

Örnekler, zaman serisi kimliği, tür, ad, açıklama, hiyerarşiler ve örnek alanları gibi *örnek özellikleri*olarak adlandırılan onlarla ilişkili açıklayıcı bilgilere sahiptir. En azından, örnek özellikleri hiyerarşi bilgilerini içerir.

*Örnek alanları,* hiyerarşi düzeyleriiçin değerlerin yanı sıra üretici, işleç ve benzeri değerleri içerebilen açıklayıcı bilgiler topluluğu.

Bir olay kaynağı Zaman Serisi Öngörüleri ortamı için yapılandırıldıktan sonra, örnekler otomatik olarak keşfedilir ve bir zaman serisi modelinde oluşturulur. Örnekler, Time Series Model sorguları kullanılarak Zaman Serisi Öngörüleri explorer aracılığıyla oluşturulabilir veya güncelleştirilebilir.

[Contoso Rüzgar Çiftliği demo](https://insights.timeseries.azure.com/preview/samples) birkaç canlı örnek örnekler sağlar.

[![Zaman Serisi Model örneği örneği](media/v2-update-tsm/time-series-model-instance.png)](media/v2-update-tsm/time-series-model-instance.png#lightbox)

### <a name="instance-properties"></a>Örnek özellikleri

Örnekler **timeSeriesId,** **typeId,** **ad,** **açıklama,** **hiyerarşiId'ler**ve **instanceFields**tarafından tanımlanır. Her örnek yalnızca bir *tür*ve bir veya daha fazla *hiyerarşieşler.*

| Özellik | Açıklama |
| --- | ---|
| timeSeriesId | Örneğin ilişkili olduğu zaman serisinin UUID'si. |
| Typeıd | Zaman Serisi Modeli nin UUID örneği ile ilişkilidir. Varsayılan olarak, keşfedilen tüm yeni örnekler varsayılan bir türle ilişkilendirilir.
| ad | **Ad** özelliği isteğe bağlıdır ve büyük/küçük harf duyarlıdır. **Ad** kullanılamıyorsa, varsayılan olarak **timeSeriesId'e** Bir ad vereli, **timeSeriesId** [hala kuyuda](time-series-insights-update-explorer.md#4-time-series-well)kullanılabilir. |
| açıklama | Örneğin metin açıklaması. |
| hiyerarşiIds | Örneğin hangi hiyerarşilere ait olduğunu tanımlar. |
| instanceAlanlar | Bir örneğin özellikleri ve bir örneği tanımlayan statik veriler. Arama işlemleri gerçekleştirmek için dizin oluşturmayı desteklerken hiyerarşi veya hiyerarşi dışı özelliklerin değerlerini tanımlarlar. |

> [!NOTE]
> Hiyerarşiler örnek alanları kullanılarak oluşturulur. Ek **örnek Alanlar,** diğer örnek özellik tanımları için eklenebilir.

Örnekler aşağıdaki JSON gösterimi var:

```JSON
{
  "timeSeriesId": ["PU2"],
  "typeId": "545314a5-7166-4b90-abb9-fd93966fa39b",
  "hierarchyIds": ["95f0a8d1-a3ef-4549-b4b3-f138856b3a12"],
  "description": "Pump #2",
  "instanceFields": {
    "Location": "Redmond",
    "Fleet": "Fleet 5",
    "Unit": "Pump Unit 3",
    "Manufacturer": "Contoso",
    "ScalePres": "0.54",
    "scaleTemp": "0.54"
  }
}
```

> [!TIP]
> Zaman Serisi Öngörüler Örnek API ve oluşturmak, okumak, güncelleme küçlükve (CRUD) desteği silmek için, [Veri sorgu makalesini](time-series-insights-update-tsq.md#time-series-model-query-tsm-q-apis) ve [Örnek API REST belgelerini](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api)okuyun.

## <a name="time-series-model-hierarchies"></a>Zaman Serisi Model hiyerarşileri

Zaman Serisi Model *hiyerarşileri* özellik adlarını ve ilişkilerini belirterek örnekleri düzenler.

Belirli bir Zaman Serisi Öngörüleri ortamında birden çok hiyerarşiyi yapılandırabilirsiniz. Zaman Serisi Modeli örneği, tek bir hiyerarşiyle veya birden çok hiyerarşiyle (çok-çok ilişkisi) eşlenebilir.

[Contoso Wind Farm demo](https://insights.timeseries.azure.com/preview/samples) istemci arabirimi standart bir örnek ve tür hiyerarşisi görüntüler.

[![Zaman Serisi Model hiyerarşisi örneği](media/v2-update-tsm/time-series-model-hierarchies.png)](media/v2-update-tsm/time-series-model-hierarchies.png#lightbox)

### <a name="hierarchy-definition"></a>Hiyerarşi tanımı

Hiyerarşiler hiyerarşi **id**, **ad**ve **kaynak**tarafından tanımlanır.

| Özellik | Açıklama |
| ---| ---|
| id | Hiyerarşi için benzersiz tanımlayıcı, örneğin, bir örnek tanımlarken kullanılır. |
| ad | Hiyerarşi için bir ad sağlamak için kullanılan bir dize. |
| source | Kullanıcıların oluşturmak istediği hiyerarşinin yukarıdan aşağıya üst-alt sırası olan kuruluş hiyerarşisini veya yolu belirtir. Üst-alt özellikleri eşlenek örnek alanları. |

Hiyerarşiler JSON'da şu şekilde temsil edilir:

```JSON
{
  "hierarchies": [
    {
      "id": "6e292e54-9a26-4be1-9034-607d71492707",
      "name": "Location",
      "source": {
        "instanceFieldNames": [
          "state",
          "city"
        ]
      }
    },
    {
      "id": "a28fd14c-6b98-4ab5-9301-3840f142d30e",
      "name": "ManufactureDate",
      "source": {
        "instanceFieldNames": [
          "year",
          "month"
        ]
      }
    }
  ]
}
```

Önceki JSON örneğinde:

* `Location`üst `states` ve alt `cities`ile bir hiyerarşi tanımlar. Her `location` biri `states`birden fazla olabilir, `cities`sırayla birden fazla olabilir.
* `ManufactureDate`üst `year` ve alt `month`ile bir hiyerarşi tanımlar. Her `ManufactureDate` biri `years`birden fazla olabilir, `months`sırayla birden fazla olabilir.

> [!TIP]
> Zaman Serisi Öngörüler Örneği API ve CRUD desteği için Veri [sorgumakalesini](time-series-insights-update-tsq.md#time-series-model-query-tsm-q-apis) ve [Hiyerarşi API REST belgelerini](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api)okuyun.

### <a name="hierarchy-example"></a>Hiyerarşi örneği

**Hiyerarşi H1'in** `building`, ve `floor`onun `room` **örnek Alan Adları** tanımının bir parçası olduğu bir örnek düşünün:

```JSON
{
  "id": "aaaaaa-bbbbb-ccccc-ddddd-111111",
  "name": "H1",
  "source": {
    "instanceFieldNames": [
      "building",
      "floor",
      "room"
    ]
  }
}
```

Önceki tanımda ve birkaç zaman dizisinde kullanılan örnek alanları göz önüne alındığında, hiyerarşi öznitelikleri ve değerleri aşağıdaki tabloda gösterildiği gibi görünür:

| Zaman Serisi Kimliği | Örnek alanlar |
| --- | --- |
| ID1 | "bina" = "1000", "kat" = "10", "oda" = "55"  |
| ID2 | "bina" = "1000", "oda" = "55" |
| ID3 | "kat" = "10" |
| ID4 | "bina" = "1000", "kat" = "10"  |
| ID5 | "Bina", "kat" veya "oda" hiçbiri ayarlı değildir. |

Zaman Serisi **ID1** ve **ID4,** [Azure Zaman Serisi Öngörüler gezgininde](time-series-insights-update-explorer.md) **H1** hiyerarşisinin bir parçası olarak görüntülenir, çünkü *bina,* *zemin*ve *oda* parametrelerini tam olarak tanımlamış ve doğru şekilde sıralamış.

Diğerleri, belirtilen veri hiyerarşisine uymadığından *Ebeveynsiz Örnekler* altında sınıflandırılır.

## <a name="time-series-model-types"></a>Zaman Serisi Model türleri

Zaman Serisi Model *türleri,* hesaplamalar yapmak için değişkenleri veya formülleri tanımlamanıza yardımcı olur. Türleri belirli bir Time Series Insights örneği ile ilişkilidir.

Bir türbir veya daha fazla değişken olabilir. Örneğin, bir Zaman Serisi Model örneği, *avg sıcaklık,* *min sıcaklığı*ve *maksimum sıcaklık*değişkenlerinden oluşan *sıcaklık sensörü*türünden olabilir.

[Contoso Rüzgar Çiftliği demo,](https://insights.timeseries.azure.com/preview/samples) kendi örnekleri ile ilişkili birkaç Zaman Serisi Model türleri görselleştirir.

[![Zaman Serisi Model türü örneği](media/v2-update-tsm/time-series-model-types.png)](media/v2-update-tsm/time-series-model-types.png#lightbox)

> [!TIP]
> Zaman Serisi Öngörüler Örneği API ve CRUD desteği için Veri [sorgu makalesini](time-series-insights-update-tsq.md#time-series-model-query-tsm-q-apis) ve [Tip API REST belgelerini](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api)okuyun.

### <a name="type-properties"></a>Tür özellikleri

Zaman Serisi Model türleri **id,** **isim,** **açıklama**ve **değişkenler**ile tanımlanır.

| Özellik | Açıklama |
| ---| ---|
| id | Türü için UUID. |
| ad | Türü için bir ad sağlamak için kullanılan bir dize. |
| açıklama | Türü için bir dize açıklaması. |
| Değişken | Türle ilişkili değişkenleri belirtin. |

Türleri aşağıdaki JSON örneğine uygundur:

```JSON
{
  "types": [
    {
      "id": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
      "name": "DefaultType",
      "description": "Default type",
      "variables": {
        "EventCount": {
          "kind": "aggregate",
          "value": null,
          "filter": null,
          "aggregation": {
            "tsx": "count()"
          }
        },
        "Interpolated Speed": {
          "kind": "numeric",
          "value": {
              "tsx": "$event.[speed].Double"
          },
          "filter": null,
          "interpolation": {
              "kind": "step",
              "boundary": {
                  "span": "P1D"
              }
          },
          "aggregation": {
              "tsx": "left($value)"
          }
        }
      }
    }
  ]
}
```

### <a name="variables"></a>Değişkenler

Zaman Serisi Öngörüleri türleri, olaylarla ilgili formül ve hesaplama kurallarını belirten birçok değişkene sahip olabilir.

Her değişken üç *türden*biri olabilir: *sayısal*, *kategorik*, ve *toplam*.

* **Sayısal** türler sürekli değerlerle çalışır. 
* **Kategorik** türler, tanımlanmış bir ayrıdeğer kümesiyle çalışır.
* **Toplam** değerler tek bir tür (tüm sayısal veya tüm kategorik) birden çok değişkeni birleştirir.

Aşağıdaki tablo, her değişken türü için hangi özelliklerin ilgili olduğunu gösterir.

[![Zaman Serisi Model değişken tablosu](media/v2-update-tsm/time-series-model-variable-table.png)](media/v2-update-tsm/time-series-model-variable-table.png#lightbox)

#### <a name="numeric-variables"></a>Sayısal değişkenler

| Değişken özellik | Açıklama |
| --- | ---|
| Değişken filtre | Filtreler, hesaplama için düşünülen satır sayısını kısıtlamak için isteğe bağlı koşullu yan tümcelerdir. |
| Değişken değer | Cihazdan veya sensörlerden gelen veya Zaman Serisi İfadeleri kullanılarak dönüştürülen hesaplama için kullanılan telemetri değerleri. Sayısal tür değişkenler *Çift*türünden olmalıdır.|
| Değişken enterpolasyon | Enterpolasyon, varolan verileri kullanarak bir sinyali nasıl yeniden oluşturacağız belirtir. *Sayısal* değişkenler için adım ve *Doğrusal* enterpolasyon seçenekleri mevcuttur. |
| Değişken toplama | *Avg*, *Min*, *Max*, *Sum*, *Count*, *First*, *Son* ve zaman ağırlıklı (*Avg*, *Min*, *Max*, *Sum*, *Sol*) operatörleri ile destek hesaplama. |

Değişkenler aşağıdaki JSON örneğine uygundur:

```JSON
"Interpolated Speed": {
  "kind": "numeric",
  "value": {
    "tsx": "$event.[speed].Double"
  },
  "filter": null,
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span": "P1D"
    }
  },
  "aggregation": {
    "tsx": "left($value)"
  }
}
```

#### <a name="categorical-variables"></a>Kategorik değişkenler

| Değişken özellik | Açıklama |
| --- | ---|
| Değişken filtre | Filtreler, hesaplama için düşünülen satır sayısını kısıtlamak için isteğe bağlı koşullu yan tümcelerdir. |
| Değişken değer | Cihazdan veya sensörlerden gelen hesaplama için kullanılan telemetri değerleri. Kategorik tür değişkenler *Uzun* veya *String*olmalıdır. |
| Değişken enterpolasyon | Enterpolasyon, varolan verileri kullanarak bir sinyali nasıl yeniden oluşturacağız belirtir. *Adım* enterpolasyon seçeneği kategorik değişkenler için kullanılabilir. |
| Değişken kategoriler | Kategoriler, aygıttan gelen değerler veya sensörler arasında bir etikete eşleme oluşturur. |
| Değişken varsayılan kategori | Varsayılan kategori, "kategoriler" özelliğinde eşlenen olmayan tüm değerler içindir. |

Değişkenler aşağıdaki JSON örneğine uygundur:

```JSON
"Status": {
  "kind": "categorical",
  "value": {
     "tsx": "toLong($event.[Status].Double)"
},
  "interpolation": {
    "kind": "step",
    "boundary": {
      "span" : "PT1M"
    }
  },
  "categories": [
    {
      "values": [0, 1, 2],
      "label": "Good"
    },
    {
      "values": [3],
      "label": "Bad"
    }
  ],
  "defaultCategory": {
    "label": "Not Applicable"
  }
}
```

#### <a name="aggregate-variables"></a>Toplam değişkenler

| Değişken özellik | Açıklama |
| --- | ---|
| Değişken filtre | Filtreler, hesaplama için düşünülen satır sayısını kısıtlamak için isteğe bağlı koşullu yan tümcelerdir. |
| Değişken toplama | *Avg,* *Min,* *Max*, *Sum*, *Count*, *First*, *Last*ile destek hesaplaması . |

Değişkenler aşağıdaki JSON örneğine uygundur:

```JSON
"Aggregate Speed": {
  "kind": "aggregate",
  "filter": null,
  "aggregation": {
    "tsx": "avg($event.Speed.Double)"
  }
}
```

Değişkenler bir zaman serisi modelinin tür tanımında depolanır ve depolanan tanımı geçersiz kılmak için [Sorgu API'leri](time-series-insights-update-tsq.md) aracılığıyla satır satır da sağlanabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Zaman Serisi Öngörüleri Önizleme depolama ve giriş'i](./time-series-insights-update-storage-ingress.md)okuyun.

- Azure Zaman Serisi [Öngörüleri Önizlemesinde Veri modellemesinde](./time-series-insights-update-how-to-tsm.md) sık karşılaşılan Zaman Serisi Modeli işlemleri hakkında bilgi edinin

- Yeni [Time Series Model](https://docs.microsoft.com/rest/api/time-series-insights/preview-model) başvuru belgelerini okuyun.
