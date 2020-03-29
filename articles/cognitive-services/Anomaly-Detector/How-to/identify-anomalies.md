---
title: Zaman serisi verilerinizde Anomali Dedektörü API'si nasıl kullanılır?
titleSuffix: Azure Cognitive Services
description: Verilerinizdeki anormallikleri toplu olarak veya veri akışında nasıl algılayıcı olarak algılayıcı olarak öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: aahi
ms.openlocfilehash: ca93de71f64efaf21c78b37b9c9aee193d13b28d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71840216"
---
# <a name="how-to-use-the-anomaly-detector-api-on-your-time-series-data"></a>Nasıl kullanılır: Zaman serisi verilerinizde Anomali Dedektörü API'sını kullanın  

[Anomali Dedektörü API](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect) anomali algılama iki yöntem sağlar. Anormallikleri times serisiniz boyunca bir toplu iş olarak algılayabilir veya verileriniz en son veri noktasının anomali durumunu algılayarak oluşturulur. Algılama modeli, her veri noktasının beklenen değeri ve üst ve alt anomali algılama sınırları ile birlikte anomali sonuçlarını döndürür. bu değerleri, normal değerlerin ve verilerdeki anormalliklerin aralığını görselleştirmek için kullanabilirsiniz.

## <a name="anomaly-detection-modes"></a>Anomali algılama modları 

Anomali Dedektörü API algılama modları sağlar: toplu ve akış.

> [!NOTE]
> Aşağıdaki istek URL'leri aboneliğiniz için uygun bitiş noktasıyla birleştirilmelidir. Örneğin, `https://<your-custom-subdomain>.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/entire/detect`


### <a name="batch-detection"></a>Toplu işlem algılama

Belirli bir zaman aralığındaki veri noktaları nın bir bölümündeki anormallikleri algılamak için, zaman serisi verilerinizle aşağıdaki istek URI'yi kullanın: 

`/timeseries/entire/detect`. 

Zaman serisi verilerinizi aynı anda göndererek, API tüm seriyi kullanarak bir model oluşturur ve her veri noktasını onunla analiz edecektir.  

### <a name="streaming-detection"></a>Akış algılama

Veri akışındaki anormallikleri sürekli olarak algılamak için, en son veri noktanızla aşağıdaki istek URI'yi kullanın: 

`/timeseries/last/detect'`. 

Oluşturduğunuz yeni veri noktaları göndererek verilerinizi gerçek zamanlı olarak izleyebilirsiniz. Gönderdiğiniz veri noktalarıyla bir model oluşturulur ve API zaman serisindeki en son noktanın bir anormallik olup olmadığını belirler.

## <a name="adjusting-lower-and-upper-anomaly-detection-boundaries"></a>Alt ve üst anomali algılama sınırlarının ayarlanması

Varsayılan olarak, anomali tespiti için üst ve `expectedValue`alt `upperMargin`sınırlar `lowerMargin`, , ve . Farklı sınırlar gerektiriyorsa, bir `marginScale` `upperMargin` veya . `lowerMargin` Sınırlar aşağıdaki gibi hesaplanır:

|Sınır  |Hesaplama  |
|---------|---------|
|`upperBoundary` | `expectedValue + (100 - marginScale) * upperMargin`        |
|`lowerBoundary` | `expectedValue - (100 - marginScale) * lowerMargin`        |

Aşağıdaki örnekler, farklı hassasiyetlerde bir Anomaly Detector API sonucu nu göstermektedir.

### <a name="example-with-sensitivity-at-99"></a>99 hassasiyetli örnek

![Varsayılan Duyarlılık](../media/sensitivity_99.png)

### <a name="example-with-sensitivity-at-95"></a>95 hassasiyetli örnek

![99 Hassasiyet](../media/sensitivity_95.png)

### <a name="example-with-sensitivity-at-85"></a>85 hassasiyetli örnek

![85 Hassasiyet](../media/sensitivity_85.png)

## <a name="next-steps"></a>Sonraki Adımlar

* [Anomali Algılayıcısı API'si nedir?](../overview.md)
* [Hızlı başlangıç: Anomali Dedektörü REST API'yi kullanarak zaman serisi verilerinizdeki anormallikleri tespit edin](../quickstarts/detect-data-anomalies-csharp.md)
