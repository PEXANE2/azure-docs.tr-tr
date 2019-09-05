---
title: 'Hızlı Başlangıç: Bilgi Bankası, REST, C# -soru-cevap oluşturma yayımlama'
titleSuffix: Azure Cognitive Services
description: Bu C# REST tabanlı bir hızlı başlangıç size yol gösterir, yayımlanan Bilgi Bankası temsil eden adanmış bir Azure Search dizini için test edilmiş Bilgi Bankası en son sürümünü iter, Bilgi Bankası yayımlama aracılığıyla. Ayrıca uygulamanızda veya sohbet botunuzda çağrılabilecek bir uç nokta da oluşturulur.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 09/03/2019
ms.author: diberry
ms.openlocfilehash: dbd6bddd06713cae7ac69d19e8d13f20fa04f6d0
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70306882"
---
# <a name="quickstart-publish-a-knowledge-base-in-qna-maker-using-c"></a>Hızlı Başlangıç: Soru-Cevap Oluşturma kullanarak Bilgi Bankası yayımlamaC#

REST tabanlı bu hızlı başlangıçta, Bilgi Bankası (KB) program aracılığıyla yayımlama aracılığıyla size yol gösterir. Yayımlama, bilgi bankanızın son sürümünü adanmış bir Azure Search dizinine gönderir ve uygulamanızda ya da sohbet botunuzda çağrılabilecek bir uç nokta oluşturur.

Bu hızlı başlangıç şu Soru-Cevap Oluşturma API'lerini çağırır:
* [Publish](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase/publish): Bu API için istek gövdesinde herhangi bir bilgi iletilmesi gerekmez.

## <a name="prerequisites"></a>Önkoşullar

* En son [**Visual Studio Community sürümü**](https://www.visualstudio.com/downloads/).
* [Soru-Cevap Oluşturma hizmetine](../How-To/set-up-qnamaker-service-azure.md) sahip olmanız gerekir. Anahtarınızı almak için, panonuzda **Kaynak Yönetimi** altında **Anahtarlar** öğesini seçin. 
* Soru-Cevap Oluşturma bilgi bankası (KB) kimliği aşağıda gösterildiği gibi URL'nin kbid sorgu dizesi bölümünde bulunur.

    ![Soru-Cevap Oluşturma bilgi bankası kimliği](../media/qnamaker-quickstart-kb/qna-maker-id.png)

    Henüz bir bilgi tabanınız yoksa, bu hızlı başlangıç için kullanmak üzere bir örnek oluşturabilirsiniz: [Yeni bir Bilgi Bankası oluşturun](create-new-kb-csharp.md).

> [!NOTE] 
> Tam çözüm dosyası (ler) [ **Azure-Samples/bilişsel hizmetler-qnamaker-CSharp** GitHub deposundan](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/quickstarts/publish-knowledge-base)kullanılabilir.

## <a name="create-knowledge-base-project"></a>Bilgi bankası projesi oluşturma

1. Visual Studio 2019 Community Edition 'ı açın.
1. Yeni bir **konsol uygulaması (.NET Core)** projesi oluşturun ve projeyi `QnaMakerQuickstart`adlandırın. Diğer ayarlar için varsayılan değerleri kabul edin.

## <a name="add-required-dependencies"></a>Gerekli bağımlılıkları ekleme

Program.cs dosyasının en üst kısmındaki tek using deyimini aşağıdaki satırlarla değiştirerek projeye gerekli bağımlılıkları ekleyin:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=1-2 "Add the required dependencies")]

## <a name="add-required-constants"></a>Gerekli sabitleri ekleme

İçinde **ana** yöntemi, soru-cevap Oluşturucu erişmek için gerekli sabitleri ekleyin. Değerleri kendi değerlerinizle değiştirin.

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=11-14 "Add the required constants")]

## <a name="add-post-request-to-publish-knowledge-base"></a>Bilgi Bankası yayımlama için POST isteği Ekle

Sonra gerekli sabitleri, Bilgi Bankası yayımlama için soru-cevap Oluşturucu API'si bir HTTPS isteği yapar ve yanıtı alan aşağıdaki kodu ekleyin:

[!code-csharp[Add HTTP Post request and response](~/samples-qnamaker-csharp/documentation-samples/quickstarts/publish-knowledge-base/QnAMakerPublishQuickstart/Program.cs?range=16-29&dedent=8 "Add HTTP Post request and response")]

Yayımlama başarılı olursa API çağrısı boş yanıt gövdesiyle 204 durumunu döndürür. 
 
## <a name="build-and-run-the-program"></a>Programı derleme ve çalıştırma

Programı derleyin ve çalıştırın. Otomatik olarak Bilgi Bankası yayımlama için soru-cevap Oluşturucu API'si isteği gönderir ve yanıtı konsol penceresinde yazdırılır.

Bilgi bankanız yayımlandıktan sonra istemci uygulaması veya sohbet botu ile uç noktadan sorgulayabilirsiniz. 

[!INCLUDE [Clean up files and knowledge base](../../../../includes/cognitive-services-qnamaker-quickstart-cleanup-resources.md)] 

## <a name="next-steps"></a>Sonraki adımlar

Bilgi Bankası yayımlandıktan sonra ihtiyacınız [yanıt oluşturmak için uç nokta URL'si](../Tutorials/create-publish-answer.md#generating-an-answer). 

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
