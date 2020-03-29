---
title: 'Quickstart: Bilgi tabanı, REST, Python - QnA Maker yayımlayın'
description: Bu Python REST tabanlı quickstart bilgi tabanınızı yayınlar ve uygulamanızda veya sohbet botunuzda çağrılabilecek bir bitiş noktası oluşturur.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 6f053ecbd58d3c2527c1b904437dcc4715c76af1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851657"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-python"></a>Hızlı başlangıç: Python kullanarak Soru-Cevap Oluşturma’da bilgi bankası yayımlama

Bu REST tabanlı quickstart programlı bilgi tabanı (KB) yayımlama ile size yol. Yayımlama, bilgi tabanının en son sürümünü özel bir Azure Bilişsel Arama dizinine iter ve uygulamanızda veya sohbet botunuzda çağrılabilecek bir bitiş noktası oluşturur.

Bu hızlı başlangıç QnA Maker REST API'leri çağırır:
* [Publish](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Ön koşullar

* [Python 3.7](https://www.python.org/downloads/)
* [Bir QnA Maker hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalı. Anahtar ve bitiş noktanızı (kaynak adını içerir) almak için Azure portalındaki kaynağınız için **Hızlı Başlat'ı** seçin.
* QnA Maker bilgi bankası (KB) Kimliği `kbid` URL'de bulunan sorgu dize parametresi aşağıda gösterildiği gibi.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](../how-to/create-knowledge-base.md).

> [!NOTE]
> Tam çözüm dosyası(lar) Azure [ **Örnekleri/bilişsel hizmetler-qnamaker-python** GitHub deposundan](https://github.com/Azure-Samples/cognitive-services-qnamaker-python/tree/master/documentation-samples/quickstarts/publish-knowledge-base)edinilebilir.

## <a name="create-a-knowledge-base-python-file"></a>Bilgi bankası Python dosyası oluşturma

`publish-kb-3x.py` adlı bir dosya oluşturun.

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Aşağıdaki satırları `publish-kb-3x.py` adlı dosyanın en üstüne ekleyerek projeye gerekli bağımlılıkları dahil edin:

[!code-python[Add the required dependencies](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=1-1 "Add the required dependencies")]

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

Yukarıdaki gerekli bağımlılıklardan sonra Soru-Cevap Oluşturma hizmetine erişmek için gerekli sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

[!code-python[Add the required constants](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=5-15 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Bilgi tabanını yayınlamak için POST isteği ekleme

Gerekli sabitlerden sonra, bir bilgi tabanı yayımlamak için QnA Maker API'ye https isteğinde bulunan ve yanıtı alan aşağıdaki kodu ekleyin:

[!code-python[Add a POST request to publish knowledge base](~/samples-qnamaker-python/documentation-samples/quickstarts/publish-knowledge-base/publish-kb-3x.py?range=17-26 "Add a POST request to publish knowledge base")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. Kod 204 yanıtları için içerik ekler.

Diğer yanıtlarda döndürülen yanıt değiştirilmez.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Programı çalıştırmak için aşağıdaki komutu bir komut satırına yazın. Bilgi tabanını yayımlamak için QnA Maker API'ye istek gönderecek, ardından başarı veya hatalar için 204'ün çıktısını alacaktır.

```bash
python publish-kb-3x.py
```

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi tabanı yayımlandıktan [sonra, bir yanıt oluşturmak için bitiş noktası URL'sine](./get-answer-from-knowledge-base-python.md)ihtiyacınız var.

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)

[Soru-Cevap Oluşturma’ya genel bakış](../Overview/overview.md)