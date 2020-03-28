---
title: Quickstart - Python'da API'ye sorgu gönderme - Bing Yerel İş Arama
titleSuffix: Azure Cognitive Services
description: Python'da Bing Yerel İş Arama API'sini kullanmaya başlamak için bu hızlı başlatmayı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: quickstart
ms.date: 11/29/2019
ms.author: aahi
ms.openlocfilehash: c7e7ef7f052fccfea18b246f41109d5fa7528b4b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75379752"
---
# <a name="quickstart-send-a-query-to-the-bing-local-business-search-api-in-python"></a>Hızlı başlatma: Python'daki Bing Yerel İş Arama API'sine sorgu gönderme

Azure Bilişsel Hizmeti olan Bing Yerel İş Arama API'sine istek göndermeye başlamak için bu hızlı başlangıcı kullanın. Bu basit uygulama Python'da yazılmış olsa da, API, HTTP isteklerini yapma ve JSON'u ayrıştırma yeteneğine sahip herhangi bir programlama diliyle uyumlu bir RESTful Web hizmetidir.

Bu örnek uygulama, arama sorgusu `hotel in Bellevue`için API'den yerel yanıt verilerini alır.

## <a name="prerequisites"></a>Ön koşullar

* [Piton](https://www.python.org/) 2.x veya 3.x
 
Bing API'leri ile [Bilişsel Hizmetler API hesabınız](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) olmalıdır. [Ücretsiz deneme](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) bu hızlı başlangıç için yeterlidir. Ücretsiz deneme sürümü tarafından sağlanan erişim anahtarını kullanın.  Ayrıca bakınız [Bilişsel Hizmetler Fiyatlandırma - Bing Arama API.](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/)

## <a name="run-the-complete-application"></a>Uygulamanın tamamını çalıştırın

Aşağıdaki kod yerelleştirilmiş sonuçlar alır. Aşağıdaki adımları izler:
1. Ana bilgisayar ve yol ile uç noktayı belirtmek için değişkenleri bildirme.
2. Sorgu parametresini belirtin. 
3. İsteği oluşturan ve Ocp-Apim-Subscription-Key üst bilgisini ekleyen Search işlevini tanımlama.
4. Ocp-Apim-Subscription-Key üst bilgisini ayarlama. 
5. Bağlantıyı kurun ve isteği gönderin.
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
- [Yerel İş Arama Java Quickstart](local-search-java-quickstart.md)
- [Yerel İş Arama C# Quickstart](local-quickstart.md)
- [Yerel İş Arama Düğümü Quickstart](local-search-node-quickstart.md)
