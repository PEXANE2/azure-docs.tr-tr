---
title: "Hızlı başlangıç: Python için SDK 'Yı kullanarak videoları arama-Bing Video Arama"
titleSuffix: Azure Cognitive Services
description: Python için Bing Video Arama SDK kullanarak video arama istekleri göndermek için bu hızlı başlangıcı kullanın
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: quickstart
ms.date: 12/09/2019
ms.author: aahi
ms.openlocfilehash: c1afea4e6cacc1f6e9ac84ca20e638836540396e
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75448400"
---
# <a name="quickstart-perform-a-video-search-with-the-bing-video-search-sdk-for-python"></a>Hızlı başlangıç: Python için Bing Video Arama SDK ile video araması gerçekleştirme

Python için Bing Video Arama SDK ile haberleri aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Video Arama birçok programlama dili ile uyumlu bir REST API sahip olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu, [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/video_search_samples.py) 'da ek ek açıklamalar ve özelliklerle bulunabilir.

[!INCLUDE [cognitive-services-bing-video-search-signup-requirements](../../../../includes/cognitive-services-bing-video-search-signup-requirements.md)]

## <a name="prerequisites"></a>Ön koşullar

- [Python](https://www.python.org/) 2. x veya 3. x
- Python için Bing Video Arama SDK 'Sı

Python [sanal ortamı](https://docs.python.org/3/tutorial/venv.html)kullanmanız önerilir. [Venv modülü](https://pypi.python.org/pypi/virtualenv)ile sanal bir ortam yükleyebilir ve başlatabilirsiniz. Python 2,7 için virtualenv 'yi şu ile birlikte:

```console
python -m venv mytestenv
```

Bing Video Arama SDK 'sını şu şekilde yüklemelisiniz:

```console
cd mytestenv
python -m pip install azure-cognitiveservices-search-videosearch
```

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarma deyimlerini ekleyin. 

    ```python
    from azure.cognitiveservices.search.videosearch import VideoSearchAPI
    from azure.cognitiveservices.search.videosearch.models import VideoPricing, VideoLength, VideoResolution, VideoInsightModule
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Abonelik anahtarınız için bir değişken oluşturun. 

    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    ```

## <a name="create-the-search-client"></a>Arama istemcisini oluşturma

`CognitiveServicesCredentials` nesnesinin bir örneğini oluşturun ve istemcinin bir örneğini başlatın:

```python
client = VideoSearchAPI(CognitiveServicesCredentials(subscription_key))
```

## <a name="send-a-search-request-and-get-a-response"></a>Arama isteği gönderme ve yanıt edinme

1. Bing Video Arama API'si bir istek göndermek ve bir yanıt almak için arama Sorgunuzla birlikte `client.videos.search()` kullanın.

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
> [Tek sayfalı Web uygulaması oluşturma](../tutorial-bing-video-search-single-page-app.md)

## <a name="see-also"></a>Ayrıca bkz. 

- [Bing Video Arama API'si nedir?](../overview.md)
- [Bilişsel Hizmetler .NET SDK örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
