---
title: Azure Time Series Insights Preview 'da zaman serisi modeli | Microsoft Docs
description: Azure Time Series Insights zaman serisi modelini anlama.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 10/07/2019
ms.custom: seodec18
ms.openlocfilehash: 8f4ed6de5ff47efa441c371b80670c500f57c9bb
ms.sourcegitcommit: f9e81b39693206b824e40d7657d0466246aadd6e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72034054"
---
# <a name="time-series-model"></a>Zaman Serisi Modeli

Bu makalede Azure Time Series Insights önizlemenin zaman serisi modeli bölümü açıklanır. Modelin kendisini, yeteneklerini ve kendi modelinizi oluşturmaya ve güncelleştirmeye nasıl başladığına ilişkin bir ele alınmaktadır.

Geleneksel olarak, IoT cihazlarından toplanan verilerin bağlamsal bilgileri yoktur, bu da algılayıcıların hızla bulunup çözümlenmesini zorlaştırır. Zaman serisi modeli için ana işlem, IoT verilerinin bulunmasını ve çözümlenmesini basitleştirmeye yöneliktir. Tüketiciye yönelik veri kümelerini hazırlamaya yardımcı olmak üzere zaman serisi verilerinin görselleştirme, bakım ve zenginleştirmesini etkinleştirerek bu amaca erişir.

Zaman serisi modelleri, cihaz ve cihaz dışı varlıkları dışladığı için sorgularda ve gezintide önemli bir rol oynar. Zaman serisi modelinde kalıcı olan veriler, içinde depolanan formüllerin avantajlarından yararlanarak zaman serisi sorgu hesaplamaları 'nı güçlendirir.

[![ zaman serisi modeline genel bakış](media/v2-update-tsm/tsm.png)](media/v2-update-tsm/tsm.png#lightbox)

## <a name="key-capabilities"></a>Temel işlevler

Zaman serisi bağlamı 'nı yönetmeyi basit ve kolay hale getirmek için amaç sayesinde, zaman serisi modeli Time Series Insights önizlemede aşağıdaki özellikleri sağlar. Size yardımcı olur:

* Hesaplamalar ve formülleri yazın ve yönetin, skalar işlevlerden yararlanarak verileri dönüştürün, toplu işlemleri yapın ve bu şekilde devam edin.
* Gezinti ve başvuruyu etkinleştirmek ve zaman serisi telemetrisine bağlam sağlamak için üst-alt ilişkileri tanımlayın.
* *Örnek alanlarının* örneklerle ilişkili özellikleri tanımlayın ve bunları hiyerarşi oluşturmak için kullanın.

## <a name="entity-components"></a>Varlık bileşenleri

Zaman serisi modellerinin üç çekirdek bileşeni vardır:

* <a href="#time-series-model-types">Zaman serisi model türleri</a>
* <a href="#time-series-model-hierarchies">Zaman serisi model hiyerarşileri</a>
* <a href="#time-series-model-instances">Zaman serisi model örnekleri</a>

Bu bileşenler, zaman serisi modeli belirtmek ve Azure Time Series Insights verilerinizi düzenlemek için birleştirilir.

## <a name="time-series-model-types"></a>Zaman serisi model türleri

Zaman serisi model *türleri* , hesaplamalar yapmak için değişkenler veya formüller tanımlamanıza yardımcı olur. Türler, belirli bir Time Series Insights örneğiyle ilişkilendirilir. Bir tür bir veya daha fazla değişkene sahip olabilir. Örneğin, bir Time Series Insights örneği, *Ortalama sıcaklık*, *en az sıcaklık*ve *en fazla sıcaklık*değişkenlerinden oluşan *sıcaklık algılayıcısı*türünde olabilir. Veriler Time Series Insights akışa başladığında varsayılan bir tür oluşturacağız. Varsayılan tür, model ayarlarından alınabilir ve güncelleştirilir. Varsayılan türlerin olay sayısını sayan bir değişkeni vardır.

### <a name="time-series-model-type-json-example"></a>Zaman serisi model türü JSON örneği

Örnek:

```JSON
{
    "name": "SampleType",
    "description": "This is sample type",
    "variables": {
        "Avg Temperature": {
            "kind": "numeric",
            "filter": null,
            "value": { "tsx": "$event.temperature.Double" },
            "aggregation": {"tsx": "avg($value)"}
        },
        "Count Temperature": {
            "kind": "aggregate",
            "filter": null,
            "value": null,
            "aggregation": {"tsx": "count()"}
        }
    }
}
```

Zaman serisi model türleri hakkında daha fazla bilgi için [başvuru belgelerine](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#types-api)bakın.

### <a name="variables"></a>Değişkenler

Time Series Insights türler, olaylardaki değerler üzerinde hesaplamalar olarak adlandırılan değişkenlere sahiptir. Time Series Insights değişken tanımları formül ve hesaplama kurallarını içerir. Değişken tanımlarına *tür*, *değer*, *filtre*, *azaltma*ve *sınırlar*dahildir. Değişkenler, zaman serisi modelindeki tür tanımında depolanır ve depolanan tanımı geçersiz kılmak için sorgu API 'Leri aracılığıyla satır içi olarak sağlanır.

Aşağıdaki matris, değişken tanımları için bir gösterge olarak kullanılabilir:

[![Değişken tanım tablosu türü](media/v2-update-tsm/table.png)](media/v2-update-tsm/table.png#lightbox)

| Tanım | Açıklama |
| --- | ---|
| Değişken türü |  *Sayısal* ve *Toplam* türleri desteklenir |
| Değişken filtresi | Değişken filtreleri, koşullara göre hesaplama için göz önünde bulundurulmakta olan satır sayısını kısıtlamak için isteğe bağlı bir filtre yan tümcesi belirtir. |
| Değişken değeri | Değişken değerleri, hesaplamada kullanılmalıdır. Söz konusu veri noktasına başvurmak için ilgili alan. |
| Değişken toplama | Değişkenin toplama işlevi, hesaplamanın bir parçasını sunar. Time Series Insights normal toplamları destekler (yani, *Min*, *Max*, *AVG*, *Sum*ve *Count*). |

## <a name="time-series-model-hierarchies"></a>Zaman serisi model hiyerarşileri

Hiyerarşiler, özellik adlarını ve bunların ilişkilerini belirterek örnekleri düzenler. Tek bir hiyerarşiniz veya birden çok hiyerarşiniz olabilir. Verilerinizin geçerli bir parçası olması gerekmez, ancak her örnek bir hiyerarşiye eşlenmelidir. Zaman serisi model örneği, tek bir hiyerarşiye veya birden çok hiyerarşiyle eşlenir.

Hiyerarşiler *HIYERARŞI kimliği*, *ad*ve *kaynak*tarafından tanımlanır. Hiyerarşiler, kullanıcıların oluşturmak istedikleri hiyerarşinin üst-alt öğe sırası olan bir yola sahiptir. Üst-alt özellikleri eşleme *örneği alanları*.

### <a name="time-series-model-hierarchy-json-example"></a>Zaman serisi model hiyerarşisi JSON örneği

Örnek:

```JSON
{
    "id": "4c6f1231-f632-4d6f-9b63-e366d04175e3",
    "name": "Location",
    "source": {
        "instanceFieldNames": [
                "state",
                "city"
            ]
    }
}
```

Zaman serisi model hiyerarşileri hakkında daha fazla bilgi için [başvuru belgelerine](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#hierarchies-api)bakın.

### <a name="hierarchy-definition-behavior"></a>Hiyerarşi tanımı davranışı

' Hiyerarşinin, tanımının bir parçası olarak *derleme*, *kat*ve *Oda* sahibi olduğu aşağıdaki örneği göz önünde bulundurun:

```plaintext
 H1 = [“building”, “floor”, “room”]
```

*Örnek alanlarına*bağlı olarak, hiyerarşi öznitelikleri ve değerleri aşağıdaki tabloda gösterildiği gibi görünür:

| Zaman serisi KIMLIĞI | Örnek alanları |
| --- | --- |
| ID1 | "derleme" = "1000", "Floor" = "10", "Oda" = "55"  |
| ID2 | "derleme" = "1000", "Oda" = "55" |
| ID3 | "Floor" = "10" |
| ID4 | "derleniyor" = "1000", "Floor" = "10"  |
| ID5 | "Derleme", "kat" veya "Oda" belirtilmemiş |

Yukarıdaki örnekte, **ID1** ve **ID4** , Azure Time Series Insights Explorer 'da H1 hiyerarşisinin bir parçası olarak gösterilir ve REST, belirtilen veri hiyerarşisine uygun olmadıkları Için *üst öğe olmayan örnekler* altında sınıflandırılmaktadır.

## <a name="time-series-model-instances"></a>Zaman serisi model örnekleri

Örnekler, zaman serisidir. Çoğu durumda, *DeviceID* veya *AssetID* , ortamdaki varlığın benzersiz tanımlayıcısıdır. Örnekler, örnek özellikleri olarak adlandırılan bunlarla ilişkili açıklayıcı bilgilere sahiptir. En azından, örnek özellikleri hiyerarşi bilgilerini içerir. Ayrıca üretici, operatör veya son hizmet tarihi gibi faydalı, açıklayıcı verileri de içerebilir.

Örnekler *TypeId*, *timeseriesıd*, *Name*, *Description*, *hierarchyıds*ve *ınstancefields*tarafından tanımlanır. Her örnek yalnızca bir *türe*ve bir veya daha fazla hiyerarşilere eşlenir. Örnekler Hiyerarşilerdeki tüm özellikleri miras alır ve diğer örnek özellik tanımı için ek *ınstancefields* eklenebilir.

*ınstancefields* , bir örneğin ve bir örneği tanımlayan herhangi bir statik verinin özellikleridir. Bunlar hiyerarşi veya hiyerarşi dışı özelliklerin değerlerini tanımlar, Ayrıca, arama işlemlerini gerçekleştirmek için dizin oluşturmayı da destekler.

*Name* özelliği isteğe bağlıdır ve büyük/küçük harfe duyarlıdır. *Ad* yoksa, varsayılan olarak zaman serisi kimliği olur. Bir *ad* sağlanmışsa, zaman serisi kimliği yine de kullanılabilir olacaktır (Gezgin 'deki grafiklerin altındaki kılavuz).

### <a name="time-series-model-instance-json-example"></a>Zaman serisi model örneği JSON örneği

Örnek:

```JSON
{
    "typeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393",
    "timeSeriesId": ["sampleTimeSeriesId"],
    "name": "sampleName",
    "description": "Sample Instance",
    "hierarchyIds": [
        "1643004c-0a84-48a5-80e5-7688c5ae9295"
    ],
    "instanceFields": {
        "state": "California",
        "city": "Los Angeles"
    }
}
```

Zaman serisi model örnekleri hakkında daha fazla bilgi için [başvuru belgelerine](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#instances-api)bakın.

### <a name="time-series-model-settings-example"></a>Zaman serisi model ayarları örneği

Örnek:

```JSON
{
    "modelSettings": {
        "name": "DefaultModel",
        "timeSeriesIdProperties": [
            {
                "name": "id",
                "type": "String"
            }
        ],
        "defaultTypeId": "1be09af9-f089-4d6b-9f0b-48018b5f7393"
    }
}
```

Zaman serisi modeli ayarları hakkında daha fazla bilgi için [başvuru belgelerine](https://docs.microsoft.com/rest/api/time-series-insights/preview-model#model-settings-api)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Time Series Insights Preview Storage and ınress](./time-series-insights-update-storage-ingress.md).

- Bkz. yeni [zaman serisi modeli](https://docs.microsoft.com/rest/api/time-series-insights/preview-model).