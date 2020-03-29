---
title: 'Quickstart: Bilgi tabanından cevap alın - REST, C# - QnA Maker'
description: Bu C# REST tabanlı quickstart programlı bir bilgi tabanından bir cevap alma ile size yol.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 16093ec5e837b098da3c9b038fe2a57cd76c7151
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851811"
---
# <a name="quickstart-get-answers-to-a-question-from-a-knowledge-base-with-c"></a>Hızlı başlangıç: C ile bilgi tabanından bir sorunun yanıtlarını alın #

Bu hızlı başlangıç, programlı bir şekilde yayınlanan bir QnA Maker bilgi tabanından yanıt almanıza yol açtır. Bilgi tabanı, SSS gibi [veri kaynaklarından](../Concepts/knowledge-base.md) gelen soruları ve yanıtları içerir. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) QnA Maker hizmetine gönderilir. [Yanıt,](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) en yüksek öngörülen yanıtı içerir.

[Referans belgeleri](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime) | [Örnek](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/blob/master/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs)

## <a name="prerequisites"></a>Ön koşullar

* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* [Bir QnA Maker hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalı. Anahtarınızı almak için, QnA Maker kaynağınız için Azure panonuzda **Kaynak Yönetimi** altındaki **Anahtarlar'ı** seçin.
* Sayfa ayarlarını **yayımlayın.** Yayımlanmış bir bilgi tabanınız yoksa, boş bir bilgi tabanı oluşturun, ardından **Ayarlar** sayfasındabir bilgi tabanı nı içe aktarın ve yayımlayın. [Bu temel bilgi tabanını](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv)indirebilir ve kullanabilirsiniz.

    Yayımlama sayfası ayarları, POSTA rota değerini, Ana Bilgisayar değerini ve EndpointKey değerini içerir.

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

## <a name="create-a-knowledge-base-project"></a>Bilgi bankası projesi oluşturma

1. Açık Visual Studio 2019 Topluluk baskısı.
1. Yeni bir Konsol Uygulaması (.NET Core) projesi oluşturun ve projeye QnaMakerQuickstart adını ver. Diğer ayarlar için varsayılan değerleri kabul edin.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Program.cs dosyasının üst kısmında, projeye gerekli bağımlılıkları eklemek için tek liyakat ifadesini aşağıdaki satırlarla değiştirin:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=1-3 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

`Program` Sınıfın üst kısmında, içinde `Main`, QnA Maker erişmek için gerekli sabitleri ekleyin. Bu değerler, bilgi tabanını yayımladıktan sonra **Yayımla** sayfasında yer alabilirsiniz.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=9-41 "Add the required constants")]

## <a name="add-a-post-request-to-send-question-and-get-answer"></a>Soru göndermek ve yanıt almak için POST isteği ekleme

Aşağıdaki kod, soruyu bilgi tabanına göndermek için QnA Maker API'ye https isteğinde bulunmaz ve yanıtı alır:

[!code-csharp[Add a POST request to send question to knowledge base](~/samples-qnamaker-csharp/documentation-samples/quickstarts/get-answer/QnAMakerAnswerQuestion/Program.cs?range=43-76 "Add a POST request to send question to knowledge base")]

Üstbilginin `Authorization` değeri dizeiçerir. `EndpointKey`

[İstek](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response)hakkında daha fazla bilgi edinin.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Visual Studio'dan programı oluşturun ve çalıştırın. İsteği otomatik olarak QnA Maker API'sine gönderir, ardından konsol penceresine yazdırılır.

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)]

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)