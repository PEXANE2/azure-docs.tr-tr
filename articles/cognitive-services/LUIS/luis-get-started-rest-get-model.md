---
title: "Hızlı başlangıç: REST API 'Leri ile model edinme-LUıS"
titleSuffix: Azure Cognitive Services
description: Bu REST API hızlı başlangıçta, bir modeli eklemek ve bir modeli eğitmek için kıvrımlı kullanın.
services: cognitive-services
author: nitinme
manager: diberry
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 06/19/2020
ms.author: nitinme
ms.openlocfilehash: e5cf3160e6592a48e3a81480480ad8f01cc6af3a
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206264"
---
# <a name="quickstart-use-curl-and-rest-to-change-model"></a>Hızlı başlangıç: modeli değiştirmek için kıvrımlı ve REST kullanma

Bu hızlı başlangıç, bir pizza uygulamasına örnek ekleme ve uygulamayı eğitme hakkında size gösterir. Örnek konuşmalar, bir amaçla eşleşmiş kullanıcı konuşma metinleridir. Amaçlar için örnek konuşmalar sağlayarak, LUIS’e kullanıcı tarafından sağlanan hangi tür metinlerin hangi amaca ait olduğunu öğretirsiniz.

Bu hızlı başlangıcı izleyerek sırayla üç REST çağrısı gerçekleştirirsiniz.

- İlk olarak, pizza uygulama modelini eğitebilmeniz için kullanılacak bir toplu iş toplu [işi ekleyin](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c09) .
- Daha sonra, yeni [uygulama sürümü](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c45) çağrısını kullanarak pizza uygulaması için bir eğitim oturumuna başlarsınız.
- Son olarak, [diğer sürümü Al eğitim durumu](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c46) çağrısını kullanarak, pizza uygulaması için eğitim oturumunun durumunu alacaksınız.

[Başvuru belgeleri](https://westeurope.dev.cognitive.microsoft.com/docs/services/luis-programmatic-apis-v3-0-preview/operations/5890b47c39e2bb052c5b9c45)

## <a name="prerequisites"></a>Önkoşullar

* Boş bir [Luo](https://www.luis.ai) hesabı.
* [Visual Studio Code](https://code.visualstudio.com/)gibi bir metin düzenleyici.
* Komut satırı programı kıvır. Kıvrımlı program macOS, en çok Linux dağıtımları ve Windows 10 derleme 1803 ve üzeri sürümlerde zaten yüklüdür.

    Kıvrımlı yüklemeniz gerekiyorsa, kıvrımlı [indirme sayfasından](https://curl.haxx.se/download.html)kıvrımlı indirebilirsiniz.

## <a name="create-pizza-app"></a>Pizza uygulaması oluşturma

[!INCLUDE [Create pizza app](includes/get-started-get-model-create-pizza-app.md)]

## <a name="create-a-json-file-to-train-the-pizza-app"></a>Pizza uygulamasını eğitme için bir JSON dosyası oluşturma

Üç örnek elde eden bir JSON dosyası oluşturmak için aşağıdaki JSON verilerini adlı bir dosyaya kaydedin `ExampleUtterances.JSON` :

```JSON
[
  {
    "text": "order a pizza",
    "intentName": "ModifyOrder",
    "entityLabels": [
      {
        "entityName": "Order",
        "startCharIndex": 6,
        "endCharIndex": 12
      }
    ]
  },
  {
    "text": "order a large pepperoni pizza",
    "intentName": "ModifyOrder",
    "entityLabels": [
      {
        "entityName": "Order",
        "startCharIndex": 6,
        "endCharIndex": 28
      },
      {
        "entityName": "FullPizzaWithModifiers",
        "startCharIndex": 6,
        "endCharIndex": 28
      },
      {
        "entityName": "PizzaType",
        "startCharIndex": 14,
        "endCharIndex": 28
      },
      {
        "entityName": "Size",
        "startCharIndex": 8,
        "endCharIndex": 12
      }
    ]
  },
  {
    "text": "I want two large pepperoni pizzas on thin crust",
    "intentName": "ModifyOrder",
    "entityLabels": [
      {
        "entityName": "Order",
        "startCharIndex": 7,
        "endCharIndex": 46
      },
      {
        "entityName": "FullPizzaWithModifiers",
        "startCharIndex": 7,
        "endCharIndex": 46
      },
      {
        "entityName": "PizzaType",
        "startCharIndex": 17,
        "endCharIndex": 32
      },
      {
        "entityName": "Size",
        "startCharIndex": 11,
        "endCharIndex": 15
      },
      {
        "entityName": "Quantity",
        "startCharIndex": 7,
        "endCharIndex": 9
      },
      {
        "entityName": "Crust",
        "startCharIndex": 37,
        "endCharIndex": 46
      }
    ]
  }
]`
```

Örnek utterl JSON, belirli bir biçimi izler.

`text` alanı, örnek konuşmanın metnini içerir. `intentName` alanı, LUIS uygulaması içindeki mevcut bir amacın adına karşılık gelmelidir. `entityLabels` alanı gereklidir. Herhangi bir varlığı etiketlemek istemiyorsanız, boş bir dizi girin.

entityLabels dizisi boş değilse `startCharIndex` ve `endCharIndex` değerlerinin `entityName` alanında başvurulan varlığı işaretlemesi gerekir. Dizin sıfır tabanlıdır. Metinde bir boşluk olan etiketi başlatabilir veya sonlandırdıysanız, utsliği ekleme API çağrısı başarısız olur.

## <a name="add-example-utterances"></a>Örnek konuşmalar ekleme

1. Örnek toplu işi karşıya yüklemek için bu komutu metin düzenleyicinize kopyalayın:

    #### <a name="windows"></a>[Windows](#tab/windows)

    ```cURL
    curl "***YOUR-AUTHORING-ENDPOINT***/luis/authoring/v3.0-preview/apps/***YOUR-APP-ID***/versions/***YOUR-APP-VERSION***/examples?verbose=true&show-all-intents=true" ^
          --request POST ^
          --header "Content-Type:application/json" ^
          --header "Ocp-Apim-Subscription-Key: ***YOUR-AUTHORING-KEY***" ^
          --data "@ExampleUtterances.JSON"
    ```

    #### <a name="macos"></a>[macOS](#tab/unix)

    ```cURL
    curl "***YOUR-AUTHORING-ENDPOINT***/luis/authoring/v3.0-preview/apps/***YOUR-APP-ID***/versions/***YOUR-APP-VERSION***/examples?verbose=true&show-all-intents=true" \
          --request POST \
          --header "Content-Type:application/json" \
          --header "Ocp-Apim-Subscription-Key: ***YOUR-AUTHORING-KEY***" \
          --data "@ExampleUtterances.JSON"
    ```

    #### <a name="linux"></a>[Linux](#tab/linux)

    ```cURL
    curl "***YOUR-AUTHORING-ENDPOINT***/luis/authoring/v3.0-preview/apps/***YOUR-APP-ID***/versions/***YOUR-APP-VERSION***/examples?verbose=true&show-all-intents=true" \
          --request POST \
          --header "Content-Type:application/json" \
          --header "Ocp-Apim-Subscription-Key: ***YOUR-AUTHORING-KEY***" \
          --data "@ExampleUtterances.JSON"
    ```

    ***

1. İle başlayan değerleri `***YOUR-` kendi değerlerinizle değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`***YOUR-AUTHORING-ENDPOINT***`| Yazma URL 'niz uç noktasıdır. Örneğin, "https://REPLACE-WITH-YOUR-RESOURCE-NAME.api.cognitive.microsoft.com/". Kaynağı oluşturduğunuzda kaynak adınızı ayarlarsınız.|
    |`***YOUR-APP-ID***`| LUSıS uygulama KIMLIĞINIZ. |
    |`***YOUR-APP-VERSION***`| LUSıS uygulama sürümünüz. Pizza uygulaması için sürüm numarası tırnak işareti olmadan "0,1" olur.|
    |`***YOUR-AUTHORING-KEY***`|32 karakter yazma anahtarınız.|

    Atanan anahtarlar ve kaynaklar, **Azure kaynakları** sayfasındaki Yönet bölümündeki Luo portalında görünür. Uygulama KIMLIĞI, **uygulama ayarları** sayfasında aynı Yönet bölümünde bulunur.

1. Bir komut Istemi (Windows) veya Terminal (macOS ve Linux) başlatın ve dizinleri dosyayı kaydettiğiniz dizinle değiştirin `ExampleUtterances.JSON` .

1. Düzenleyiciden kıvrımlı komutunu kopyalayın ve bir komut Istemine (Windows) veya terminale (macOS ve Linux) yapıştırın. Komutu yürütmek için ENTER tuşuna basın.

    Aşağıdaki yanıtı görmeniz gerekir:

    ```json
    [{"value":{"ExampleId":1255129706,"UtteranceText":"order a pizza"},"hasError":false},{"value":{"ExampleId":1255129707,"UtteranceText":"order a large pepperoni pizza"},"hasError":false},{"value":{"ExampleId":1255129708,"UtteranceText":"i want two large pepperoni pizzas on thin crust"},"hasError":false}]
    ```

    Okunabilirlik için biçimlendirilen çıktı aşağıda verilmiştir:

    ```json
    [
      {
        "value": {
          "ExampleId": 1255129706,
          "UtteranceText": "order a pizza"
        },
        "hasError": false
      },
      {
        "value": {
          "ExampleId": 1255129707,
          "UtteranceText": "order a large pepperoni pizza"
        },
        "hasError": false
      },
      {
        "value": {
          "ExampleId": 1255129708,
          "UtteranceText": "i want two large pepperoni pizzas on thin crust"
        },
        "hasError": false
      }
    ]
    ```

## <a name="train-the-pizza-app-model"></a>Pizza uygulama modelini eğitme

1. Pizza uygulaması için bir eğitim oturumuna başlamak için, bu komutu metin düzenleyicinize kopyalayın:

    #### <a name="windows"></a>[Windows](#tab/windows)

    ```cURL
    curl "***YOUR-AUTHORING-ENDPOINT***/luis/authoring/v3.0-preview/apps/***YOUR-APP-ID***/versions/***YOUR-APP-VERSION***/train?verbose=true&show-all-intents=true" ^
          --data "" ^
          --request POST ^
          --header "Content-Type:application/json" ^
          --header "Ocp-Apim-Subscription-Key: ***YOUR-AUTHORING-KEY***"
    ```

    #### <a name="macos"></a>[macOS](#tab/unix)

    ```cURL
    curl "***YOUR-AUTHORING-ENDPOINT***/luis/authoring/v3.0-preview/apps/***YOUR-APP-ID***/versions/***YOUR-APP-VERSION***/train?verbose=true&show-all-intents=true" \
          --data "" \
          --request POST \
          --header "Content-Type:application/json" \
          --header "Ocp-Apim-Subscription-Key: ***YOUR-AUTHORING-KEY***"
    ```

    #### <a name="linux"></a>[Linux](#tab/linux)

    ```cURL
    curl "***YOUR-AUTHORING-ENDPOINT***/luis/authoring/v3.0-preview/apps/***YOUR-APP-ID***/versions/***YOUR-APP-VERSION***/train?verbose=true&show-all-intents=true" \
          --data "" \
          --request POST \
          --header "Content-Type:application/json" \
          --header "Ocp-Apim-Subscription-Key: ***YOUR-AUTHORING-KEY***"
    ```

    ***

1. Daha önce yaptığınız gibi, değerlerini `***YOUR-` kendi değerlerinizle başlayarak değiştirin.

1. Düzenleyiciden kıvrımlı komutunu kopyalayın ve bir komut Istemine (Windows) veya terminale (macOS ve Linux) yapıştırın. Komutu yürütmek için ENTER tuşuna basın.

    Aşağıdaki yanıtı görmeniz gerekir:

    ```json
    {"statusId":2,"status":"UpToDate"}
    ```

    Okunabilirlik için biçimlendirilen çıktı aşağıda verilmiştir:

    ```json
    {
      "statusId": 2,
      "status": "UpToDate"
    }
    ```

## <a name="get-the-status-of-training"></a>Eğitimin durumunu alın

1. Eğitim oturumunun eğitim durumunu almak için bu komutu metin düzenleyicinize kopyalayın:

    #### <a name="windows"></a>[Windows](#tab/windows)

    ```cURL
    curl "***YOUR-AUTHORING-ENDPOINT***/luis/authoring/v3.0-preview/apps/***YOUR-APP-ID***/versions/***YOUR-APP-VERSION***/train?verbose=true&show-all-intents=true" ^
            --request GET ^
            --header "Content-Type:application/json" ^
            --header "Ocp-Apim-Subscription-Key: ***YOUR-AUTHORING-KEY***"
    ```

    #### <a name="macos"></a>[macOS](#tab/unix)

    ```cURL
    curl "***YOUR-AUTHORING-ENDPOINT***/luis/authoring/v3.0-preview/apps/***YOUR-APP-ID***/versions/***YOUR-APP-VERSION***/train?verbose=true&show-all-intents=true" \
            --request GET \
            --header "Content-Type:application/json" \
            --header "Ocp-Apim-Subscription-Key: ***YOUR-AUTHORING-KEY***"
    ```

    #### <a name="linux"></a>[Linux](#tab/linux)

    ```cURL
    curl "***YOUR-AUTHORING-ENDPOINT***/luis/authoring/v3.0-preview/apps/***YOUR-APP-ID***/versions/***YOUR-APP-VERSION***/train?verbose=true&show-all-intents=true" \
            --request GET \
            --header "Content-Type:application/json" \
            --header "Ocp-Apim-Subscription-Key: ***YOUR-AUTHORING-KEY***"
    ```

    ***

1. Daha önce yaptığınız gibi, değerlerini `***YOUR-` kendi değerlerinizle başlayarak değiştirin.

1. Düzenleyiciden kıvrımlı komutunu kopyalayın ve bir komut Istemine (Windows) veya terminale (macOS ve Linux) yapıştırın. Komutu yürütmek için ENTER tuşuna basın.

    Aşağıdaki yanıtı görmeniz gerekir:

    ```json
    [{"modelId":"8eb7ad8f-5db5-4c28-819b-ca3905fffd80","details":{"statusId":2,"status":"UpToDate","exampleCount":171}},{"modelId":"6f53bc92-ae54-44ce-bc4e-010d1f8cfda0","details":{"statusId":2,"status":"UpToDate","exampleCount":171}},{"modelId":"6cb17888-ad6e-464c-82c0-d37fd1f2c4f8","details":{"statusId":2,"status":"UpToDate","exampleCount":171}},{"modelId":"a16fc4fd-1949-4e77-9de3-30369f16c3a5","details":{"statusId":2,"status":"UpToDate","exampleCount":171}},{"modelId":"6bacdb75-1889-4f93-8971-8c8995ff8960","details":{"statusId":2,"status":"UpToDate","exampleCount":171}},{"modelId":"be963f4c-4898-48d7-9eba-3c6af0867b9d","details":{"statusId":2,"status":"UpToDate","exampleCount":171}}]
    ```

    Okunabilirlik için biçimlendirilen çıktı aşağıda verilmiştir:

    ```json
    [
      {
        "modelId": "8eb7ad8f-5db5-4c28-819b-ca3905fffd80",
        "details": {
          "statusId": 2,
          "status": "UpToDate",
          "exampleCount": 171
        }
      },
      {
        "modelId": "6f53bc92-ae54-44ce-bc4e-010d1f8cfda0",
        "details": {
          "statusId": 2,
          "status": "UpToDate",
          "exampleCount": 171
        }
      },
      {
        "modelId": "6cb17888-ad6e-464c-82c0-d37fd1f2c4f8",
        "details": {
          "statusId": 2,
          "status": "UpToDate",
          "exampleCount": 171
        }
      },
      {
        "modelId": "a16fc4fd-1949-4e77-9de3-30369f16c3a5",
        "details": {
          "statusId": 2,
          "status": "UpToDate",
          "exampleCount": 171
        }
      },
      {
        "modelId": "6bacdb75-1889-4f93-8971-8c8995ff8960",
        "details": {
          "statusId": 2,
          "status": "UpToDate",
          "exampleCount": 171
        }
      },
      {
        "modelId": "be963f4c-4898-48d7-9eba-3c6af0867b9d",
        "details": {
          "statusId": 2,
          "status": "UpToDate",
          "exampleCount": 171
        }
      }
    ]
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı tamamladığınızda dosyayı `ExampleUtterances.JSON` dosya sisteminden silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir uygulama için en iyi uygulamalar](luis-concept-best-practices.md)
