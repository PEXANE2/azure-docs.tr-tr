---
title: "Hızlı başlangıç: Bilgi Bankası 'ndan yanıt alın-REST, Python-Soru-Cevap Oluşturma"
description: Bu Python REST tabanlı hızlı başlangıç, Bilgi Bankası 'ndan programlama yoluyla yanıt alma konusunda size kılavuzluk eder.
ms.topic: quickstart
ms.date: 01/28/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCHANGE-20200128
ms.openlocfilehash: f439a492e2e63e3f99f80004b387d9cfc415e4b0
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842963"
---
# <a name="quickstart-get-answers-to-a-question-from-a-knowledge-base-with-python"></a>Hızlı başlangıç: Python ile Bilgi Bankası 'ndan bir soruya yanıt alın

Bu hızlı başlangıçta, yayımlanan Soru-Cevap Oluşturma bilgi bankasından programlı bir yanıt alma işlemi adım adım gösterilmektedir. Bilgi Bankası, SSS gibi [veri kaynaklarından](../Concepts/knowledge-base.md) gelen sorular ve yanıtlar içerir. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) soru-cevap oluşturma hizmetine gönderilir. [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) , en fazla tahmini yanıtı içerir.

[Başvuru belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime) | [örneği](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/blob/master/documentation-samples/quickstarts/get-answer/get-answer-3x.py)

## <a name="prerequisites"></a>Ön koşullar

* [Python 3,6 veya üzeri](https://www.python.org/downloads/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için seçin **anahtarları** altında **kaynak yönetimi** Azure Panonuzda soru-cevap Oluşturucu kaynağınızın.
* **Yayımlama** sayfası ayarları. Yayımlanmış bir bilgi tabanınız yoksa, boş bir Bilgi Bankası oluşturun ve ardından **Ayarlar** sayfasına bir Bilgi Bankası alın ve ardından yayımlayın. [Bu temel Bilgi Bankası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv)'nı indirip kullanabilirsiniz.

    Yayımlama sayfası ayarları, POST yol değerini, ana bilgisayar değerini ve EndpointKey değerini içerir.

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

## <a name="create-a-python-file"></a>Python dosyası oluşturma

Adlı yeni bir dosya oluşturun ve açın VSCode `get-answer-3x.py`.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

`get-answer-3x.py` dosyanın en üstünde projeye gerekli bağımlılıkları ekleyin:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=1-2 "Add the required dependencies")]

<!--TBD - reword this following paragraph -->

Konak ve rota **Yayımla** sayfasında göründükleri şekilde farklıdır. Bunun nedeni, Python kitaplığının konakta herhangi bir yönlendirmeye izin vermez. **Yayımla** sayfasında ana bilgisayarın bir parçası olarak görünen yönlendirme yola taşındı.

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

Soru-Cevap Oluşturma erişmek için gerekli sabitleri ekleyin. Bu değerler, Bilgi Bankası 'nı yayımladıktan sonra **Yayımla** sayfasında bulunur.

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=5-25 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-an-answer"></a>Soru göndermek ve yanıt almak için bir POST isteği ekleyin

Aşağıdaki kod, soruyu bilgi tabanına göndermek için Soru-Cevap Oluşturma API'si HTTPS isteği yapar ve yanıtı alır:

[!code-python[Add a POST request to send question to knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/get-answer/get-answer-3x.py?range=27-48 "Add a POST request to send question to knowledge base")]

`Authorization` üst bilgisi değeri dize `EndpointKey`içerir.

## <a name="run-the-program"></a>Programı çalıştırma

Programı komut satırından çalıştırın. Otomatik olarak için soru-cevap Oluşturucu API'si isteği gönderir ve ardından konsol penceresine yazdırır.

Dosyasını çalıştırın:

```bash
python get-answer-3x.py
```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]

[İstek](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response)hakkında daha fazla bilgi edinin.

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
