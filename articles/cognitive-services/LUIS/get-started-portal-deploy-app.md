---
title: 'Hızlı başlangıç: LUO portalı ile uygulama dağıtma'
titleSuffix: Azure Cognitive Services
description: Uygulama, sohbet bot gibi bir istemci uygulamasına söylenişi öngörülerini döndürmeye başladıktan sonra, lusıs uygulamanızı tahmin uç noktasına dağıtmayı öğrenin. Bu hızlı başlangıç, bir tahmin uç noktası kaynağı oluşturarak, uygulamayı uygulamaya atamaya, uygulamayı eğitmek ve uygulamayı yayımlatabarak bir uygulamayı nasıl dağıtacağınızı gösterir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 10/17/2019
ms.author: diberry
ms.openlocfilehash: ecae5c7db02436fe34fec19989f174504fd1e03a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488720"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Hızlı başlangıç: Lua portalında uygulama dağıtma

[!INCLUDE [Waiting for LUIS portal refresh](./includes/wait-v3-upgrade.md)]


Lusıs uygulamanız, bir istemci uygulamasına söylenişi tahminleri (örneğin, bir sohbet bot) döndürmeye hazırsanız uygulamayı tahmin uç noktasına dağıtmanız gerekir.

Bu hızlı başlangıçta, bir uygulamayı dağıtmayı öğreneceksiniz. Bir tahmin uç noktası kaynağı oluşturur, uygulamayı uygulamaya atayabilir, uygulamayı eğitme ve uygulamayı yayımlayabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

* Bir [Azure aboneliği](https://azure.microsoft.com/free)alın.
* [Önceki Portal Hızlı](get-started-portal-build-app.md) başlangıcını tamamlayıp [uygulamayı indirip içeri aktarın](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json).

## <a name="create-the-endpoint-resource"></a>Uç nokta kaynağı oluşturma

Tahmin uç noktası kaynağını Azure portal oluşturursunuz. Bu kaynak yalnızca Endpoint tahmin sorguları için kullanılmalıdır. Uygulamada değişiklik yazmak için bu kaynağı kullanmayın.

1. [Azure portalında](https://ms.portal.azure.com/) oturum açın.

1. Sol üst panelde yeşil **+** oturum açma seçeneğini belirleyin. Market 'te `Cognitive Services` arayın ve seçin.

1. Aboneliği aşağıdaki ayarlarla yapılandırın:

   |Ayar|Değer|Amaç|
   |--|--|--|
   |Ad|`my-cognitive-service-resource`|Azure kaynağının adı. Bu adı, kaynağı LUE portalında uygulamaya atarken kullanmanız gerekir.|
   |Abonelik|Aboneliğiniz|Hesabınızla ilişkili aboneliklerden birini seçin.|
   |Konum|**Batı ABD**|Bu kaynak için Azure bölgesi.|
   |Fiyatlandırma katmanı|**S0**|Bu kaynak için varsayılan fiyatlandırma katmanı.|
   |Kaynak grubu|`my-cognitive-service-resource-group`|Tüm bilişsel hizmet kaynaklarınız için yeni bir kaynak grubu oluşturun. Kaynaklarla işiniz bittiğinde, aboneliğinizi temizlemek için kaynak grubunu silebilirsiniz. |
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

1. Kiracınızı, aboneliğinizi ve kaynak adınızı seçin. **Kaynak ata**' yı seçin.

   ![Uygulamanıza bir kaynak atayın](./media/get-started-portal-deploy-app/assign-resource.png)

1. Tablodaki yeni satırı bulun ve uç nokta URL 'sini kopyalayın. Bir tahmine yönelik olarak LUSıS API uç noktası için `HTTP GET` isteği oluşturmak üzere doğru bir şekilde oluşturulur.

## <a name="train-and-publish-the-app"></a>Uygulamayı eğitme ve yayımlama

Uygulamayı test etmeye hazırsanız eğitme. Geçerli eğitilen sürümün sorgu tahmin uç noktası çalışma zamanından istemci uygulamaları için kullanılabilir olmasını istediğinizde uygulamayı yayımlayın.

1. Uygulamanın eğitilmesi durumunda sağ üstteki menüden **eğit** ' i seçin.

1. Üstteki menüden **Yayımla** ' yı seçin. Üretim yuvasını ve Yayımla ' yı seçin.

1. Bildirim çubuğu göründüğünde yayımlama tamamlanmıştır.

1. Bölümün **Azure kaynaklarını** Yönet sayfasında, atanan kaynakların ve karşılık gelen uç nokta URL 'lerinin listesini bulun.

1. Örnek sorguyu bir tarayıcı penceresine kopyalayın ve Kullanıcı utinizi `query` parametresi olarak ekleyin.

## <a name="prediction-endpoint-request"></a>Tahmin uç noktası isteği

URL 'nin sonundaki `query=` **sorgu** için kısaysa ve kullanıcının UTTERINE GET isteğine eklendiği yerdir. `query=`sonra, önceki hızlı başlangıç için kullanılan Kullanıcı utterliğini girin:

```Is there a form named hrf-234098```

Sorgu dizesinin aşağıdaki çiftleri içerdiğinden emin olun:

* `show-all-intents=true`
* `verbose=true`

Tarayıcı, yanıtı gösterir:

```JSON
{
    "query": "Is there a form named hrf-234098",
    "prediction": {
        "topIntent": "FindForm",
        "intents": {
            "FindForm": {
                "score": 0.9768753
            },
            "None": {
                "score": 0.0216071177
            }
        },
        "entities": {
            "Human Resources Form Number": [
                "hrf-234098"
            ],
            "$instance": {
                "Human Resources Form Number": [
                    {
                        "type": "Human Resources Form Number",
                        "text": "hrf-234098",
                        "startIndex": 22,
                        "length": 10,
                        "modelTypeId": 8,
                        "modelType": "Regex Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    }
                ]
            }
        }
    }
}
```

Test bölmesinde aynı bilgi düzeyini görmek için uygulamayı yayımlamanız gerekir. Uygulama yayımlandıktan sonra test bölmesinde **Yayımlanla Karşılaştır** ' ı seçin. Önceki adımla aynı JSON 'ı görmek için yayımlanan test bölmesinde **JSON görünümünü göster** ' i kullanın. Bu şekilde, üzerinde çalışmakta olduğunuz geçerli uygulamayı uç noktada yayınlanan bir uygulamayla karşılaştırabilirsiniz.

[![Şu anda düzenleyen uygulamanın yayımlanmış sürümü karşılaştırması](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlangıç ile işiniz bittiğinde, üst gezinti menüsünden **uygulamalarım** ' ı seçin. Listeden uygulamanın onay kutusunu seçin ve sonra listenin üzerindeki bağlam araç çubuğundan **Sil** ' i seçin.

[Uygulama ![uygulamalarımın listesinden sil](./media/get-started-portal-build-app/delete-app.png)](./media/get-started-portal-build-app/delete-app.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ortak amaçları ve varlıkları tanımla](luis-tutorial-prebuilt-intents-entities.md)
