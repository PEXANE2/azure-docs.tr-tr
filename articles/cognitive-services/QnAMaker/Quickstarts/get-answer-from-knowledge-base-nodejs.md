---
title: "Hızlı Başlangıç: Bilgi Bankası 'ndan yanıt alın-REST, Node. js-Soru-Cevap Oluşturma"
titleSuffix: Azure Cognitive Services
description: Bu Node. js REST tabanlı hızlı başlangıç, Bilgi Bankası 'ndan programlama yoluyla yanıt alma konusunda size kılavuzluk eder.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: e80f852a48d7da9480e6c0eddc675e6bcf524eb7
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562966"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-nodejs"></a>Node. js ile Bilgi Bankası 'ndan bir soruya yanıt alın

Bu hızlı başlangıçta, yayımlanan Soru-Cevap Oluşturma bilgi bankasından programlı bir yanıt alma işlemi adım adım gösterilmektedir. Bilgi Bankası, SSS gibi [veri kaynaklarından](../Concepts/data-sources-supported.md) gelen sorular ve yanıtlar içerir. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) soru-cevap oluşturma hizmetine gönderilir. [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) , en fazla tahmini yanıtı içerir. 

## <a name="prerequisites"></a>Önkoşullar

* [Node.js](https://nodejs.org/en/download/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için seçin **anahtarları** altında **kaynak yönetimi** Azure Panonuzda soru-cevap Oluşturucu kaynağınızın. 
* **Yayımlama** sayfası ayarları. Yayımlanmış bir bilgi tabanınız yoksa, boş bir Bilgi Bankası oluşturun ve ardından **Ayarlar** sayfasına bir Bilgi Bankası alın ve ardından yayımlayın. [Bu temel Bilgi Bankası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv)'nı indirip kullanabilirsiniz. 

    Yayımlama sayfası ayarları, POST yol değerini, ana bilgisayar değerini ve EndpointKey değerini içerir. 

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Bu hızlı başlangıç [https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs](https://github.com/Azure-Samples/cognitive-services-qnamaker-nodejs/tree/master/documentation-samples/quickstarts/get-answer) kodu depoda. 

## <a name="create-a-nodejs-file"></a>Node. js dosyası oluşturma

Adlı yeni bir dosya oluşturun ve açın VSCode `get-answer.js`. 

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

`get-answer.js` Dosyanın en üstünde, gerekli bağımlılıkları projeye ekleyin:

[!code-nodejs[Add the required dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=1-4 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Sonra, Soru-Cevap Oluşturma erişmek için gerekli sabitleri ekleyin. Bu değerler, Bilgi Bankası 'nı yayımladıktan sonra **Yayımla** sayfasında bulunur. 

[!code-nodejs[Add the required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=6-22 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-an-answer"></a>Soru göndermek ve yanıt almak için bir POST isteği ekleyin

Aşağıdaki kod, soruyu bilgi tabanına göndermek için Soru-Cevap Oluşturma API'si HTTPS isteği yapar ve yanıtı alır:

[!code-nodejs[Add a POST request to send question to knowledge base](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/get-answer/get-answer.js?range=24-49 "Add a POST request to send question to knowledge base")]

Üstbilginin değeri dizeyi `EndpointKey`içerir. `Authorization` 

## <a name="install-the-dependencies"></a>Bağımlılıkları yükler

Bağımlılıkları komut satırından yükler:

```bash
npm install request request-promise
```

## <a name="run-the-program"></a>Programı çalıştırma

Programı komut satırından çalıştırın. Otomatik olarak için soru-cevap Oluşturucu API'si isteği gönderir ve ardından konsol penceresine yazdırır.

Dosyasını çalıştırın:

```bash
node get-answer.js
```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]

[İstek](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response)hakkında daha fazla bilgi edinin.

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
