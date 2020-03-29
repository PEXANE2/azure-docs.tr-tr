---
title: 'Quickstart: Bilgi bankası, REST, Java - QnA Maker yayınlayın'
description: Bu Java REST tabanlı quickstart bilgi tabanınızı yayınlar ve uygulamanızda veya sohbet botunuzda çağrılabilecek bir bitiş noktası oluşturur.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 149d7963f29bf041cda75fffaac533e0a62ee7a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851690"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-java"></a>Hızlı başlangıç: Java kullanarak Soru-Cevap Oluşturma’da bilgi bankası yayımlama

Bu REST tabanlı quickstart programlı bilgi tabanı (KB) yayımlama ile size yol. Yayımlama, bilgi tabanının en son sürümünü özel bir Azure Bilişsel Arama dizinine iter ve uygulamanızda veya sohbet botunuzda çağrılabilecek bir bitiş noktası oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Ön koşullar

* [JDK SE](https://aka.ms/azure-jdks)  (Java Geliştirme Seti, Standart Sürüm)
* Bu örnek, HTTP Bileşenleri'ndeki Apache [HTTP istemcisini](https://hc.apache.org/httpcomponents-client-ga/) kullanır. Projenize aşağıdaki Apache HTTP istemci kitaplıklarını eklemeniz gerekir:
    * httpclient-4.5.3.jar
    * httpcore-4.4.6.jar
    * commons-logging-1.2.jar
* [Visual Studio Code](https://code.visualstudio.com/)
* [Bir QnA Maker hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalı. Anahtar ve bitiş noktanızı (kaynak adını içerir) almak için Azure portalındaki kaynağınız için **Hızlı Başlat'ı** seçin.
* QnA Maker bilgi bankası (KB) Kimliği `kbid` URL'de bulunan sorgu dize parametresi aşağıda gösterildiği gibi.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-csharp.md).

> [!NOTE]
> Tam çözüm dosyası(lar) Azure [ **Örnekleri/bilişsel hizmetler-qnamaker-java** GitHub deposundan](https://github.com/Azure-Samples/cognitive-services-qnamaker-java/tree/master/documentation-samples/quickstarts/publish-knowledge-base)edinilebilir.

## <a name="create-a-java-file"></a>Java dosyası oluşturma

VSCode'u açın ve `PublishKB.java`adında yeni bir dosya oluşturun.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Sınıfın üst `PublishKB.java`kısmında, projeye gerekli bağımlılıkları eklemek için aşağıdaki satırları ekleyin:

[!code-java[Add the required dependencies](~/samples-qnamaker-java/documentation-samples/quickstarts/publish-knowledge-base/PublishKB.java?range=1-13 "Add the required dependencies")]

## <a name="create-publishkb-class-with-main-method"></a>Ana yöntemle PublishKB sınıfı oluşturma

Bağımlılıklar sonra, aşağıdaki sınıf ekleyin:

```Go
public class PublishKB {

    public static void main(String[] args)
    {
    }
}
```

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

**Ana** yöntemde, QnA Maker erişmek için gerekli sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

[!code-java[Add the required constants](~/samples-qnamaker-java/documentation-samples/quickstarts/publish-knowledge-base/PublishKB.java?range=27-30 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Bilgi tabanını yayınlamak için POST isteği ekleme

Gerekli sabitlerden sonra, bir bilgi tabanı yayımlamak için QnA Maker API'ye https isteğinde bulunan ve yanıtı alan aşağıdaki kodu ekleyin:

[!code-java[Add a POST request to publish knowledge base](~/samples-qnamaker-java/documentation-samples/quickstarts/publish-knowledge-base/PublishKB.java?range=32-44 "Add a POST request to publish knowledge base")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. Kod 204 yanıtları için içerik ekler.

Diğer yanıtlarda döndürülen yanıt değiştirilmez.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Komut satırından programı oluşturun ve çalıştırın. İsteği otomatik olarak QnA Maker API'sine gönderir, ardından konsol penceresine yazdırılır.

1. Dosyayı oluşturun:

    ```bash
    javac -cp "lib/*" PublishKB.java
    ```

1. Dosyayı çalıştırın:

    ```bash
    java -cp ".;lib/*" PublishKB
    ```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi tabanı yayımlandıktan [sonra, bir yanıt oluşturmak için bitiş noktası URL'sine](./get-answer-from-knowledge-base-java.md)ihtiyacınız var.

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)