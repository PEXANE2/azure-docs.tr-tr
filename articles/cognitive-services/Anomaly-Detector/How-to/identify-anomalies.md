---
title: Zaman serisi verilerinizde anomali algılayıcı API 'sini kullanma
titleSuffix: Azure Cognitive Services
description: Verilerinize toplu olarak veya akış verilerinde nasıl bilgi alabileceğinizi öğrenin.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 10/01/2019
ms.author: mbullwin
ms.openlocfilehash: 74f891ba7f5b400b5782565e670539167f4e2464
ms.sourcegitcommit: e7152996ee917505c7aba707d214b2b520348302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2020
ms.locfileid: "97703441"
---
# <a name="how-to-use-the-anomaly-detector-api-on-your-time-series-data"></a>Nasıl yapılır: zaman serisi verilerinizde anomali algılayıcısı API 'sini kullanma  

[Anomali algılayıcı API 'si](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect) iki anomali algılama yöntemi sağlar. Anormallikleri, zaman serilerinden bir toplu iş olarak algılayabilir ya da verileriniz en son veri noktasının anomali durumunu algılayarak oluşturulur. Algılama modeli, her bir veri noktasının beklenen değeriyle birlikte anomali sonuçları ve üst ve düşük anomali algılama sınırlarını döndürür. Bu değerleri normal değerlerin aralığını görselleştirmek ve verilerdeki bozukluklar için kullanabilirsiniz.

## <a name="anomaly-detection-modes"></a>Anomali algılama modları 

Anomali algılayıcı API 'SI algılama modları sağlar: Batch ve streaming.

> [!NOTE]
> Aşağıdaki istek URL 'Leri, aboneliğiniz için uygun uç noktayla birleştirilmelidir. Örnek: `https://<your-custom-subdomain>.api.cognitive.microsoft.com/anomalydetector/v1.0/timeseries/entire/detect`


### <a name="batch-detection"></a>Toplu iş algılama

Belirli bir zaman aralığı boyunca veri noktaları için bir toplu iş boyunca anomali algılama için, zaman serisi verilerinize sahip aşağıdaki istek URI 'sini kullanın: 

`/timeseries/entire/detect`. 

Zaman serisi verilerinizi aynı anda gönderdiğinizde, API tüm seriyi kullanarak bir model oluşturur ve her bir veri noktasını onunla analiz eder.  

### <a name="streaming-detection"></a>Akış algılama

Akış verilerinde anormallikleri sürekli olarak algılamak için, en son veri noktanmla aşağıdaki istek URI 'sini kullanın: 

`/timeseries/last/detect'`. 

Yeni veri noktalarını oluştururken gönderdiğinizde verilerinizi gerçek zamanlı olarak izleyebilirsiniz. Gönderme yaptığınız veri noktalarıyla bir model oluşturulur ve API, zaman serisinde en son noktanın bir anomali olup olmadığını tespit eder.

## <a name="adjusting-lower-and-upper-anomaly-detection-boundaries"></a>Alt ve üst anomali algılama sınırlarını ayarlama

Varsayılan olarak, anomali algılama için üst ve alt sınırlar,, ve kullanılarak `expectedValue` hesaplanır `upperMargin` `lowerMargin` . Farklı sınırlara ihtiyaç duyuyorsanız, veya ' a uygulanmasını öneririz `marginScale` `upperMargin` `lowerMargin` . Sınırlar aşağıdaki şekilde hesaplanır:

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

* [Anomali Algılayıcısı API'si nedir?](../overview.md)
* [Hızlı başlangıç: anomali algılayıcısının kullanıldığı zaman serisi verilerinizde bozukluklar saptama](../quickstarts/client-libraries.md)
