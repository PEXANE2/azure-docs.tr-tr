---
title: 'Quickstart: Bilgi tabanından cevap alın - REST, Python - QnA Maker'
description: Bu Python REST tabanlı quickstart programlı bir bilgi tabanından bir cevap alma ile size yol.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: e290b97446d441a8218a40b528f9833d45960580
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851723"
---
# <a name="quickstart-get-answers-to-a-question-from-a-knowledge-base-with-python"></a>Hızlı başlangıç: Python ile bir bilgi tabanından bir sorunun yanıtlarını alın

Bu hızlı başlangıç, programlı bir şekilde yayınlanan bir QnA Maker bilgi tabanından yanıt almanıza yol açtır. Bilgi tabanı, SSS gibi [veri kaynaklarından](../Concepts/knowledge-base.md) gelen soruları ve yanıtları içerir. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) QnA Maker hizmetine gönderilir. [Yanıt,](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) en yüksek öngörülen yanıtı içerir.

[Referans belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime) | [Örnek](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/get-answer/get-answer-3x.py)

## <a name="prerequisites"></a>Ön koşullar

* [Python 3.6 veya üzeri](https://www.python.org/downloads/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Bir QnA Maker hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalı. Anahtarınızı almak için, QnA Maker kaynağınız için Azure panonuzda **Kaynak Yönetimi** altındaki **Anahtarlar'ı** seçin.
* Sayfa ayarlarını **yayımlayın.** Yayımlanmış bir bilgi tabanınız yoksa, boş bir bilgi tabanı oluşturun, ardından **Ayarlar** sayfasındabir bilgi tabanı nı içe aktarın ve yayımlayın. [Bu temel bilgi tabanını](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv)indirebilir ve kullanabilirsiniz.

    Yayımlama sayfası ayarları, POSTA rota değerini, Ana Bilgisayar değerini ve EndpointKey değerini içerir.

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

## <a name="create-a-python-file"></a>Python dosyası oluşturma

VSCode'u açın ve `get-answer-3x.py`adında yeni bir dosya oluşturun.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

`get-answer-3x.py` Dosyanın üst kısmında, projeye gerekli bağımlılıkları ekleyin:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=1-2 "Add the required dependencies")]

<!--TBD - reword this following paragraph -->

Ana bilgisayar ve rota, **Yayımlama** sayfasında göründüklerden farklıdır. Bunun nedeni, python kitaplığı ana bilgisayarda yönlendirmeye izin vermemektir. Ana bilgisayar parçası olarak **Yayımla** sayfasında görünen yönlendirme rotaya taşındı.

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

QnA Maker'a erişmek için gerekli sabitleri ekleyin. Bu değerler, bilgi tabanını yayımladıktan sonra **Yayımla** sayfasında yer alabilirsiniz.

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=5-25 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-an-answer"></a>Soru göndermek ve yanıt almak için POST isteği ekleme

Aşağıdaki kod, soruyu bilgi tabanına göndermek için QnA Maker API'ye https isteğinde bulunmaz ve yanıtı alır:

[!code-python[Add a POST request to send question to knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=27-48 "Add a POST request to send question to knowledge base")]

Üstbilginin `Authorization` değeri dizeiçerir. `EndpointKey`

## <a name="run-the-program"></a>Programı çalıştırma

Programı komut satırından çalıştırın. İsteği otomatik olarak QnA Maker API'sine gönderir, ardından konsol penceresine yazdırılır.

Dosyayı çalıştırın:

```bash
python get-answer-3x.py
```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]

[İstek](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response)hakkında daha fazla bilgi edinin.

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)