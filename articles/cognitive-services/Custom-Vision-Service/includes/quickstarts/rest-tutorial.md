---
author: PatrickFarley
ms.author: pafarley
ms.service: cognitive-services
ms.date: 12/09/2020
ms.topic: include
ms.openlocfilehash: 8890a0132e5f510a0af2862c483206fd025a68d8
ms.sourcegitcommit: c7153bb48ce003a158e83a1174e1ee7e4b1a5461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2021
ms.locfileid: "98256378"
---
Özel Görüntü İşleme REST API kullanmaya başlayın. API 'YI çağırmak ve bir görüntü sınıflandırma modeli oluşturmak için bu adımları izleyin. Bir proje oluşturacak, Etiketler ekleyecek, projeyi eğtireceksiniz ve projenin tahmin uç nokta URL 'sini programlı bir şekilde test etmek üzere kullanacaksınız. Bu örneği kendi görüntü tanıma uygulamanızı oluşturmak için bir şablon olarak kullanın.

> [!NOTE]
> Özel Görüntü İşleme, bir istemci kitaplığı SDK 'Sı veya [tarayıcı tabanlı kılavuz](../../get-started-build-detector.md)aracılığıyla kolayca kullanılır.

.NET için Özel Görüntü İşleme istemci kitaplığı 'nı kullanın:

* Yeni bir Özel Görüntü İşleme projesi oluşturma
* Projeye Etiketler ekleyin
* Görüntüleri karşıya yükleme ve etiketleme
* Projeyi eğitme
* Geçerli yinelemeyi Yayımla
* Tahmin uç noktasını test etme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/cognitive-services/)
* Azure aboneliğiniz olduktan sonra bir Özel Görüntü İşleme kaynak oluşturun ve bir <a href="https://portal.azure.com/?microsoft_azure_marketplace_ItemHideKey=microsoft_azure_cognitiveservices_customvision#create/Microsoft.CognitiveServicesCustomVision"  title=" "  target="_blank"> <span class="docon docon-navigate-external x-hidden-focus"></span> </a> eğitim ve tahmin kaynağı oluşturmak ve anahtarlarınızı ve uç noktanızı almak için Azure Portal özel görüntü işleme bir kaynak oluşturun. Dağıtım için bekleyin ve **Kaynağa Git** düğmesine tıklayın.
    * Uygulamanızı Özel Görüntü İşleme bağlamak için oluşturduğunuz kaynaklarda anahtar ve uç nokta gerekir. Anahtarınızı ve uç noktanızı daha sonra hızlı başlangıçta aşağıdaki koda yapıştırabilirsiniz.
    * `F0`Hizmeti denemek ve daha sonra üretime yönelik ücretli bir katmana yükseltmek için ücretsiz fiyatlandırma katmanını () kullanabilirsiniz.


## <a name="create-a-new-custom-vision-project"></a>Yeni bir Özel Görüntü İşleme projesi oluşturma

Görüntü sınıflandırması projesi oluşturmak için aşağıdaki gibi bir komut kullanacaksınız. Oluşturulan proje, [özel görüntü işleme web sitesinde](https://customvision.ai/)görünür.


:::code language="shell" source="~/cognitive-services-quickstart-code/curl/custom-vision/image-classifier.sh" ID="createproject":::

Komutu bir metin düzenleyicisine kopyalayın ve aşağıdaki değişiklikleri yapın:

* `{subscription key}`Geçerli yüz abonelik anahtarınızla değiştirin.
* `{endpoint}`Abonelik anahtarınıza karşılık gelen uç noktayla değiştirin.
   [!INCLUDE [subdomains-note](../../../../../includes/cognitive-services-custom-subdomains-note.md)]
* `{name}`Projenizin adıyla değiştirin.
* Project 'in kullanacağı model türünü yapılandırmak için isteğe bağlı olarak diğer URL parametrelerini ayarlayın. Seçenekler için [Creatproject API 'sine](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddeae) bakın.

Aşağıdakine benzer bir JSON yanıtı alacaksınız. `"id"`Projenizin değerini geçici bir konuma kaydedin.

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "description": "string",
  "settings": {
    "domainId": "00000000-0000-0000-0000-000000000000",
    "classificationType": "Multiclass",
    "targetExportPlatforms": [
      "CoreML"
    ],
    "useNegativeSet": true,
    "detectionParameters": "string",
    "imageProcessingSettings": {
      "augmentationMethods": {}
    }
  },
  "created": "string",
  "lastModified": "string",
  "thumbnailUri": "string",
  "drModeEnabled": true,
  "status": "Succeeded"
}
```

## <a name="add-tags-to-the-project"></a>Projeye Etiketler ekleyin

Modelin eğiteolacağı etiketleri tanımlamak için aşağıdaki komutu kullanın.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/custom-vision/image-classifier.sh" ID="createtag":::

* Bir kez daha, kendi anahtarını ve uç nokta URL 'nizi ekleyin.
* `{projectId}`Kendi proje Kimliğinizle değiştirin.
* `{name}`Kullanmak istediğiniz etiketin adıyla değiştirin.

Projenizde kullanmak istediğiniz tüm etiketler için bu işlemi tekrarlayın. Belirtilen örnek görüntüleri kullanıyorsanız, ve etiketlerini ekleyin `"Hemlock"` `"Japanese Cherry"` .

Aşağıdakine benzer bir JSON yanıtı alırsınız. `"id"`Her etiketin değerini geçici bir konuma kaydedin.

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "description": "string",
  "type": "Regular",
  "imageCount": 0
}
```

## <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Sonra, bu proje için örnek görüntüleri indirin. [Örnek görüntüler klasörünün](https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/CustomVision/ImageClassification/Images) içeriğini yerel cihazınıza kaydedin.

> [!NOTE]
> Microsoft garaj projesi olan Trove, eğitim amaçlarıyla görüntü kümeleri toplamanıza ve satın almanıza olanak tanır. Görüntülerinizi topladıktan sonra bunları indirebilir ve ardından Özel Görüntü İşleme projenize her zamanki şekilde aktarabilirsiniz. Daha fazla bilgi edinmek için [Trove sayfasını](https://www.microsoft.com/en-us/ai/trove?activetab=pivot1:primaryr3) ziyaret edin.

Görüntüleri karşıya yüklemek ve etiketleri uygulamak için aşağıdaki komutu kullanın; "Hemlock" görüntüleri için bir kez ve "Japon Çraz" görüntüleri için ayrı olarak. Daha fazla seçenek için [veri API 'Sinden görüntü oluşturma](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddeb5) bölümüne bakın.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/custom-vision/image-classifier.sh" ID="uploadimages":::

* Bir kez daha, kendi anahtarını ve uç nokta URL 'nizi ekleyin.
* `{projectId}`Kendi proje Kimliğinizle değiştirin.
* `{tagArray}`Bir ETIKETININ kimliğiyle değiştirin.
* Ardından, isteğin gövdesini etiketlemek istediğiniz görüntülerin ikili verileriyle doldurun.

## <a name="train-the-project"></a>Projeyi eğitme

Bu yöntem, karşıya yüklediğiniz etiketli görüntülerde modeli TRAZ ve geçerli proje yinelemesi için bir KIMLIK döndürüyor.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/custom-vision/image-classifier.sh" ID="trainproject":::

* Bir kez daha, kendi anahtarını ve uç nokta URL 'nizi ekleyin.
* `{projectId}`Kendi proje Kimliğinizle değiştirin.
* `{tagArray}`Bir ETIKETININ kimliğiyle değiştirin.
* Ardından, isteğin gövdesini etiketlemek istediğiniz görüntülerin ikili verileriyle doldurun.
* İsteğe bağlı olarak diğer URL parametrelerini kullanın. Seçenekler için [Proje API 'Sini eğitme](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc7548b571998fddee1) bölümüne bakın.

> [!TIP]
> Seçili etiketlerle eğitme
>
> İsteğe bağlı olarak, uygulanan etiketlerin yalnızca bir alt kümesini eğitebilirsiniz. Yalnızca belirli etiketlerden yeterince birini uygulamadıysanız bunu yapmak isteyebilirsiniz, ancak bu, başkalarının yeterli olması durumunda. İsteğe bağlı JSON içeriğini isteğiniz gövdesine ekleyin. Diziyi, `"selectedTags"` kullanmak istediğiniz etiketlerin kimlikleriyle doldurun.
> ```json
> {
>   "selectedTags": [
>     "00000000-0000-0000-0000-000000000000"
>   ]
> }
> ```

JSON yanıtı, eğitim projeniz hakkında yineleme KIMLIĞI () de dahil olmak üzere bilgiler içerir `"id"` . Sonraki adım için bu değeri kaydedin.

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "name": "string",
  "status": "string",
  "created": "string",
  "lastModified": "string",
  "trainedAt": "string",
  "projectId": "00000000-0000-0000-0000-000000000000",
  "exportable": true,
  "exportableTo": [
    "CoreML"
  ],
  "domainId": "00000000-0000-0000-0000-000000000000",
  "classificationType": "Multiclass",
  "trainingType": "Regular",
  "reservedBudgetInHours": 0,
  "trainingTimeInMinutes": 0,
  "publishName": "string",
  "originalPublishResourceId": "string"
}
```

## <a name="publish-the-current-iteration"></a>Geçerli yinelemeyi Yayımla

Bu yöntem, modelin geçerli yinelemesini sorgulama için kullanılabilir hale getirir. Döndürülen model adını, tahmin istekleri göndermek için bir başvuru olarak kullanırsınız. 

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/custom-vision/image-classifier.sh" ID="publish":::

* Bir kez daha, kendi anahtarını ve uç nokta URL 'nizi ekleyin.
* `{projectId}`Kendi proje Kimliğinizle değiştirin.
* `{iterationId}`Önceki adımda döndürülen kimlikle değiştirin.
* `{publishedName}`Tahmin modelinize atamak istediğiniz adla değiştirin.
* `{predictionId}`Kendi tahmin kaynak Kimliğinizle değiştirin. Bu dosyayı, **ABONELIK kimliği** olarak listelenen Azure Portal tahmin kaynağınızın **genel bakış** sekmesinde bulabilirsiniz.
* İsteğe bağlı olarak diğer URL parametrelerini kullanın. Bkz. [Yayımlama yineleme](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc7548b571998fdded5) API 'si.

## <a name="test-the-prediction-endpoint"></a>Tahmin uç noktasını test etme

Son olarak, bu komutu kullanarak eğitilen modelinizi Etiketler ile sınıflandırarak yeni bir görüntü yükleyerek test edin. Görüntüyü daha önce indirdiğiniz örnek dosyaların "test" klasöründe kullanabilirsiniz.

:::code language="shell" source="~/cognitive-services-quickstart-code/curl/custom-vision/image-classifier.sh" ID="publish":::

* Bir kez daha, kendi anahtarını ve uç nokta URL 'nizi ekleyin.
* `{projectId}`Kendi proje Kimliğinizle değiştirin.
* `{publishedName}`Önceki adımda kullandığınız adla değiştirin.
* Yerel görüntünüzün ikili verilerini istek gövdesine ekleyin.
* İsteğe bağlı olarak diğer URL parametrelerini kullanın. [Sınıflandırma görüntüsü](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.1/operations/5eb37d24548b571998fde5f3) API 'sine bakın.

Döndürülen JSON yanıtı, her etiket için en az bir modelin resme uyguladığı her bir etikete sahip olur. 

```json
{
  "id": "00000000-0000-0000-0000-000000000000",
  "project": "00000000-0000-0000-0000-000000000000",
  "iteration": "00000000-0000-0000-0000-000000000000",
  "created": "string",
  "predictions": [
    {
      "probability": 0.0,
      "tagId": "00000000-0000-0000-0000-000000000000",
      "tagName": "string",
      "boundingBox": {
        "left": 0.0,
        "top": 0.0,
        "width": 0.0,
        "height": 0.0
      },
      "tagType": "Regular"
    }
  ]
}
```

[!INCLUDE [clean-ic-project](../../includes/clean-ic-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık REST API kullanarak görüntü sınıflandırma işleminin her adımını tamamladınız. Bu örnek tek bir eğitim yinelemesi yürütür, ancak genellikle modelinizi daha doğru hale getirmek için birden çok kez eğitmeniz ve test etmeniz gerekir.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](../../test-your-model.md)

* [Özel Görüntü İşleme nedir?](../../overview.md)
* [API başvuru belgeleri (eğitim)](/dotnet/api/overview/azure/cognitiveservices/client/customvision?view=azure-dotnet)
* [API başvuru belgeleri (tahmin)](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.3/operations/5eb0bcc6548b571998fddeae)
