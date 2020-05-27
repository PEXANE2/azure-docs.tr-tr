---
title: Hızlı başlangıç-Python 'da API 'ye sorgu gönderme-Bing yerel Iş arama
titleSuffix: Azure Cognitive Services
description: Python 'da Bing yerel Iş Arama API 'sini kullanmaya başlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 05/12/2020
ms.author: aahi
ms.openlocfilehash: 3a90d5455c0664ceabf80647fc94a37ad0c716b5
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873021"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Hızlı başlangıç: Python 'da Bing yerel Iş Arama API 'sine bir sorgu gönderme

Azure bilişsel hizmeti olan Bing yerel Iş Arama API 'sine istek gönderme hakkında bilgi edinmek için bu hızlı başlangıcı kullanın. Bu basit uygulama Python 'da yazılmış olsa da, API, HTTP istekleri yapma ve JSON 'u ayrıştırma yeteneğine sahip olan herhangi bir programlama diliyle uyumlu olan bir yeniden takip eden Web hizmetidir.

Bu örnek uygulama, bir arama sorgusu için API 'den gelen yerel yanıt verilerini alır.

## <a name="prerequisites"></a>Ön koşullar

* [Python](https://www.python.org/) 2. x veya 3. x.
* Bing Arama API'leri olan bilişsel [Hizmetler API 'si hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) . Bu hızlı başlangıçta [ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) yeterlidir. Ücretsiz deneme sürümünüzü etkinleştirdiğinizde sunulan API anahtarını kaydedin. Daha fazla bilgi için bkz. bilişsel [Hizmetler fiyatlandırması-BING arama API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-complete-application"></a>Uygulamanın tamamını Çalıştır

Aşağıdaki örnekte, aşağıdaki adımlarda uygulanan yerelleştirilmiş sonuçlar alınır:
1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Sorgu parametresini belirtin. 
3. İsteği oluşturan ve üstbilgiyi ekleyen arama işlevini tanımlayın `Ocp-Apim-Subscription-Key` .
4. Üstbilgiyi ayarlayın `Ocp-Apim-Subscription-Key` . 
5. Bağlantıyı yapın ve isteği gönderin.
6. JSON sonuçlarını yazdırma.

Bu demo için kodun tamamı aşağıdaki gibidir:

```python
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'api.cognitive.microsoft.com'
path = '/bing/v7.0/localbusinesses/search'

query = 'restaurant in Bellevue'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

```

## <a name="next-steps"></a>Sonraki adımlar
- [Yerel Iş arama Java hızlı başlangıç](local-search-java-quickstart.md)
- [Yerel Iş arama C# hızlı başlangıç](local-quickstart.md)
- [Yerel Iş arama Node. js hızlı başlangıç](local-search-node-quickstart.md)
