---
title: "Hızlı Başlangıç: Bing Görsel Arama SDK'sı, Python"
titleSuffix: Azure Cognitive Services
description: Python SDK'yı kullanarak Bing Görsel Arama hizmetinden görüntü öngörüleri almaya başlamak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 12/17/2019
ms.author: aahi
ms.openlocfilehash: 2a5fb6d72ab2259b2c11d1d71e93aa635da36946
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75446560"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-sdk-for-python"></a>Quickstart: Python için Bing Görsel Arama SDK'sını kullanarak görüntü öngörüleri alın

Python SDK'yı kullanarak Bing Görsel Arama hizmetinden görüntü öngörüleri almaya başlamak için bu hızlı başlangıcı kullanın. Bing Görsel Arama çoğu programlama diliyle uyumlu bir REST API'ye sahip olsa da, SDK hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örneğin kaynak kodu [GitHub'da](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/visual_search_samples.py) bulunabilir 

## <a name="prerequisites"></a>Ön koşullar

* [Piton](https://www.python.org/) 2.x veya 3.x
* [Sanal](https://docs.python.org/3/tutorial/venv.html)ortam kullanılması önerilir. Sana ortamı [venv modülü](https://pypi.python.org/pypi/virtualenv) ile yükleyin ve başlatın.
* Python için Bing Görsel Arama SDK. Aşağıdaki komutlarla yükleyebilirsiniz:
    1. `cd mytestenv`
    2. `python -m pip install azure-cognitiveservices-search-visualsearch`

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Favori IDE veya düzenleyicinizde yeni bir Python dosyası oluşturun ve aşağıdaki alma deyimlerini ekleyin. 

    ```python
    import http.client, urllib.parse
    import json
    import os.path
    from azure.cognitiveservices.search.visualsearch import VisualSearchClient
    from azure.cognitiveservices.search.visualsearch.models import (
        VisualSearchRequest,
        CropArea,
        ImageInfo,
        Filters,
        KnowledgeRequest,
    )
    from msrest.authentication import CognitiveServicesCredentials
    ```
2. Abonelik anahtarınız, Özel Yapılandırma Kimliğiniz ve yüklemek istediğiniz resim için değişkenler oluşturun. 
    
    ```python
    subscription_key = 'YOUR-VISUAL-SEARCH-ACCESS-KEY'
    PATH = 'C:\\Users\\USER\\azure-cognitive-samples\\mytestenv\\TestImages\\'
    image_path = os.path.join(PATH, "image.jpg")
    
    ```

3. İstemciyi başlatma

    ```python
    client = VisualSearchClient(endpoint="https://api.cognitive.microsoft.com", credentials=CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-the-search-request"></a>Arama isteğini gönderme

1. Resim dosyası açıkken, `VisualSearchRequest()`serihale edin `knowledge_request` ve parametre `visual_search()`olarak geçirin.

    ```python
    with open(image_path, "rb") as image_fd:
        # You need to pass the serialized form of the model
        knowledge_request = json.dumps(VisualSearchRequest().serialize())
    
        print("\r\nSearch visual search request with binary of dog image")
        result = client.images.visual_search(image=image_fd, knowledge_request=knowledge_request)
    ```

2. Herhangi bir sonuç döndürüldüyse, bunları, etiketleri ve ilk etiketteki eylemleri yazdırın.

    ```python
    if not result:
            print("No visual search result data.")
    
            # Visual Search results
        if result.image.image_insights_token:
            print("Uploaded image insights token: {}".format(result.image.image_insights_token))
        else:
            print("Couldn't find image insights token!")
    
        # List of tags
        if result.tags:
            first_tag = result.tags[0]
            print("Visual search tag count: {}".format(len(result.tags)))
    
            # List of actions in first tag
            if first_tag.actions:
                first_tag_action = first_tag.actions[0]
                print("First tag action count: {}".format(len(first_tag.actions)))
                print("First tag action type: {}".format(first_tag_action.action_type))
            else:
                print("Couldn't find tag actions!")
        else:
            print("Couldn't find image tags!")
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalık bir web uygulaması oluşturma](tutorial-bing-visual-search-single-page-app.md)
