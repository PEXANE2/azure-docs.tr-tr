---
title: 'Quickstart: Python kullanarak Bing Özel Arama bitiş noktasını arayın | Microsoft Dokümanlar'
titleSuffix: Azure Cognitive Services
description: Python'u kullanarak Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: a601b309d18e489f6b631cb26e5f3e13ef790b42
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80238843"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>Quickstart: Python'u kullanarak Bing Özel Arama bitiş noktanızı arayın

Bing Özel Arama örneğinden arama sonuçları istemeye başlamak için bu hızlı başlangıcı kullanın. Bu uygulama Python'da yazılmış olsa da, Bing Özel Arama API'sı çoğu programlama diliyle uyumlu bir RESTful web hizmetidir. Bu örnek için kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py)kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

- Bing Özel Arama örneği. Bkz. Hızlı Başlangıç: Daha fazla bilgi için [ilk Bing Özel Arama örneğini oluşturun.](quick-start.md)
- [Piton](https://www.python.org/) 2.x veya 3.x

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Favori IDE veya düzenleyicinizde yeni bir Python dosyası oluşturun ve aşağıdaki alma deyimlerini ekleyin. Abonelik anahtarınız, Özel Yapılandırma Kimliğiniz ve arama teriminiz için değişkenler oluşturun. 

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>Arama isteği gönderme ve alma 

1. Arama teriminizi `q=` sorgu parametresine ve arama örneğinizin Özel Yapılandırma Kimliği'ne `customconfig=`ekleyerek istek URL'sini oluştur. parametreleri bir `&` karakterle ayırın. Aşağıdaki genel bitiş noktasını veya kaynağınız için Azure portalında görüntülenen [özel alt etki alanı](../../cognitive-services/cognitive-services-custom-subdomains.md) bitiş noktasını kullanabilirsiniz.

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
> [Özel Arama web uygulaması oluşturma](./tutorials/custom-search-web-page.md)
