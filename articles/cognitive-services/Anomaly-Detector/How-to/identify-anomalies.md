---
title: Zaman serisi verilerinizde anomali algılayıcı API 'sini kullanma
titleSuffix: Azure Cognitive Services
description: Verilerinize toplu olarak veya akış verilerinde nasıl bilgi alabileceğinizi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: c7b3d9b66d74f16dc0938c888456d673b9cd4b77
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882880"
---
# <a name="how-to-use-the-anomaly-detector-api-on-your-time-series-data"></a>Nasıl yapılır: Zaman serisi verilerinizde anomali algılayıcı API 'sini kullanın  

[Anomali algılayıcı API 'si](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect) iki anomali algılama yöntemi sağlar. Anormallikleri, zaman serilerinden bir toplu iş olarak algılayabilir ya da verileriniz en son veri noktasının anomali durumunu algılayarak oluşturulur. Algılama modeli, her bir veri noktasının beklenen değeriyle birlikte anomali sonuçları ve üst ve düşük anomali algılama sınırlarını döndürür. Bu değerleri normal değerlerin aralığını görselleştirmek ve verilerdeki bozukluklar için kullanabilirsiniz.

## <a name="anomaly-detection-modes"></a>Anomali algılama modları 

Anomali algılayıcı API 'SI algılama modları sağlar: Batch ve streaming.

> [!NOTE]
> Aşağıdaki istek URL 'Leri, aboneliğiniz için uygun uç noktayla birleştirilmelidir. Örneğin, `https://westus2.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/entire/detect`


### <a name="batch-detection"></a>Toplu iş algılama

Belirli bir zaman aralığı boyunca veri noktaları için bir toplu iş boyunca anomali algılama için, zaman serisi verilerinize sahip aşağıdaki istek URI 'sini kullanın: 

`/timeseries/entire/detect`. 

Zaman serisi verilerinizi aynı anda gönderdiğinizde, API tüm seriyi kullanarak bir model oluşturur ve her bir veri noktasını onunla analiz eder.  

### <a name="streaming-detection"></a>Akış algılama

Akış verilerinde anormallikleri sürekli olarak algılamak için, en son veri noktanmla aşağıdaki istek URI 'sini kullanın: 

`/timeseries/last/detect'`. 

Yeni veri noktalarını oluştururken gönderdiğinizde verilerinizi gerçek zamanlı olarak izleyebilirsiniz. Gönderme yaptığınız veri noktalarıyla bir model oluşturulur ve API, zaman serisinde en son noktanın bir anomali olup olmadığını tespit eder.

## <a name="adjusting-lower-and-upper-anomaly-detection-boundaries"></a>Alt ve üst anomali algılama sınırlarını ayarlama

Varsayılan olarak, anomali algılama için üst ve alt sınırlar, `expectedValue` `upperMargin`, ve `lowerMargin`kullanılarak hesaplanır. Farklı sınırlara ihtiyaç duyuyorsanız, `marginScale` `upperMargin` veya `lowerMargin`' a uygulanmasını öneririz. Sınırlar aşağıdaki şekilde hesaplanır:

|Oluşturmasını  |Hesaplama  |
|---------|---------|
|`upperBoundary` | `expectedValue + (100 - marginScale) * upperMargin`        |
|`lowerBoundary` | `expectedValue - (100 - marginScale) * lowerMargin`        |

Aşağıdaki örneklerde farklı sensitivities bir anomali algılayıcı API sonucu gösterilmektedir.

### <a name="example-with-sensitivity-at-99"></a>99 ile duyarlılık içeren örnek

![Varsayılan duyarlılık](../media/sensitivity_99.png)

### <a name="example-with-sensitivity-at-95"></a>95 ile duyarlılık içeren örnek

![99 duyarlılığı](../media/sensitivity_95.png)

### <a name="example-with-sensitivity-at-85"></a>85 ile duyarlılık içeren örnek

![85 duyarlılığı](../media/sensitivity_85.png)

## <a name="next-steps"></a>Sonraki Adımlar

* [Anomali algılayıcı API nedir?](../overview.md)
* [Hızlı Başlangıç: Anomali algılayıcısı 'nı kullanarak zaman serisi verilerinizde bozukluklar olup REST API](../quickstarts/detect-data-anomalies-csharp.md)
