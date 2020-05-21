---
title: "C 'de REST çağrısıyla model al #"
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/18/2020
ms.author: diberry
ms.openlocfilehash: 19f72dbb62fc2084bf0c9609fb3782e083c911af
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83655517"
---
## <a name="prerequisites"></a>Ön koşullar

* Azure Language Understanding-yazma kaynağı 32 karakter anahtarı ve yazma uç noktası URL 'SI. [Azure Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) veya [Azure CLI](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli)ile oluşturun.
* GitHub deposundan [pizza](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/luis/apps/pizza-with-machine-learned-entity.json) uygulamasını içeri aktarın `Azure-Samples/cognitive-services-sample-data-files` .
* Alınan pizza uygulaması için LUSıS uygulama KIMLIĞI. Uygulama kimliği, uygulama panosunda gösterilir.
* Konuşmaları alan uygulamanın içindeki sürüm kimliği.
* [Node.js](https://nodejs.org/) programlama dili
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="create-the-nodejs-project"></a>Node. js projesini oluşturma

1. Node. js projenizi (gibi) tutmak için yeni bir klasör oluşturun `node-model-with-rest` .

1. Yeni bir komut Istemi açın, oluşturduğunuz klasöre gidin ve aşağıdaki komutu yürütün:

    ```console
    npm init
    ```

    Her sorulduğunda varsayılan ayarları kabul etmek için ENTER tuşuna basın.

1. Aşağıdaki komutu girerek istek Promise modülünü yükler:

    ```console
    npm install --save request-promise
    ```

## <a name="change-model-programmatically"></a>Modeli program aracılığıyla değiştirme

1. `model.js` adlı yeni bir dosya oluşturun. Aşağıdaki kodu ekleyin:

    ```javascript
    var request = require('request-promise');

    //////////
    // Values to modify.

    // YOUR-APP-ID: The App ID GUID found on the www.luis.ai Application Settings page.
    const LUIS_appId = "YOUR-APP-ID";

    // YOUR-AUTHORING-KEY: Your LUIS authoring key, 32 character value.
    const LUIS_authoringKey = "YOUR-AUTHORING-KEY";

    // YOUR-AUTHORING-ENDPOINT: Replace this with your authoring key endpoint.
    // For example, "https://your-resource-name.api.cognitive.microsoft.com/"
    const LUIS_endpoint = "YOUR-AUTHORING-ENDPOINT";

    // NOTE: Replace this your version number. The Pizza app uses a version number of "0.1".
    const LUIS_versionId = "0.1";
    //////////

    const addUtterancesURI = `${LUIS_endpoint}luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/examples`;
    const addTrainURI = `${LUIS_endpoint}luis/authoring/v3.0-preview/apps/${LUIS_appId}/versions/${LUIS_versionId}/train`;

    const utterances = [
        {
            'text': 'order a pizza',
            'intentName': 'ModifyOrder',
            'entityLabels': [
                {
                    'entityName': 'Order',
                    'startCharIndex': 6,
                    'endCharIndex': 12
                }
            ]
        },
        {
            'text': 'order a large pepperoni pizza',
            'intentName': 'ModifyOrder',
            'entityLabels': [
                {
                    'entityName': 'Order',
                    'startCharIndex': 6,
                    'endCharIndex': 28
                },
                {
                    'entityName': 'FullPizzaWithModifiers',
                    'startCharIndex': 6,
                    'endCharIndex': 28
                },
                {
                    'entityName': 'PizzaType',
                    'startCharIndex': 14,
                    'endCharIndex': 28
                },
                {
                    'entityName': 'Size',
                    'startCharIndex': 8,
                    'endCharIndex': 12
                }
            ]
        },
        {
            'text': 'I want two large pepperoni pizzas on thin crust',
            'intentName': 'ModifyOrder',
            'entityLabels': [
                {
                    'entityName': 'Order',
                    'startCharIndex': 7,
                    'endCharIndex': 46
                },
                {
                    'entityName': 'FullPizzaWithModifiers',
                    'startCharIndex': 7,
                    'endCharIndex': 46
                },
                {
                    'entityName': 'PizzaType',
                    'startCharIndex': 17,
                    'endCharIndex': 32
                },
                {
                    'entityName': 'Size',
                    'startCharIndex': 11,
                    'endCharIndex': 15
                },
                {
                    'entityName': 'Quantity',
                    'startCharIndex': 7,
                    'endCharIndex': 9
                },
                {
                    'entityName': 'Crust',
                    'startCharIndex': 37,
                    'endCharIndex': 46
                }
            ]
        }
    ];

    // Main function.
    const main = async() =>{

        await addUtterances(utterances);
        await train("POST");
        await train("GET");

    }

    // Adds the utterances to the model.
    const addUtterances = async (utterances) => {

        const options = {
            uri: addUtterancesURI,
            method: 'POST',
            headers: {
                'Ocp-Apim-Subscription-Key': LUIS_authoringKey
            },
            json: true,
            body: utterances
        };

        const response = await request(options)
        console.log("addUtterance:\n" + JSON.stringify(response, null, 2));
    }

    // With verb === "POST", sends a training request.
    // With verb === "GET", obtains the training status.
    const train = async (verb) => {

        const options = {
            uri: addTrainURI,
            method: verb,
            headers: {
                'Ocp-Apim-Subscription-Key': LUIS_authoringKey
            },
            json: true,
            body: null // The body can be empty for a training request
        };

        const response = await request(options)
        console.log("train " + verb + ":\n" + JSON.stringify(response, null, 2));
    }

    // MAIN
    main().then(() => console.log("done")).catch((err)=> console.log(err));
    ```

1. İle başlayan değerleri `YOUR-` kendi değerlerinizle değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-APP-ID`| LUSıS uygulama KIMLIĞINIZ. |
    |`YOUR-AUTHORING-KEY`|32 karakter yazma anahtarınız.|
    |`YOUR-AUTHORING-ENDPOINT`| Yazma URL 'niz uç noktasıdır. Örneğin, `https://replace-with-your-resource-name.api.cognitive.microsoft.com/`. Kaynağı oluşturduğunuzda kaynak adınızı ayarlarsınız.|

    Atanan anahtarlar ve kaynaklar, **Azure kaynakları** sayfasındaki Yönet bölümündeki Luo portalında görünür. Uygulama KIMLIĞI, **uygulama ayarları** sayfasında aynı Yönet bölümünde bulunur.

1. Komut isteminde, projeyi çalıştırmak için aşağıdaki komutu girin:

    ```console
    node model.js
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç ile işiniz bittiğinde, proje klasörünü dosya sisteminden silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir uygulama için en iyi uygulamalar](../luis-concept-best-practices.md)