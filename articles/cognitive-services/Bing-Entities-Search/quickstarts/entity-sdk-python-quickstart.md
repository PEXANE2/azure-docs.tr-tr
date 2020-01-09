---
title: 'Hızlı başlangıç: Python için SDK ile varlıkları arama-Bing Varlık Arama'
titleSuffix: Azure Cognitive Services
description: Python için Bing Varlık Arama SDK 'Sı olan varlıkları aramak için bu hızlı başlangıcı kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: quickstart
ms.date: 12/11/2019
ms.author: aahi
ms.openlocfilehash: 33ab516b5b501a79ba84c5aba9c3231634f3c662
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75448705"
---
# <a name="quickstart-bing-entity-search-sdk-with-python"></a>Hızlı Başlangıç: Python ile Bing Varlık Arama SDK'sı

Python için Bing Varlık Arama SDK ile varlıkları aramaya başlamak için bu hızlı başlangıcı kullanın. Bing Varlık Arama birçok programlama dili ile uyumlu bir REST API sahip olsa da SDK, hizmeti uygulamalarınızla tümleştirmenin kolay bir yolunu sunar. Bu örneğe ilişkin kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/entity_search_samples.py)' da bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* Python [2. x veya 3. x](https://www.python.org/)

* [Python için Bing varlık arama SDK 'sı](https://pypi.org/project/azure-cognitiveservices-search-entitysearch/)

Python sanal ortamı kullanmanız önerilir. Venv modülü ile sanal bir ortam yükleyebilir ve başlatabilirsiniz. Virtualenv 'yi şu ile yükleyebilirsiniz:

```Console
python -m venv mytestenv
```

Bing Varlık Arama SDK 'sını şu şekilde yüklemelisiniz:

```Console
cd mytestenv
python -m pip install azure-cognitiveservices-search-entitysearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../../includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. En sevdiğiniz IDE veya düzenleyicide yeni bir Python dosyası oluşturun ve aşağıdaki içeri aktarma deyimlerini ekleyin. 

    ```python
    from azure.cognitiveservices.search.entitysearch import EntitySearchAPI
    from azure.cognitiveservices.search.entitysearch.models import Place, ErrorResponseException
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Abonelik anahtarınız için bir değişken oluşturun ve ile yeni bir `CognitiveServicesCredentials` nesnesi oluşturarak istemciyi oluşturun.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    client = EntitySearchAPI(CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-a-search-request-and-receive-a-response"></a>Arama isteği gönderme ve yanıt alma

1. `client.entities.search()` ve bir arama sorgusuyla Bing Varlık Arama için bir arama isteği gönderin. 
    
    ```python
    entity_data = client.entities.search(query="Gibralter")
    ```

2. Varlıklar döndürülürse `entity_data.entities.value` bir listeye dönüştürün ve ilk sonucu yazdırın.
    ```python
    if entity_data.entities.value:
    
        main_entities = [entity for entity in entity_data.entities.value
                         if entity.entity_presentation_info.entity_scenario == "DominantEntity"]
    
        if main_entities:
            print(main_entities[0].description)
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../overview.md )