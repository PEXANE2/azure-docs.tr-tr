---
title: 'Quickstart: Bilgi tabanından cevap alın - REST, Go - QnA Maker'
description: Bu Go REST tabanlı quickstart programlı bir bilgi tabanından bir cevap alma ile size yol.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 37a8925f8460e448dce3b66354f5181fe103a6a8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851800"
---
# <a name="quickstart-get-answers-to-a-question-from-a-knowledge-base-with-go"></a>Hızlı başlangıç: Go ile bir bilgi tabanından bir sorunun yanıtlarını alın

Bu hızlı başlangıç, programlı bir şekilde yayınlanan bir QnA Maker bilgi tabanından yanıt almanıza yol açtır. Bilgi tabanı, SSS gibi [veri kaynaklarından](../Concepts/knowledge-base.md) gelen soruları ve yanıtları içerir. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) QnA Maker hizmetine gönderilir. [Yanıt,](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) en yüksek öngörülen yanıtı içerir.

[Referans belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime) | [Örnek](https://github.com/Azure-Samples/cognitive-services-qnamaker-go/blob/master/documentation-samples/quickstarts/get-answer/get-answer.go)

## <a name="prerequisites"></a>Ön koşullar

* [Go 1.10.1](https://golang.org/dl/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Bir QnA Maker hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalı. Anahtarınızı almak için, QnA Maker kaynağınız için Azure panonuzda **Kaynak Yönetimi** altındaki **Anahtarlar'ı** seçin.
* Sayfa ayarlarını **yayımlayın.** Yayımlanmış bir bilgi tabanınız yoksa, boş bir bilgi tabanı oluşturun, ardından **Ayarlar** sayfasındabir bilgi tabanı nı içe aktarın ve yayımlayın. [Bu temel bilgi tabanını](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv)indirebilir ve kullanabilirsiniz.

    Yayımlama sayfası ayarları, POSTA rota değerini, Ana Bilgisayar değerini ve EndpointKey değerini içerir.

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

## <a name="create-a-go-file"></a>Go dosyası oluşturma

VSCode'u açın ve `get-answer.go` adlı yeni bir dosya oluşturun ve aşağıdaki sınıfı ekleyin:

```Go
package main

func main() {

}
```

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

İşlevin `main` üzerinde, `get-answer.go` dosyanın üst kısmında, projeye gerekli bağımlılıkları ekleyin:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=3-9 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

`main` Fonksiyonun üst kısmında, QnA Maker erişmek için gerekli sabitleri ekleyin. Bu değerler, bilgi tabanını yayımladıktan sonra **Yayımla** sayfasında yer alabilirsiniz.

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=17-33 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Soru göndermek ve yanıt almak için POST isteği ekleme

Aşağıdaki kod, soruyu bilgi tabanına göndermek için QnA Maker API'ye https isteğinde bulunmaz ve yanıtı alır:

[!code-go[Add a POST request to send question to knowledge base](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=35-48 "Add a POST request to send question to knowledge base")]

Üstbilginin `Authorization` değeri dizeiçerir. `EndpointKey`

[İstek](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response)hakkında daha fazla bilgi edinin.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Komut satırından programı oluşturun ve çalıştırın. İsteği otomatik olarak QnA Maker API'sine gönderir, ardından konsol penceresine yazdırılır.

1. Dosyayı oluşturun:

    ```bash
    go build get-answer.go
    ```

1. Dosyayı çalıştırın:

    ```bash
    ./get-answer
    ```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)