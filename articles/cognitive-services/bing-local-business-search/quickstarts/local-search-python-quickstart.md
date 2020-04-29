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
ms.date: 11/29/2019
ms.author: aahi
ms.openlocfilehash: c7e7ef7f052fccfea18b246f41109d5fa7528b4b
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75379752"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Hızlı başlangıç: Python 'da Bing yerel Iş Arama API 'sine bir sorgu gönderme

Azure bilişsel hizmeti olan Bing yerel Iş Arama API 'sine istek göndermeye başlamak için bu hızlı başlangıcı kullanın. Bu basit uygulama Python 'da yazıldığı sırada, API, HTTP istekleri yapma ve JSON 'u ayrıştırma yeteneğine sahip olan herhangi bir programlama diliyle uyumlu olan bir yeniden takip eden Web hizmetidir.

Bu örnek uygulama, arama sorgusu `hotel in Bellevue`için API 'den gelen yerel yanıt verilerini alır.

## <a name="prerequisites"></a>Ön koşullar

* [Python](https://www.python.org/) 2. x veya 3. x
 
Bing API 'lerle bir bilişsel [Hizmetler API 'si hesabınızın](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olması gerekir. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz deneme tarafından sunulan erişim anahtarını kullanın.  Ayrıca bkz. bilişsel [Hizmetler fiyatlandırması-BING arama API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="run-the-complete-application"></a>Uygulamanın tamamını Çalıştır

Aşağıdaki kod yerelleştirilmiş sonuçları alır. Aşağıdaki adımları izler:
1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Sorgu parametresini belirtin. 
3. İsteği oluşturan ve Ocp-Apim-Subscription-Key üst bilgisini ekleyen Search işlevini tanımlama.
4. Ocp-Apim-Subscription-Key üst bilgisini ayarlama. 
5. Bağlantıyı yapın ve isteği gönderin.
6. JSON sonuçlarını yazdırma.

Bu tanıtımda kullanılan kodun tamamı aşağıda verilmiştir:

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
- [Yerel Iş arama düğümü hızlı başlangıç](local-search-node-quickstart.md)
