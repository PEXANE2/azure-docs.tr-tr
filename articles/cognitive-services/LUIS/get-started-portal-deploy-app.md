---
title: 'Quickstart: LUIS portalı ile bir uygulama dağıtma'
description: Bu hızlı başlangıç, bir tahmin bitiş noktası kaynağı oluşturarak, kaynağı atayarak, uygulamanın nasıl dağıtılsüreceğini ve uygulamayı nasıl yayımlayyaratacağını gösterir.
ms.topic: quickstart
ms.date: 04/06/2020
ms.openlocfilehash: aaf86766c2357c5382b78cd4a35fd4b159e5c0f3
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756294"
---
# <a name="quickstart-deploy-an-app-in-the-luis-portal"></a>Quickstart: LUIS portalında bir uygulama dağıtma

LUIS uygulamanız bir istemci uygulamasına (örneğin, bir sohbet botu) sözcük tahminleri döndürmeye hazır olduğunda, uygulamayı tahmin bitiş noktasına dağıtmanız gerekir.

Bu hızlı başlangıçta, bir uygulamayı dağıtmayı öğrenirsiniz. Bir tahmin bitiş noktası kaynağı oluşturur, kaynağı uygulamaya atar, uygulamayı eğitin ve uygulamayı yayımlarsınız.

## <a name="prerequisites"></a>Ön koşullar

* Azure [aboneliği](https://azure.microsoft.com/free)alın.
* Önceki [portalı hızlı başlatın](get-started-portal-build-app.md) veya [uygulamayı indirip içe aktarın.](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/in-portal/build-portal-app.json)
* Azure kaynak kimlik doğrulaması için tarih veren uygulamalarınız varsa, [bir Azure kaynağına geçiş yapın.](luis-migration-authoring.md) E-posta kimlik doğrulaması geçerli olduğunda bazı portal sayfaları farklı görünür.

<a name="create-the-endpoint-resource"></a>

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

## <a name="assign-the-resource-key-to-the-luis-app-in-the-luis-portal"></a>LUIS portalındaki LUIS uygulamasının kaynak anahtarını atama

LUIS için her yeni bir yazma veya sorgu tahmin kaynağı oluşturduğunuzda, kaynağı LUIS uygulamasına atamanız gerekir. Atandıktan sonra, yeni bir kaynak oluşturmadığınız sürece bu adımı tekrar yapmanız gerekmez. Uygulamanızın bölgelerini genişletmek veya daha fazla sayıda tahmin sorgusunu desteklemek için yeni bir kaynak oluşturabilirsiniz.

1. Önizleme LUIS [portalında](https://preview.luis.ai) oturum açın ve uygulamalar listesinden **myEnglishApp** uygulamasını seçin.

1. Sağ üst menüde **Yönet'i** ve ardından **Azure Kaynakları'nı**seçin.

1. LUIS eklemek için **tahmin kaynağı ekle'yi**seçin.

    ![LUIS tahmin kaynağını eklemek için tahmin kaynağı ekle'yi seçin](./media/get-started-portal-deploy-app/azure-resources-add-prediction-resource.png)

1. Kiracınızı, aboneliğinizi ve kaynak adınızı seçin. **Kaynak Ata'yı**seçin.

   ![Uygulamanız için kaynak atama](./media/get-started-portal-deploy-app/assign-resource.png)

1. Uygulamanızın yazarlık anahtarını eklemek için aynı adımları tamamlayın.

1. Yeni tahmin kaynağı için tablodaki yeni satırı bulun ve bitiş noktası URL'sini kopyalayın. Doğru bir tahmin için LUIS `HTTP GET` API bitiş noktasına bir istek yapmak için inşa edilmiştir.

> [!TIP]
> LUIS uygulamanızı geliştirmek için Etkin öğrenmeyi kullanmayı planlıyorsanız, **sorgu parametrelerini değiştir'i** seçin ve **günlükleri kaydet'i**seçin. Bu eylem, sorgu string `log=true` parametresini ekleyerek örnek URL'yi değiştirir. Çalışma zamanı bitiş noktasına öngörü sorguları yaparken değiştirilen örnek sorgu URL'sini kopyalayın ve kullanın.

## <a name="train-the-app"></a>Uygulamayı eğitme

[!INCLUDE [LUIS How to Train steps](includes/howto-train.md)]

## <a name="publish-the-app-to-the-prediction-endpoint"></a>Uygulamayı tahmin bitiş noktasına yayımlama

[!INCLUDE [LUIS How to Train steps](includes/howto-publish.md)]

## <a name="prediction-endpoint-request"></a>Tahmin bitiş noktası isteği

Önizleme portalında, `query=` URL'nin sonunda kullanıcının söyleyişi GET isteğine eklenir. Sonra `query=`, önceki quickstart sonunda kullanılan aynı kullanıcı söyleyerek girin:

```Is there a form named hrf-234098```

Sorgu dizesinin aşağıdaki çiftleri içerdiğinden emin olun:

* `show-all-intents=true`
* `verbose=true`

Tarayıcı yanıtı gösterir:

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

Test bölmesinde aynı düzeyde bilgi görmek için uygulamayı yayımlamanız gerekir. Uygulama yayınlandıktan sonra, test bölmesinde **yayınlananile karşılaştır'ı** seçin. Önceki adımla aynı JSON'u görmek için yayımlanmış test bölmesinde **JSON'u göster görünümünü** kullanın. Bu şekilde, üzerinde çalıştığınız geçerli uygulamayla yapılan değişiklikleri bitiş noktasına kadar yayınlanmış bir uygulamayla karşılaştırabilirsiniz.

[![Uygulamanın şu anda yayınlanan sürümüyle karşılaştırma](./media/get-started-portal-deploy-app/compare-test-pane.png)](./media/get-started-portal-deploy-app/compare-test-pane.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu hızlı başlatmayı bitirdiğinizde, üstteki gezinme menüsünden **Uygulamalarım'ı** seçin. Listeden uygulamanın onay kutusunu seçin ve ardından listenin üstündeki bağlam araç çubuğundan **Sil'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ortak niyetleri ve varlıkları belirleme](luis-tutorial-prebuilt-intents-entities.md)
