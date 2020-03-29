---
title: 'Quickstart: Bilgi bankası, REST, Go - QnA Maker yayınlayın'
description: Bu Go REST tabanlı quickstart bilgi tabanınızı yayınlar ve uygulamanızda veya sohbet botunuzda çağrılabilecek bir bitiş noktası oluşturur.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 4ce655bdc7a913ecb281ce8a75e7ec4f2009a2ea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851701"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-go"></a>Hızlı başlangıç: Go kullanarak Soru-Cevap Oluşturma’da bilgi bankası yayımlama

Bu REST tabanlı quickstart programlı bilgi tabanı (KB) yayımlama ile size yol. Yayımlama, bilgi tabanının en son sürümünü özel bir Azure Bilişsel Arama dizinine iter ve uygulamanızda veya sohbet botunuzda çağrılabilecek bir bitiş noktası oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Ön koşullar

* [Go 1.10.1](https://golang.org/dl/)
* [Bir QnA Maker hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalı. Anahtar ve bitiş noktanızı (kaynak adını içerir) almak için Azure portalındaki kaynağınız için **Hızlı Başlat'ı** seçin.

* QnA Maker bilgi bankası (KB) Kimliği `kbid` URL'de bulunan sorgu dize parametresi aşağıda gösterildiği gibi.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-csharp.md).

> [!NOTE]
> Tam çözüm dosyası(lar) Azure [ **Örnekleri/bilişsel-hizmetler-qnamaker-go** GitHub deposundan](https://github.com/Azure-Samples/cognitive-services-qnamaker-go/tree/master/documentation-samples/quickstarts/publish-knowledge-base)edinilebilir.

## <a name="create-a-go-file"></a>Go dosyası oluşturma

VSCode'u açın ve `publish-kb.go`adında yeni bir dosya oluşturun.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Aşağıdaki satırları `publish-kb.go` adlı dosyanın en üstüne ekleyerek projeye gerekli bağımlılıkları dahil edin:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/publish-knowledge-base/publish-kb.go?range=3-7 "Add the required dependencies")]

## <a name="create-the-main-function"></a>Ana işlevi oluşturma

Gerekli bağımlılıklar sonra, aşağıdaki sınıfı ekleyin:

```Go
package main

func main() {

}
```

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

Ana **içinde**


 fonksiyonu, QnA Maker erişmek için gerekli sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/publish-knowledge-base/publish-kb.go?range=16-20 "Add the required constants")]

## <a name="add-post-request-to-publish-kb"></a>KB yayımlamak için POST isteğini ekleme

Gerekli sabitlerden sonra, bir bilgi tabanı yayımlamak için QnA Maker API'ye https isteğinde bulunan ve yanıtı alan aşağıdaki kodu ekleyin:

[!code-go[Add a POST request to publish KB](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=35-48 "Add a POST request to publish KB")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. Kod 204 yanıtları için içerik ekler.

Diğer yanıtlarda döndürülen yanıt değiştirilmez.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Dosyayı derlemek için aşağıdaki komutu girin. Komut istemi başarılı bir derleme için herhangi bir bilgi döndürmez.

```bash
go build publish-kb.go
```

Programı çalıştırmak için aşağıdaki komutu bir komut satırına yazın. KB'yi yayımlamak için QnA Maker API'ye istek gönderecek, ardından başarı veya hatalar için 204'ün çıktısını alacaktır.

```bash
./publish-kb
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi tabanı yayımlandıktan [sonra, bir yanıt oluşturmak için bitiş noktası URL'sine](./get-answer-from-knowledge-base-go.md)ihtiyacınız var.

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)