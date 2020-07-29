---
title: Java 'da REST çağrısıyla model al
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/03/2020
ms.custom: devx-track-java
ms.author: diberry
ms.openlocfilehash: a18cee5d6bfa13d377aa6900b0ea4a35c9edf4e8
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87374763"
---
[Başvuru belgeleri](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c45)  |  [Örnek](https://github.com/Azure-Samples/cognitive-services-quickstart-code/blob/master/java/LUIS/java-model-with-rest/Model.java)

## <a name="prerequisites"></a>Önkoşullar

* [JDK SE](https://aka.ms/azure-jdks)  (Java Geliştirme Seti, Standart Sürüm)
* [Visual Studio Code](https://code.visualstudio.com/) veya en sevdiğiniz IDE

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="change-model-programmatically"></a>Modeli program aracılığıyla değiştirme

1. Java projenizi tutmak için gibi yeni bir klasör oluşturun `java-model-with-rest` .

1. `lib`Aşağıdaki Java LIBS içinde adlı bir alt dizin oluşturun ve `lib` alt dizine kopyalayın:

    * [Commons-logging-1.2. jar](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/quickstarts/analyze-text/java/lib/commons-logging-1.2.jar)
    * [HttpClient-4.5.3. jar](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/quickstarts/analyze-text/java/lib/httpclient-4.5.3.jar)
    * [httpcore-4.4.6. jar](https://raw.githubusercontent.com/Azure-Samples/cognitive-services-language-understanding/master/documentation-samples/quickstarts/analyze-text/java/lib/httpcore-4.4.6.jar)

1. `Model.java` adlı yeni bir dosya oluşturun. Şu kodu ekleyin:

    [!code-java[Code snippet](~/cognitive-services-quickstart-code/java/LUIS/java-model-with-rest/Model.java)]

1. İle başlayan değerleri `YOUR-` kendi değerlerinizle değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-APP-ID`| LUSıS uygulama KIMLIĞINIZ. |
    |`YOUR-AUTHORING-KEY`|32 karakter yazma anahtarınız.|
    |`YOUR-AUTHORING-ENDPOINT`| Yazma URL 'niz uç noktasıdır. Örneğin, `https://replace-with-your-resource-name.api.cognitive.microsoft.com/`. Kaynağı oluşturduğunuzda kaynak adınızı ayarlarsınız.|

    Atanan anahtarlar ve kaynaklar, **Azure kaynakları** sayfasındaki Yönet bölümündeki Luo portalında görünür. Uygulama KIMLIĞI, **uygulama ayarları** sayfasında aynı Yönet bölümünde bulunur.

1. Dosyayı oluşturduğunuz dizindeki aynı dizinde bir komut istemiyle birlikte `Model.java` , Java dosyasını derlemek için aşağıdaki komutu girin:

    * Windows kullanıyorsanız şu komutu kullanın:`javac -cp ";lib/*" Model.java`
    * MacOS veya Linux kullanıyorsanız şu komutu kullanın:`javac -cp ":lib/*" Model.java`

1. Komut istemine aşağıdaki metni girerek Java uygulamasını komut satırından çalıştırın:

    * Windows kullanıyorsanız şu komutu kullanın:`java -cp ";lib/*" Model`
    * MacOS veya Linux kullanıyorsanız şu komutu kullanın:`java -cp ":lib/*" Model`

1. Yazma yanıtını gözden geçirin:

    ```console
    [{"value":{"ExampleId":1137150691,"UtteranceText":"order a pizza"},"hasError":false},{"value":{"ExampleId":1137150692,"UtteranceText":"order a large pepperoni pizza"},"hasError":false},{"value":{"ExampleId":1137150693,"UtteranceText":"i want two large pepperoni pizzas on thin crust"},"hasError":false}]
    {"statusId":9,"status":"Queued"}
    [{"modelId":"edb46abf-0000-41ab-beb2-a41a0fe1630f","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"a5030be2-616c-4648-bf2f-380fa9417d37","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"3f2b1f31-a3c3-4fbd-8182-e9d9dbc120b9","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"e4b6704b-1636-474c-9459-fe9ccbeba51c","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"031d3777-2a00-4a7a-9323-9a3280a30000","details":{"statusId":9,"status":"Queued","exampleCount":0}},{"modelId":"9250e7a1-06eb-4413-9432-ae132ed32583","details":{"statusId":3,"status":"InProgress","exampleCount":0,"progressSubstatus":"CollectingData"}}]
    ```

    Okunabilirlik için biçimlendirilen çıktı aşağıda verilmiştir:

    ```json
    [
      {
        "value": {
          "ExampleId": 1137150691,
          "UtteranceText": "order a pizza"
        },
        "hasError": false
      },
      {
        "value": {
          "ExampleId": 1137150692,
          "UtteranceText": "order a large pepperoni pizza"
        },
        "hasError": false
      },
      {
        "value": {
          "ExampleId": 1137150693,
          "UtteranceText": "i want two large pepperoni pizzas on thin crust"
        },
        "hasError": false
      }
    ]
    {
      "statusId": 9,
      "status": "Queued"
    }
    [
      {
        "modelId": "edb46abf-0000-41ab-beb2-a41a0fe1630f",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "a5030be2-616c-4648-bf2f-380fa9417d37",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "3f2b1f31-a3c3-4fbd-8182-e9d9dbc120b9",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "e4b6704b-1636-474c-9459-fe9ccbeba51c",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "031d3777-2a00-4a7a-9323-9a3280a30000",
        "details": {
          "statusId": 9,
          "status": "Queued",
          "exampleCount": 0
        }
      },
      {
        "modelId": "9250e7a1-06eb-4413-9432-ae132ed32583",
        "details": {
          "statusId": 3,
          "status": "InProgress",
          "exampleCount": 0,
          "progressSubstatus": "CollectingData"
        }
      }
    ]
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç ile işiniz bittiğinde, proje klasörünü dosya sisteminden silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir uygulama için en iyi uygulamalar](../luis-concept-best-practices.md)