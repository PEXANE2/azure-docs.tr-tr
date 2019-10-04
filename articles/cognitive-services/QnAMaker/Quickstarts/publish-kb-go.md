---
title: "Hızlı başlangıç: Bilgi Bankası 'nı yayımlama, REST, go-Soru-Cevap Oluşturma"
titleSuffix: Azure Cognitive Services
description: Bu, REST tabanlı hızlı başlangıçta, sınanan Bilgi Bankası 'nın en son sürümünü yayımlanan Bilgi Bankası 'nı temsil eden ayrılmış bir Azure Search dizinine gönderen bilgi bankasını yayımlama konusunda size yol gösterir. Ayrıca, uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 10/02/2019
ms.author: diberry
ms.openlocfilehash: 20e48d0c16281d6b1399d46f5ca9a7781cbaeea7
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71840200"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-go"></a>Hızlı başlangıç: go kullanarak Soru-Cevap Oluşturma bilgi tabanı yayımlama

Bu REST tabanlı hızlı başlangıç, bilgi bankanızı (KB) programlı bir şekilde yayımlama konusunda size yol gösterir. Yayımlama, bilgi tabanının en son sürümünü adanmış bir Azure Search dizinine gönderir ve uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.

Bu hızlı başlangıç Soru-Cevap Oluşturma API 'Leri çağırır:
* [Yayımla](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish) -bu API, isteğin gövdesinde herhangi bir bilgi gerektirmez.

## <a name="prerequisites"></a>Prerequisites

* [Go 1.10.1](https://golang.org/dl/)
* Bir [soru-cevap oluşturma hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve uç noktanızı (kaynak adını da içerir) almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin.

* Aşağıda gösterildiği gibi, KBID sorgu dizesi parametresindeki URL 'de Soru-Cevap Oluşturma Bilgi Bankası (KB) KIMLIĞI bulundu.

    ![Soru-Cevap Oluşturma Bilgi Bankası KIMLIĞI](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi tabanınız yoksa, bu hızlı başlangıç için kullanmak üzere bir örnek oluşturabilirsiniz: [Yeni bir Bilgi Bankası oluşturun](create-new-kb-csharp.md).

> [!NOTE] 
> Tam çözüm dosyası (ler) [ **Azure-Samples/bilişsel hizmetler-qnamaker-go** GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-qnamaker-go/tree/master/documentation-samples/quickstarts/publish-knowledge-base)bulunur.

## <a name="create-a-go-file"></a>Go dosyası oluşturma

VSCode 'u açın ve `publish-kb.go` adlı yeni bir dosya oluşturun.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları Ekle

@No__t-0 ' nın üstünde, gerekli bağımlılıkları projeye eklemek için aşağıdaki satırları ekleyin:

[!code-go[Add the required dependencies](~/samples-qnamaker-go/documentation-samples/quickstarts/publish-knowledge-base/publish-kb.go?range=3-7 "Add the required dependencies")]

## <a name="create-the-main-function"></a>Main işlevini oluşturma

Gerekli bağımlılıklardan sonra aşağıdaki sınıfı ekleyin:

```Go
package main

func main() {

}
```

## <a name="add-required-constants"></a>Gerekli sabitleri Ekle

**Ana** iç


 işlevine erişmek için gerekli sabitleri ekleyin Soru-Cevap Oluşturma. Değerleri kendi değerlerinizle değiştirin.

[!code-go[Add the required constants](~/samples-qnamaker-go/documentation-samples/quickstarts/publish-knowledge-base/publish-kb.go?range=16-20 "Add the required constants")]

## <a name="add-post-request-to-publish-kb"></a>Yayımla BB 'ye POST isteği Ekle

Gerekli sabitlerden sonra, bir Bilgi Bankası yayımlamak ve yanıtı almak için Soru-Cevap Oluşturma API'si HTTPS isteği yapan aşağıdaki kodu ekleyin:

[!code-go[Add a POST request to publish KB](~/samples-qnamaker-go/documentation-samples/quickstarts/get-answer/get-answer.go?range=35-48 "Add a POST request to publish KB")]

API çağrısı, yanıt gövdesinde içerik olmadan başarılı bir yayımlama için 204 durumu döndürür. Kod, 204 yanıt için içerik ekler.

Diğer herhangi bir yanıt için, bu yanıt değiştirilmemiş olarak döndürülür.

## <a name="build-and-run-the-program"></a>Programı derleyin ve çalıştırın

Dosyayı derlemek için aşağıdaki komutu girin. Komut istemi, başarılı bir derleme için herhangi bir bilgi döndürmez.

```bash
go build publish-kb.go
```

Programı çalıştırmak için komut satırına aşağıdaki komutu girin. Bu işlem, KB 'yi yayımlamak için Soru-Cevap Oluşturma API'si isteği gönderir, sonra başarı veya hatalar için 204 yazdır.

```bash
./publish-kb
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası yayımlandıktan sonra, [bir yanıt oluşturmak için uç nokta URL 'sine](../Tutorials/create-publish-answer.md#generating-an-answer)ihtiyacınız vardır. 

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (v4) REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
