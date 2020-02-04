---
title: REST çağrısı olan modeli alC#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: a21834cec456ded82c15dc916cc4991a196ea22c
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76966865"
---
## <a name="prerequisites"></a>Ön koşullar

* Azure Language Understanding-yazma kaynağı 32 karakter anahtarı ve yazma uç noktası URL 'SI. [Azure Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) veya [Azure CLI](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli)ile oluşturun.
* Bilişsel hizmetler-dil düzeyi GitHub deposundan [Travelagent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) uygulamasını içeri aktarın.
* Alınan TravelAgent uygulaması için LUSıS uygulama KIMLIĞI. Uygulama kimliği, uygulama panosunda gösterilir.
* Uygulama içindeki, uttersliği alan sürüm KIMLIĞI. Varsayılan kimlik: "0.1".
* [Python 3.6](https://www.python.org/downloads/) veya üzeri.
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]


## <a name="change-model-programmatically"></a>Modeli program aracılığıyla değiştirme

Uygulamaya makine tarafından öğrenilen bir varlık [API 'si](https://aka.ms/luis-apim-v3-authoring) eklemek için git ' i kullanın.

1. `model.py` adlı yeni bir dosya oluşturun. Aşağıdaki kodu ekleyin:

    ```python
    ########### Python 3.6 #############
    import requests

    # 32 character Authoring key
    LUIS_authoringKey  = "YOUR-KEY"

    LUIS_APP_ID = "YOUR-APP-ID"

    # Authoring endpoint, example: your-resource-name.api.cognitive.microsoft.com
    LUIS_ENDPOINT = "YOUR-ENDPOINT"

    # The version number of your LUIS app
    LUIS_APP_VERSION = "0.1"

    URI_AddUtterances = f'https://{LUIS_ENDPOINT}/luis/authoring/v3.0-preview/apps/{LUIS_APP_ID}/versions/{LUIS_APP_VERSION}/examples'
    URI_Train = f'https://{LUIS_ENDPOINT}/luis/authoring/v3.0-preview/apps/{LUIS_APP_ID}/versions/{LUIS_APP_VERSION}/train'

    HEADERS = {'Ocp-Apim-Subscription-Key': LUIS_authoringKey}

    def addUtterances():
        r = requests.post(URI_AddUtterances,headers=HEADERS)
        print(r.json())

    def train():
        r = requests.post(URI_Train,headers=HEADERS)
        print(r.json())

    def trainStatus():
        r = requests.get(URI_Train,headers=HEADERS)
        print(r.json())

    addUtterances()
    train()
    trainStatus()
    ```

1. `YOUR-` ile başlayan değerleri kendi değerlerinizle değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-KEY`|32 karakter yazma anahtarınız.|
    |`YOUR-ENDPOINT`| Yazma URL 'niz uç noktasıdır. Örneğin, `replace-with-your-resource-name.api.cognitive.microsoft.com`. Kaynağı oluşturduğunuzda kaynak adınızı ayarlarsınız.|
    |`YOUR-APP-ID`| LUSıS uygulama KIMLIĞINIZ. |

    Atanan anahtarlar ve kaynaklar, **Azure kaynakları** sayfasındaki Yönet bölümündeki Luo portalında görünür. Uygulama KIMLIĞI, **uygulama ayarları** sayfasında aynı Yönet bölümünde bulunur.

1. Dosyayı oluşturduğunuz dizindeki aynı dizinde bir komut istemiyle, dosyayı çalıştırmak için aşağıdaki komutu girin:

    ```console
    python model.py
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıcı tamamladığınızda dosyayı dosya sisteminden silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir uygulama için en iyi uygulamalar](../luis-concept-best-practices.md)
