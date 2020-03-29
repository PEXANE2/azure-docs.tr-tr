---
title: 'Quickstart: Bilgi tabanı, REST, C# - QnA Maker yayınlayın'
description: Bu C# REST tabanlı quickstart bilgi tabanınızı yayınlar ve uygulamanızda veya sohbet botunuzda çağrılabilen bir bitiş noktası oluşturur.
ms.date: 02/08/2020
ROBOTS: NOINDEX,NOFOLLOW
ms.custom: RESTCURL2020FEB27
ms.topic: conceptual
ms.openlocfilehash: 0a2f64795d82928e33a10c7e9d162f2333cdf7bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78851712"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-c"></a>Hızlı başlangıç: C# kullanarak Soru-Cevap Oluşturma’da bilgi bankası yayımlama

Bu REST tabanlı quickstart programlı bilgi tabanı (KB) yayımlama ile size yol. Yayımlama, bilgi tabanının en son sürümünü özel bir Azure Bilişsel Arama dizinine iter ve uygulamanızda veya sohbet botunuzda çağrılabilecek bir bitiş noktası oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Ön koşullar

* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* [Bir QnA Maker hizmetiniz](../How-To/set-up-qnamaker-service-azure.md)olmalı. Anahtar ve bitiş noktanızı (kaynak adını içerir) almak için Azure portalındaki kaynağınız için **Hızlı Başlat'ı** seçin.
* QnA Maker bilgi bankası (KB) Kimliği `kbid` URL'de bulunan sorgu dize parametresi aşağıda gösterildiği gibi.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi bankanız yoksa, bu hızlı başlangıçta kullanmak için bir örneğini oluşturabilirsiniz: [Yeni bilgi bankası oluşturma](create-new-kb-csharp.md).

> [!NOTE]
> Tam çözüm dosyası(lar) Azure [ **Örnekleri/bilişsel hizmetler-qnamaker-csharp** GitHub deposundan](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/publish-knowledge-base)edinilebilir.

## <a name="create-knowledge-base-project"></a>Bilgi bankası projesi oluşturma

1. Açık Visual Studio 2019 Topluluk baskısı.
1. Yeni bir **Konsol Uygulaması (.NET Core)** `QnaMakerQuickstart`projesi oluşturun ve projeyi adlandırın. Diğer ayarlar için varsayılan değerleri kabul edin.

## <a name="add-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Program.cs dosyasının en üst kısmındaki tek using deyimini aşağıdaki satırlarla değiştirerek projeye gerekli bağımlılıkları ekleyin:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=1-2 "Add the required dependencies")]

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

**Program** sınıfında, QnA Maker'a erişmek için gerekli sabitleri ekleyin.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=8-34 "Add the required constants")]

## <a name="add-the-main-method-to-publish-the-knowledge-base"></a>Bilgi tabanını yayımlamak için Ana yöntemi ekleyin

Gerekli sabitlerden sonra, bir bilgi tabanı yayımlamak için QnA Maker API'ye https isteğinde bulunan ve yanıtı alan aşağıdaki kodu ekleyin:

[!code-csharp[Add HTTP Post request and response](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=36-56 "Add HTTP Post request and response")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür.

## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Programı derleyin ve çalıştırın. Bilgi tabanını yayımlamak için isteği otomatik olarak QnA Maker API'sine gönderir, ardından yanıt konsol penceresine yazdırılır.

Bilgi bankanız yayımlandıktan sonra istemci uygulaması veya sohbet botu ile uç noktadan sorgulayabilirsiniz.

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilgi tabanı yayımlandıktan [sonra, bir yanıt oluşturmak için bitiş noktası URL'sine](./get-answer-from-knowledge-base-csharp.md)ihtiyacınız var.

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)