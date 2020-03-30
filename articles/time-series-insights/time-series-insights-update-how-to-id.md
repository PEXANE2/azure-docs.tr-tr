---
title: Zaman Serisi Kimliği seçmek için en iyi uygulamalar - Azure Time Series Insights | Microsoft Dokümanlar
description: Azure Zaman Serisi Öngörüleri Önizlemesinde Zaman Serisi Kimliği seçerken en iyi uygulamalar hakkında bilgi edinin.
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 02/07/2020
ms.custom: seodec18
ms.openlocfilehash: a62c2460698408f6a2bfa51c6638bdeaf88bb31f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77083535"
---
# <a name="best-practices-for-choosing-a-time-series-id"></a>Zaman Serisi kimliği seçmek için en iyi uygulamalar

Bu makalede, Azure Zaman Serisi Öngörüler Önizleme ortamınız için Zaman Serisi Kimliğinin önemi ve birini seçmek için en iyi uygulamalar özetlenmiştir.

## <a name="choose-a-time-series-id"></a>Zaman Serisi Kimliği

Uygun bir Time Series kimliği seçmek çok önemlidir. Zaman Serisi Kimliği seçmek, veritabanı için bir bölüm anahtarı seçmeye benzer. Bir Zaman Serisi Öngörüönizleme ortamı oluşturduğunuzda gereklidir. 

> [!IMPORTANT]
> Zaman Serisi D'leri şunlardır:
> * *Büyük/küçük harf duyarlı* bir özellik: harf ve karakter kovanları aramalarda, karşılaştırmalarda, güncelleştirmelerde ve bölümleme de kullanılır.
> * *Değişmez* bir özellik: oluşturulduktan sonra değiştirilemez.

> [!TIP]
> Olay kaynağınız bir IoT hub'ıysa, Zaman Serisi kimliğiniz büyük olasılıkla ***iothub-connection-device-id***olacaktır.

İzlenen en iyi uygulamalar şunlardır:

* Birçok farklı değere (örneğin, yüzlerce veya binlerce) sahip bir bölüm anahtarı seçin. Çoğu durumda, bu JSON cihazınızın kimliği, sensör kimliği veya etiket kimliği olabilir.
* Zaman Serisi [Kimliği, Zaman Serisi Modelinizin](./time-series-insights-update-tsm.md)yaprak düğümü düzeyinde benzersiz olmalıdır.
* Time Series ID'nin özellik adı dizesi için karakter sınırı 128'dir. Zaman Serisi Kimliği'nin özellik değeri için karakter sınırı 1.024'dür.
* Zaman Serisi Kimliği için benzersiz bir özellik değeri eksikse, null değer olarak kabul edilir ve benzersizlik kısıtlamasının aynı kuralını izler.
* Ayrıca, Zaman Serisi Kimliğiniz olarak en fazla *üç* önemli özellik seçebilirsiniz. Bunların birleşimi Zaman Serisi Kimliğini temsil eden bileşik bir anahtar olacaktır.  
  > [!NOTE]
  > Üç temel özelliğiniz dizeleri olmalıdır.
  > Aynı anda bir özellik yerine bu bileşik anahtara karşı sorgulamanız gerekir.

## <a name="select-more-than-one-key-property"></a>Birden fazla anahtar özelliği seçin

Aşağıdaki senaryolarda, Zaman Serisi Kimliğiniz olarak birden fazla anahtar özelliği nseçimi açıklayınız.  

### <a name="example-1-time-series-id-with-a-unique-key"></a>Örnek 1: Benzersiz bir anahtara sahip Zaman Serisi Kimliği

* Eski mal filolarınız var. Her birinin benzersiz bir anahtarı vardır.
* Bir filo benzersiz özellik **deviceId**tarafından tanımlanır. Başka bir filo için, benzersiz özellik **objectId**olduğunu. İki filo da diğer filonun eşsiz özelliğini içermez. Bu örnekte, benzersiz anahtarlar olarak **deviceId** ve **objectId**olmak üzere iki anahtar seçersiniz.
* Null değerlerini kabul ediyoruz ve bir mülkün olay yükündeki varlığının olmaması null bir değer olarak sayılır. Bu, her olay kaynağındaki verilerin benzersiz bir Zaman Serisi kimliğine sahip olduğu iki olay kaynağına veri gönderme işlemlerini işlemenin de uygun yoludur.

### <a name="example-2-time-series-id-with-a-composite-key"></a>Örnek 2: Bileşik anahtarlı Zaman Serisi Kimliği

* Aynı varlık filosu içinde benzersiz olması için birden çok özelliğin olmasını gerektirirsiniz. 
* Akıllı binalar üreticisisiniz ve her odaya sensör yerleştirin. Her odada, genellikle **sensorId**için aynı değerlere sahip. Örnekler **sensor1**, **sensor2**, ve **sensor3**.
* Binanız mülkiyet **flrRm**siteler arasında örtüşen zemin ve oda numaraları vardır. Bu sayıların **1a,** **2b**ve **3a**gibi değerleri var.
* **Redmond,** **Barselona**ve **Tokyo**gibi değerleri içeren bir özellik, **yer,** var. Benzersizlik oluşturmak için, aşağıdaki üç özelliği Time Series ID tuşlarınız olarak belirlersiniz: **sensorId**, **flrRm**, ve **konum.**

Örnek ham olay:

```JSON
{
  "sensorId": "sensor1",
  "flrRm": "1a",
  "location": "Redmond",
  "temperature": 78
}
```

Azure portalında bileşik anahtarı aşağıdaki gibi girebilirsiniz: 

```JSON
[{"name":"sensorId","type":"String"},{"name":"flrRm","type":"String"},{"name":"location","type":"string"}]
```

## <a name="next-steps"></a>Sonraki adımlar

* [Veri modelleme](./time-series-insights-update-tsm.md)hakkında daha fazla bilgi edinin.

* Azure [Zaman Serisi Öngörüleri Önizleme ortamınızı](./time-series-insights-update-plan.md)planlayın.