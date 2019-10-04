---
title: 'Hızlı başlangıç: yayımlama bilgi tabanı, REST, Python-Soru-Cevap Oluşturma'
titleSuffix: Azure Cognitive Services
description: Bu Python REST tabanlı hızlı başlangıç, bilgi bankasını yayımlarken, sınanan Bilgi Bankası 'nın en son sürümünü yayımlanan Bilgi Bankası 'nı temsil eden ayrılmış bir Azure Search dizinine gönderen size kılavuzluk eder. Ayrıca, uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 10/01/2019
ms.author: diberry
ms.openlocfilehash: 09523bb6c9c8a58f5f7bd102d2ac30ad77f28d1c
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71840182"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-python"></a>Hızlı başlangıç: Python kullanarak Soru-Cevap Oluşturma bilgi tabanı yayımlama

Bu REST tabanlı hızlı başlangıç, bilgi bankanızı (KB) programlı bir şekilde yayımlama konusunda size yol gösterir. Yayımlama, bilgi tabanının en son sürümünü adanmış bir Azure Search dizinine gönderir ve uygulamanızda veya sohbet bot 'ta çağrılabilecek bir uç nokta oluşturur.

Bu hızlı başlangıçta Soru-Cevap Oluşturma REST API 'Leri çağrıları yapılır:
* [Yayımla](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish) -bu API, isteğin gövdesinde herhangi bir bilgi gerektirmez.

## <a name="prerequisites"></a>Prerequisites

* [Python 3,7](https://www.python.org/downloads/)
* Bir [soru-cevap oluşturma hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalıdır. Anahtarınızı ve uç noktanızı (kaynak adını da içerir) almak için Azure portal kaynağınız için **hızlı başlangıç** ' ı seçin.
* Aşağıda gösterildiği gibi, KBID sorgu dizesi parametresindeki URL 'de Soru-Cevap Oluşturma Bilgi Bankası (KB) KIMLIĞI bulundu.

    ![Soru-Cevap Oluşturma Bilgi Bankası KIMLIĞI](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi tabanınız yoksa, bu hızlı başlangıç için kullanmak üzere bir örnek oluşturabilirsiniz: [Yeni bir Bilgi Bankası oluşturun](../how-to/create-knowledge-base.md).

> [!NOTE] 
> Tam çözüm dosyası (ler) [ **Azure-Samples/bilişsel hizmetler-qnamaker-Python** GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/tree/master/documentation-samples/quickstarts/publish-knowledge-base)bulunur.

## <a name="create-a-knowledge-base-python-file"></a>Bilgi Bankası Python dosyası oluşturma

@No__t-0 adlı bir dosya oluşturun.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları Ekle

@No__t-0 ' nın üstünde, gerekli bağımlılıkları projeye eklemek için aşağıdaki satırları ekleyin:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=1-1 "Add the required dependencies")]

## <a name="add-required-constants"></a>Gerekli sabitleri Ekle

Önceki gerekli bağımlılıklardan sonra, Soru-Cevap Oluşturma erişmek için gerekli sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=5-15 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Bilgi Bankası yayınlamak için POST isteği ekleyin

Gerekli sabitlerden sonra, bir Bilgi Bankası yayımlamak ve yanıtı almak için Soru-Cevap Oluşturma API'si HTTPS isteği yapan aşağıdaki kodu ekleyin:

[!code-python[Add a POST request to publish knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=17-26 "Add a POST request to publish knowledge base")]

API çağrısı, yanıt gövdesinde içerik olmadan başarılı bir yayımlama için 204 durumu döndürür. Kod, 204 yanıt için içerik ekler.

Diğer herhangi bir yanıt için, bu yanıt değiştirilmemiş olarak döndürülür.

## <a name="build-and-run-the-program"></a>Programı derleyin ve çalıştırın

Programı çalıştırmak için komut satırına aşağıdaki komutu girin. Bilgi Bankası 'nı yayımlamak için Soru-Cevap Oluşturma API'si isteği gönderir, sonra başarı veya hatalar için 204 yazdır.

```bash
python publish-kb-3x.py
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası yayımlandıktan sonra, [bir yanıt oluşturmak için uç nokta URL 'sine](../Tutorials/create-publish-answer.md#generating-an-answer)ihtiyacınız vardır. 

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (v4) REST API başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)

[Soru-Cevap Oluşturma Genel Bakış](../Overview/overview.md)
