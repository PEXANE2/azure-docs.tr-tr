---
title: 'Hızlı başlangıç: yayımlama bilgi tabanı, REST, Java-Soru-Cevap Oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu Java REST tabanlı hızlı başlangıç bilgi tabanınızı yayımlar ve uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 12/16/2019
ms.author: diberry
ms.openlocfilehash: 44b53cbfdb1982d9f9e6a0cb6408a16b1d660d2e
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75447418"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-java"></a>Hızlı başlangıç: Java kullanarak Soru-Cevap Oluşturma’da bilgi bankası yayımlama

REST tabanlı bu hızlı başlangıçta, Bilgi Bankası (KB) program aracılığıyla yayımlama aracılığıyla size yol gösterir. Yayımlama, bilgi tabanının en son sürümünü adanmış bir Azure Bilişsel Arama dizinine gönderir ve uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Ön koşullar

* [JDK SE](https://aka.ms/azure-jdks)  (Java Geliştirme Seti, Standart Sürüm)
* Bu örnek, Apache kullanır [HTTP istemcisi](https://hc.apache.org/httpcomponents-client-ga/) HTTP Components'tan. Projenize aşağıdaki Apache HTTP istemcisi kitaplıkları ekleme yapmanız gerekir:
    * httpclient 4.5.3.jar
    * httpcore 4.4.6.jar
    * Commons günlük 1.2.jar
* [Visual Studio Code](https://code.visualstudio.com/)
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı ve uç noktanızı (kaynak adını da içerir) almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin.
* Aşağıda gösterildiği gibi, `kbid` sorgu dizesi parametresindeki URL 'de Soru-Cevap Oluşturma Bilgi Bankası (KB) KIMLIĞI bulundu.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-csharp.md).

> [!NOTE]
> Tam çözüm dosyası (ler) [ **Azure-Samples/bilişsel hizmetler-qnamaker-Java** GitHub deposundan](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/tree/master/documentation-samples/quickstarts/publish-knowledge-base)alınabilir.

## <a name="create-a-java-file"></a>Bir Java dosyası oluşturma

Adlı yeni bir dosya oluşturun ve açın VSCode `PublishKB.java`.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Üst kısmındaki `PublishKB.java`, sınıfı üzerinde gerekli bağımlılıkları projeye eklemek için aşağıdaki satırları ekleyin:

[!code-java[Add the required dependencies](~/samples-qnamaker-java/documentation-samples/quickstarts/publish-knowledge-base/PublishKB.java?range=1-13 "Add the required dependencies")]

## <a name="create-publishkb-class-with-main-method"></a>Main yöntemiyle PublishKB sınıfı oluşturma

Bağımlılıklardan sonra aşağıdaki sınıfı ekleyin:

```Go
public class PublishKB {

    public static void main(String[] args)
    {
    }
}
```

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

İçinde **ana** yöntemi, soru-cevap Oluşturucu erişmek için gerekli sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

[!code-java[Add the required constants](~/samples-qnamaker-java/documentation-samples/quickstarts/publish-knowledge-base/PublishKB.java?range=27-30 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Bilgi Bankası yayımlama için POST isteği Ekle

Sonra gerekli sabitleri, Bilgi Bankası yayımlama için soru-cevap Oluşturucu API'si bir HTTPS isteği yapar ve yanıtı alan aşağıdaki kodu ekleyin:

[!code-java[Add a POST request to publish knowledge base](~/samples-qnamaker-java/documentation-samples/quickstarts/publish-knowledge-base/PublishKB.java?range=32-44 "Add a POST request to publish knowledge base")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. Kod 204 yanıtları için içerik ekler.

Diğer yanıtlarda döndürülen yanıt değiştirilmez.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Derleme ve komut satırından programı çalıştırın. Otomatik olarak için soru-cevap Oluşturucu API'si isteği gönderir ve ardından konsol penceresine yazdırır.

1. Derleme dosyası:

    ```bash
    javac -cp "lib/*" PublishKB.java
    ```

1. Dosyasını çalıştırın:

    ```bash
    java -cp ".;lib/*" PublishKB
    ```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası yayımlandıktan sonra ihtiyacınız [yanıt oluşturmak için uç nokta URL'si](../Tutorials/create-publish-answer.md#generating-an-answer).

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
