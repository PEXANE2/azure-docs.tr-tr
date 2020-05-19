---
title: 'Hızlı başlangıç: Python kullanarak Bing Özel Arama uç noktanızı çağırma | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Python kullanarak Bing Özel Arama örneğinizi arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.openlocfilehash: e1248a4e90653f0e862841ec6f58a12364943590
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83196579"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>Hızlı başlangıç: Python kullanarak Bing Özel Arama uç noktanızı çağırma

Bing Özel Arama örneğinden arama sonuçları isteme hakkında bilgi edinmek için bu hızlı başlangıcı kullanın. Bu uygulama Python 'da yazılsa da, Bing Özel Arama API'si çoğu programlama dili ile uyumlu olan bir yeniden yazılmış Web hizmetidir. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py)' da kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

- Bir Bing Özel Arama örneği. Daha fazla bilgi için bkz. [hızlı başlangıç: ilk Bing özel arama örneğinizi oluşturma](quick-start.md).
- [Python](https://www.python.org/) 2. x veya 3. x.

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

- En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarma deyimlerini ekleyin. Abonelik anahtarınız, özel yapılandırma KIMLIĞINIZ ve arama teriminiz için değişkenler oluşturun.

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderme ve alma 

1. Arama teriminizi `q=` sorgu parametresine ve arama örneğinizin özel yapılandırma kimliğini parametreye ekleyerek Istek URL 'sini oluşturun `customconfig=` . Parametreleri bir ve işareti () ile ayırın `&` . Aşağıdaki kodda genel uç noktasını kullanabilir veya kaynağınız için Azure portal görüntülenmiş [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) uç noktasını kullanabilirsiniz.

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. İsteği Bing Özel Arama örneğinize gönderin ve döndürülen arama sonuçlarını yazdırın.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Özel bir arama Web uygulaması oluşturma](./tutorials/custom-search-web-page.md)
