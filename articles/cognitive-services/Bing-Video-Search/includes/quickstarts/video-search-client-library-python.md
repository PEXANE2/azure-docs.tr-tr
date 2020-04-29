---
title: Bing Video Arama Python istemci kitaplığı hızlı başlangıç
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/19/2020
ms.author: aahi
ms.openlocfilehash: 7a9fab8ba8bb9d21c9284cbf14bc67226d2ef9d3
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80289745"
---
Python için Bing Video Arama istemci kitaplığıyla haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Video Arama, çoğu programlama dili ile uyumlu bir REST API sahip olsa da, istemci kitaplığı, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sağlar. Bu örneğe ilişkin kaynak kodu, [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) 'da ek ek açıklamalar ve özelliklerle bulunabilir.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](~/includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="prerequisites"></a>Ön koşullar

- [Python](https://www.python.org/) 2. x veya 3. x
- Python için Bing Video Arama istemci kitaplığı

Python [sanal ortamı](https://docs.python.org/3/tutorial/venv.html)kullanmanız önerilir. [Venv modülü](https://pypi.python.org/pypi/virtualenv)ile sanal bir ortam yükleyebilir ve başlatabilirsiniz. Python 2,7 için virtualenv 'yi şu ile birlikte:

```console
python -m venv mytestenv
```

Bing Video Arama istemci kitaplığı:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarma deyimlerini ekleyin. 

    ```python
    from azure.cognitiveservices.search.videosearch import VideoSearchClient
    from azure.cognitiveservices.search.videosearch.models import VideoPricing, VideoLength, VideoResolution, VideoInsightModule
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Abonelik anahtarınız için bir değişken oluşturun. 

    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    ```

## <a name="create-the-search-client"></a>Arama istemcisini oluşturma

`CognitiveServicesCredentials` nesnesinin bir örneğini oluşturun ve istemcinin bir örneğini başlatın:

```python
client = VideoSearchAPI(endpoint, CognitiveServicesCredentials(subscription_key))
```

## <a name="send-a-search-request-and-get-a-response"></a>Arama isteği gönderme ve yanıt edinme

1. Bing Video Arama API'si `client.videos.search()` istek göndermek ve yanıt almak için arama Sorgunuzla birlikte kullanın.

    ```python
    video_result = client.videos.search(query="SwiftKey")
    ```

2. Yanıt, arama sonuçları içeriyorsa, ilk birini alın ve KIMLIĞINI, adını ve URL 'sini yazdırın.

    ```python
    if video_result.value:
        first_video_result = video_result.value[0]
        print("Video result count: {}".format(len(video_result.value)))
        print("First video id: {}".format(first_video_result.video_id))
        print("First video name: {}".format(first_video_result.name))
        print("First video url: {}".format(first_video_result.content_url))
    else:
        print("Didn't see any video result data..")
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı Web uygulaması oluşturma](../../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

- [Bing Video Arama API'si nedir?](../../overview.md)
- [Bilişsel hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
