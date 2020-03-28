---
title: "C'deki REST araması ile model alın #"
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/14/2020
ms.author: diberry
ms.openlocfilehash: 4d8da7d2bc51c4fc4ebc8d71f230f24f20b3aa24
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77368427"
---
## <a name="prerequisites"></a>Ön koşullar

* Azure Dil Anlama - Kaynak 32 karakter anahtarı yazma ve bitiş noktası URL yazma. [Azure portalı](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) veya [Azure CLI](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli)ile oluşturun.
* [TravelAgent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) uygulamasını bilişsel-hizmet dili-anlayan GitHub deposundan aktarın.
* İçeri aktarılan TravelAgent uygulamasının LUIS uygulama kimliği. Uygulama kimliği, uygulama panosunda gösterilir.
* Konuşmaları alan uygulamanın içindeki sürüm kimliği. Varsayılan kimlik: "0.1".
* [Python 3.6](https://www.python.org/downloads/) veya üzeri.
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Örnek konuşmalar JSON dosyası

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="change-model-programmatically"></a>Modeli programlı olarak değiştirme

1. `model.py` adlı yeni bir dosya oluşturun. Aşağıdaki kodu ekleyin:

    [!code-python[Add example utterances to Language Understanding in python](~/samples-luis/documentation-samples/quickstarts/change-model/python/3.x/add-utterances-3-6.py)]

1. Kendi değerlerinizle `YOUR-` başlayan değerleri değiştirin.

    |Bilgi|Amaç|
    |--|--|
    |`YOUR-KEY`|32 karakter yazarlık anahtarın.|
    |`YOUR-ENDPOINT`| Yazma URL bitiş noktanız. Örneğin, `replace-with-your-resource-name.api.cognitive.microsoft.com`. Kaynağı oluşturduğunuzda kaynak adınızı ayarlarsınız.|
    |`YOUR-APP-ID`| LUIS uygulama kimliğiniz. |

    Atanan anahtarlar ve kaynaklar, **Azure kaynakları** sayfasındaki Yönet bölümündeki LUIS portalında görülebilir. Uygulama kimliği, **Uygulama Ayarları** sayfasında aynı Yönet bölümünde mevcuttur.

1. Dosyayı oluşturduğunuz yerdeki komut istemiyle, dosyayı çalıştırmak için aşağıdaki komutu girin:

    ```console
    python model.py
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlatmayı bitirdiğinizde, dosyayı dosya sisteminden silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir uygulama için en iyi uygulamalar](../luis-concept-best-practices.md)
