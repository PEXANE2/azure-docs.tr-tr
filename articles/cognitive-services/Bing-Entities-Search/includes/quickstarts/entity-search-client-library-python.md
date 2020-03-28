---
title: Bing Entity Search Python istemci kitaplığı hızlı başlat
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/06/2020
ms.author: aahi
ms.openlocfilehash: 0c0a124773eab8166806312dc47ded24e1cd841f
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "79136763"
---
Python için Bing Entity Search istemci kitaplığı olan varlıkları aramaya başlamak için bu hızlı başlatı kullanın. Bing Entity Search çoğu programlama diliyle uyumlu bir REST API'sine sahip olsa da, istemci kitaplığı hizmeti uygulamalarınız için tümleştirmenin kolay bir yolunu sağlar. Bu örnek için kaynak kodu [GitHub](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/search/entity_search_samples.py)bulunabilir.

## <a name="prerequisites"></a>Ön koşullar

* Python [2.x veya 3.x](https://www.python.org/)

* [Python için Bing Varlık Arama SDK](https://pypi.org/project/azure-cognitiveservices-search-entitysearch/)

Python sanal ortamı kullanmanız önerilir. Venv modülü ile sanal ortamı yükleyebilir ve açabilirsiniz. Virtualenv ile yükleyebilirsiniz:

```Console
python -m venv mytestenv
```

Bing Entity Search istemci kitaplığını aşağıdakilerle yükleyin:

```Console
cd mytestenv
python -m pip install azure-cognitiveservices-search-entitysearch
```

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](~/includes/cognitive-services-bing-entity-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Uygulamayı oluşturma ve başlatma

1. Favori IDE veya düzenleyicinizde yeni bir Python dosyası oluşturun ve aşağıdaki alma deyimlerini ekleyin. 

    ```python
    from azure.cognitiveservices.search.entitysearch import EntitySearchClient
    from azure.cognitiveservices.search.entitysearch.models import Place, ErrorResponseException
    from msrest.authentication import CognitiveServicesCredentials
    ```

2. Abonelik anahtarınız ve bitiş noktanız için bir değişken oluşturun. Anahtarınızla yeni `CognitiveServicesCredentials` bir nesne oluşturarak istemciyi anında anons edin.
    
    ```python
    subscription_key = "YOUR-SUBSCRIPTION-KEY"
    endpoint = "YOUR-ENDPOINT"
    client = EntitySearchclient(endpoint=endpoint, credentials=CognitiveServicesCredentials(subscription_key))
    ```

## <a name="send-a-search-request-and-receive-a-response"></a>Arama isteği gönderme ve yanıt alma

1. Bing Entity Search'e ve `client.entities.search()` arama sorgusuna bir arama isteği gönderin. 
    
    ```python
    entity_data = client.entities.search(query="Gibralter")
    ```

2. Varlıklar döndürüldüyse, `entity_data.entities.value` listeye dönüştürün ve ilk sonucu yazdırın.
    ```python
    if entity_data.entities.value:
    
        main_entities = [entity for entity in entity_data.entities.value
                         if entity.entity_presentation_info.entity_scenario == "DominantEntity"]
    
        if main_entities:
            print(main_entities[0].description)
    ```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tek sayfalı web uygulaması oluşturma](../../tutorial-bing-entities-search-single-page-app.md)

* [Bing Varlık Arama API'si nedir?](../../overview.md )