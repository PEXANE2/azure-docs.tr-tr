---
title: "Hızlı Başlangıç: Bilgi Bankası 'ndan yanıt alın-REST, Java-Soru-Cevap Oluşturma"
titleSuffix: Azure Cognitive Services
description: Bu Java REST tabanlı hızlı başlangıç, Bilgi Bankası 'ndan programlama yoluyla yanıt alma konusunda size kılavuzluk eder.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 2b888a275b08c7011c6e0b60ff1cd1d70b42f465
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559832"
---
# <a name="get-answers-to-a-question-from-a-knowledge-base-with-java"></a>Java ile Bilgi Bankası 'ndan bir soruya yanıt alın

Bu hızlı başlangıçta, yayımlanan Soru-Cevap Oluşturma bilgi bankasından programlı bir yanıt alma işlemi adım adım gösterilmektedir. Bilgi Bankası, SSS gibi [veri kaynaklarından](../Concepts/data-sources-supported.md) gelen sorular ve yanıtlar içerir. [Soru](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) soru-cevap oluşturma hizmetine gönderilir. [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response-properties) , en fazla tahmini yanıtı içerir. 

## <a name="prerequisites"></a>Önkoşullar

* [JDK SE](https://aka.ms/azure-jdks)  (Java Geliştirme Seti, Standart Sürüm)
* Bu örnek, Apache kullanır [HTTP istemcisi](https://hc.apache.org/httpcomponents-client-ga/) HTTP Components'tan. Projenize aşağıdaki Apache HTTP istemcisi kitaplıkları ekleme yapmanız gerekir: 
    * httpclient 4.5.3.jar
    * httpcore 4.4.6.jar
    * Commons günlük 1.2.jar
* [Visual Studio Code](https://code.visualstudio.com/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için seçin **anahtarları** altında **kaynak yönetimi** Azure Panonuzda soru-cevap Oluşturucu kaynağınızın. 
* **Yayımlama** sayfası ayarları. Yayımlanmış bir bilgi tabanınız yoksa, boş bir Bilgi Bankası oluşturun ve ardından **Ayarlar** sayfasına bir Bilgi Bankası alın ve ardından yayımlayın. [Bu temel Bilgi Bankası](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/knowledge-bases/basic-kb.tsv)'nı indirip kullanabilirsiniz. 

    Yayımlama sayfası ayarları, POST yol değerini, ana bilgisayar değerini ve EndpointKey değerini içerir. 

    ![Yayımlama ayarları](../media/qnamaker-quickstart-get-answer/publish-settings.png)

Bu hızlı başlangıç [https://github.com/Azure-Samples/cognitive-services-qnamaker-java](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/tree/master/documentation-samples/quickstarts/get-answer) kodu depoda. 

## <a name="create-a-java-file"></a>Java dosyası oluşturma

Vscode 'u açın ve adlı `GetAnswer.java` yeni bir dosya oluşturun ve aşağıdaki sınıfı ekleyin:

```Java
public class GetAnswer {

    public static void main(String[] args) 
    {

    }
}
```

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Bu hızlı başlangıçta HTTP istekleri için Apache sınıfları kullanılmaktadır. Getanswer sınıfının üstünde, `GetAnswer.java` dosyanın en üstünde, projeye gerekli bağımlılıkları ekleyin:

[!code-java[Add the required dependencies](~/samples-qnamaker-java/documentation-samples/quickstarts/get-answer/GetAnswer.java?range=5-13 "Add the required dependencies")]

## <a name="add-the-required-constants"></a>Gerekli sabitleri ekleme

`GetAnswer.java` Sınıfının üst kısmında, soru-cevap oluşturma erişmek için gereken sabitleri ekleyin. Bu değerler, Bilgi Bankası 'nı yayımladıktan sonra **Yayımla** sayfasında bulunur.  

[!code-java[Add the required constants](~/samples-qnamaker-java/documentation-samples/quickstarts/get-answer/GetAnswer.java?range=26-42 "Add the required constants")]

## <a name="add-a-post-request-to-send-question"></a>Soru göndermek için bir POST isteği ekleyin

Aşağıdaki kod, soruyu bilgi tabanına göndermek için Soru-Cevap Oluşturma API'si HTTPS isteği yapar ve yanıtı alır:

[!code-java[Add a POST request to send question to knowledge base](~/samples-qnamaker-java/documentation-samples/quickstarts/get-answer/GetAnswer.java?range=44-72 "Add a POST request to send question to knowledge base")]

Üstbilginin değeri dizeyi `EndpointKey`içerir. `Authorization` 

[İstek](../how-to/metadata-generateanswer-usage.md#generateanswer-request) ve [Yanıt](../how-to/metadata-generateanswer-usage.md#generateanswer-response)hakkında daha fazla bilgi edinin.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Derleme ve komut satırından programı çalıştırın. Otomatik olarak için soru-cevap Oluşturucu API'si isteği gönderir ve ardından konsol penceresine yazdırır.

1. Derleme dosyası:

    ```bash
    javac -cp "lib/*" GetAnswer.java
    ```

1. Dosyasını çalıştırın:

    ```bash
    java -cp ".;lib/*" GetAnswer
    ```

[!INCLUDE [JSON request and response](../../../../includes/cognitive-services-qnamaker-quickstart-get-answer-json.md)] 


[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
