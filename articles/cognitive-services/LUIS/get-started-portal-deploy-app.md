---
title: 'Hızlı Başlangıç: LUI portalı ile uygulama dağıtma'
titleSuffix: Azure Cognitive Services
description: Uygulama, sohbet bot gibi bir istemci uygulamasına söylenişi öngörülerini döndürmeye başladıktan sonra, lusıs uygulamanızı tahmin uç noktasına dağıtmayı öğrenin. Bu hızlı başlangıç, bir tahmin uç noktası kaynağı oluşturarak, uygulamayı uygulamaya atamaya, uygulamayı eğitmek ve uygulamayı yayımlatabarak bir uygulamayı nasıl dağıtacağınızı gösterir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/02/2019
ms.author: diberry
ms.openlocfilehash: 4e9ecdad0fb4d02f160977fa28a484b2a3a5bb30
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70257085"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Hızlı Başlangıç: LUI portalında uygulama dağıtma

Lusıs uygulamanız, bir istemci uygulamasına söylenişi tahminleri (örneğin, bir sohbet bot) döndürmeye hazırsanız uygulamayı tahmin uç noktasına dağıtmanız gerekir.

Bu hızlı başlangıçta, bir uygulamayı dağıtmayı öğreneceksiniz. Bir tahmin uç noktası kaynağı oluşturur, uygulamayı uygulamaya atayabilir, uygulamayı eğitme ve uygulamayı yayımlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure aboneliği](https://azure.microsoft.com/free)alın.
* [Önceki Portal Hızlı](get-started-portal-build-app.md) başlangıcını tamamlayıp [uygulamayı indirip içeri aktarın](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json).

## <a name="create-the-endpoint-resource"></a>Uç nokta kaynağı oluşturma

Tahmin uç noktası kaynağını Azure portal oluşturursunuz. Bu kaynak yalnızca Endpoint tahmin sorguları için kullanılmalıdır. Uygulamada değişiklik yazmak için bu kaynağı kullanmayın.

1. [Azure Portal](https://ms.portal.azure.com/) oturum açın.

1. Sol üst panelde **+** yeşil işaretini seçin. `Cognitive Services` Market 'te arama yapın ve seçin.

1. Aboneliği aşağıdaki ayarlarla yapılandırın:

   |Ayar|Value|Amaç|
   |--|--|--|
   |Name|`my-cognitive-service-resource`|Azure kaynağının adı. Bu adı, kaynağı LUE portalında uygulamaya atarken kullanmanız gerekir.|
   |Subscription|Aboneliğiniz|Hesabınızla ilişkili aboneliklerden birini seçin.|
   |Location|**Batı ABD**|Bu kaynak için Azure bölgesi.|
   |Fiyatlandırma katmanı|**S0**|Bu kaynak için varsayılan fiyatlandırma katmanı.|
   |Resource group|`my-cognitive-service-resource-group`|Tüm bilişsel hizmet kaynaklarınız için yeni bir kaynak grubu oluşturun. Kaynaklarla işiniz bittiğinde, aboneliğinizi temizlemek için kaynak grubunu silebilirsiniz. |
   | | | |

   ![Azure API seçimi](./media/get-started-portal-deploy-app/create-cognitive-services-resource.png)

1. Azure kaynağını oluşturmak için **Oluştur** ' u seçin.

   Sonraki bölümde, bu yeni kaynağı LUU portalındaki bir Luo uygulamasına bağlamayı öğreneceksiniz.

## <a name="assign-the-resource-key-to-the-luis-app-in-the-luis-portal"></a>LUE portalında LUO uygulamasına kaynak anahtarı atama

LUHER yeni kaynak oluşturduğunuzda, kaynağı LUO uygulamasına atamanız gerekir. Atandıktan sonra, yeni bir kaynak oluşturmadığınız takdirde bu adımı tekrar yapmanız gerekmez. Uygulamanızın bölgelerini genişletmek veya daha yüksek sayıda tahmin sorgusu desteklemek için yeni bir kaynak oluşturabilirsiniz.

1. [Luo portalında](https://www.luis.ai) oturum açın ve uygulamalar listesinden **Myenglishapp** uygulamasını seçin.

1. Sağ üst menüdeki **Yönet** ' i seçin ve ardından **Azure kaynakları**' nı seçin.

1. LUO eklemek için **tahmin kaynağı Ekle**' yi seçin.

    <!-- TBD: get screenshot-->

1. Kiracınızı, aboneliğinizi ve kaynak adınızı seçin. Seçin **atama kaynak**.

   ![Uygulamanıza bir kaynak atayın](./media/get-started-portal-deploy-app/assign-resource.png)

1. Tabloya yeni satır bulun ve uç nokta URL'sini kopyalayın. Bir tahmine yönelik olarak lusıs API `HTTP GET` uç noktasına bir istek oluşturmak için doğru bir şekilde oluşturulur.

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama

Uygulamayı test etmeye hazırsanız eğitme. Geçerli eğitilen sürümün sorgu tahmin uç noktası çalışma zamanından istemci uygulamaları için kullanılabilir olmasını istediğinizde uygulamayı yayımlayın.

1. Uygulamanın eğitilmesi durumunda sağ üstteki menüden **eğit** ' i seçin.

1. Üstteki menüden **Yayımla** ' yı seçin. Varsayılan ortam ayarlarını kabul edin ve **Yayımla**' yı seçin.

1. Tarayıcı penceresinin üst kısmında yeşil başarı bildirimi çubuğu göründüğünde, **uç nokta listesine başvurun**' i seçin.

   ![Uygulama bildirim çubuğu tarayıcıda başarıyla yayımlandı](./media/get-started-portal-deploy-app/successfully-published-notification.png)

1. **Anahtarlar ve uç nokta ayarları** sayfasında, atanan kaynakların listesini ve alt kısımdaki karşılık gelen uç nokta URL 'lerini bulun.

1. Yeni kaynak adınızla ilişkili uç nokta URL 'sini seçin. Bu eylem, tahmin uç noktası çalışma zamanına `GET` istek yapmak için doğru şekilde oluşturulmuş bir URL ile bir Web tarayıcısı açar.

1. URL 'nin sonundaki sorgu için kısa ve kullanıcının utterine GET isteğine eklendiği yerdir. `q=` `q=`Sonrasında, önceki hızlı başlangıç noktasında kullanılan aynı kullanıcı adını girin:

    ```Is there a form named hrf-234098```

    Tarayıcı, istemci uygulamanızın alacağı JSON ile aynı yanıtı gösterir:

    ```JSON
    {
    "query": "Is there a form named hrf-234098",
    "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9768753
    },
    "intents": [
        {
        "intent": "FindForm",
        "score": 0.9768753
        },
        {
        "intent": "None",
        "score": 0.0216071066
        }
    ],
    "entities": [
        {
        "entity": "hrf-234098",
        "type": "Human Resources Form Number",
        "startIndex": 22,
        "endIndex": 31
        }
      ]
    }
    ```

    Bu yanıt, önceki öğreticideki varsayılan test bölmesine göre daha fazla bilgi sağlar. Test bölmesinde aynı bilgi düzeyini görmek için uygulamayı yayımlamanız gerekir. Uygulama yayımlandıktan sonra test bölmesinde **Yayımlanla Karşılaştır** ' ı seçin. Önceki adımla aynı JSON 'ı görmek için yayımlanan test bölmesinde **JSON görünümünü göster** ' i kullanın. Bu şekilde, üzerinde çalışmakta olduğunuz geçerli uygulamayı uç noktada yayınlanan bir uygulamayla karşılaştırabilirsiniz.

    [![Şu anda düzenleyen uygulamanın yayımlanmış sürümüne göre karşılaştır](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç ile işiniz bittiğinde, üst gezinti menüsünden **uygulamalarım** ' ı seçin. Listeden uygulamanın onay kutusunu seçin ve sonra listenin üzerindeki bağlam araç çubuğundan **Sil** ' i seçin.

[![Uygulamayı uygulamalarım listesinden sil](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ortak amaçları ve varlıkları tanımla](luis-tutorial-prebuilt-intents-entities.md)
