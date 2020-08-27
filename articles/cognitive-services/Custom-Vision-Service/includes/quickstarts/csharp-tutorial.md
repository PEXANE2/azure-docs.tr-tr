---
author: PatrickFarley
ms.author: pafarley
ms.service: cognitive-services
ms.date: 08/17/2020
ms.custom: devx-track-csharp
ms.openlocfilehash: ba09deda155ac446693a7e1037390a3f1fd2700f
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88919395"
---
Bu makalede, bir görüntü sınıflandırma modeli oluşturmak Için C# ile Özel Görüntü İşleme istemci kitaplığını kullanmaya başlamanıza yardımcı olacak bilgiler ve örnek kod sağlanmaktadır. Oluşturulduktan sonra etiketler ekleyebilir, görüntüleri karşıya yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç nokta URL’sini alabilir ve bir görüntüyü programlama yoluyla test etmek için uç noktayı kullanabilirsiniz. Bu örneği kendi .NET uygulamanızı oluşturmak için bir şablon olarak kullanın. Kod _olmadan_ bir sınıflandırma modeli oluşturma ve kullanma sürecini yapmak istiyorsanız, bunun yerine [tarayıcı tabanlı kılavuza](../../getting-started-build-a-classifier.md) bakın.

## <a name="prerequisites"></a>Önkoşullar

- Herhangi bir [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/) sürümü
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="get-the-custom-vision-client-library-and-sample-code"></a>Özel Görüntü İşleme istemci kitaplığı ve örnek kodu alın

Özel Görüntü İşleme kullanan bir .NET uygulaması yazmak için Özel Görüntü İşleme NuGet paketleri gerekir. Bu paketler, indirileceği örnek projeye dahildir, ancak bunlara ayrı ayrı erişebilirsiniz.

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

[Bilişsel Hizmetler .NET Örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples) projesini kopyalayın veya indirin. Visual Studio'da **CustomVision/ImageClassification** klasörüne gidin ve _ImageClassification.csproj_ dosyasını açın.

Bu Visual Studio projesi, [Özel Görüntü İşleme web sitesi](https://customvision.ai/) üzerinden erişilebilen __My New Project__ adlı yeni bir proje oluşturur. Daha sonra bir sınıflandırıcıyı eğitip test etmek için görüntüleri karşıya yükler. Bu projede sınıflandırıcının, bir ağacın __Köknar__ mı yoksa __Japon Kirazı__ mı olduğunu belirlemesi hedeflenmiştir.

[!INCLUDE [get-keys](../../includes/get-keys.md)]

## <a name="understand-the-code"></a>Kodu anlama

_Program.cs_ dosyasını açın ve kodu inceleyin. Eğitim ve tahmin anahtarlarınız için sırasıyla ve olarak adlı [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) `CUSTOM_VISION_TRAINING_KEY` `CUSTOM_VISION_PREDICTION_KEY` . Betik bu değişkenlere bakar.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_keys)]

Ayrıca, Özel Görüntü İşleme Web sitesinin ayarlar sayfasından uç nokta URL 'nizi alın. `CUSTOM_VISION_ENDPOINT`adlı bir ortam değişkenine kaydedin. Betik, sınıfınızın kökünde buna bir başvuru kaydeder.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_endpoint)]

Aşağıdaki kod satırları projenin birincil işlevini yürütür.

### <a name="create-a-new-custom-vision-service-project"></a>Yeni bir Özel Görüntü İşleme hizmeti projesi oluşturma

Oluşturulan proje, daha önce ziyaret ettiğiniz [Özel Görüntü İşleme web sitesinde](https://customvision.ai/) gösterilir. Projenizi oluştururken diğer seçenekleri belirtmek için [CreateProject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.createproject?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_CreateProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_String_System_String_System_Nullable_System_Guid__System_String_System_Collections_Generic_IList_System_String__) yöntemine bakın (sınıflandırıcı Web portalı [oluşturma](../../getting-started-build-a-classifier.md) kılavuzunda açıklanmıştır).   

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_create)]

### <a name="create-tags-in-the-project"></a>Projede etiketler oluşturma

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_tags)]

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Bu projenin görüntüleri dahil edilmiştir. Bunlara, _Program.cs_'de **LoadImagesFromDisk** yönteminde başvurulur. Tek bir toplu işte en fazla 64 görüntü yükleyebilirsiniz.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_upload)]

### <a name="train-the-classifier-and-publish"></a>Sınıflandırıcıyı eğitme ve yayımlama

Bu kod, tahmin modelinin ilk yinelemesini oluşturur ve ardından bu yinelemeyi tahmin uç noktasına yayınlar. Tahmin istekleri göndermek için yinelemenin adını kullanabilirsiniz. Bir yineleme, yayınlanana kadar tahmin uç noktasında kullanılamaz.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_train)]

### <a name="set-the-prediction-endpoint"></a>Tahmin uç noktasını ayarlama

Tahmin uç noktası, bir görüntüyü geçerli modele göndermek ve sınıflandırma tahmini almak için kullanabileceğiniz başvurudur.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_prediction_endpoint)]

### <a name="submit-an-image-to-the-default-prediction-endpoint"></a>Varsayılan tahmin uç noktasına bir görüntü gönderme

Bu betikte, test görüntüsü **LoadImagesFromDisk** yönteminde yüklenir ve modelin tahmin çıkışı konsolda görüntülenir. Değişkenin değeri, `publishedModelName` özel görüntü işleme portalının **performans** sekmesinde bulunan "Yayınlanan as" değerine karşılık gelmelidir. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?name=snippet_prediction)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama çalışırken, bir konsol penceresi açıp aşağıdaki çıktıyı yazmalıdır:

```console
Creating new project:
        Uploading images
        Training
Done!

Making a prediction:
        Hemlock: 95.0%
        Japanese Cherry: 0.0%
```

Ardından test görüntüsünün (**Images/Test/** yolunda bulunur) düzgün etiketlendiğini doğrulayabilirsiniz. Uygulamadan çıkmak için herhangi bir tuşa basın. Ayrıca [Özel Görüntü İşleme web sitesine](https://customvision.ai) geri dönebilir ve yeni oluşturulan projenizin geçerli durumunu görebilirsiniz.

[!INCLUDE [clean-ic-project](../../includes/clean-ic-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Şimdi kodda görüntü sınıflandırma işleminin her adımını nasıl yapacağınızı gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür ama modelinizin daha doğru olmasını sağlamak için çoğunlukla birden çok kez eğitmeniz ve test etmeniz gerekecektir.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](../../test-your-model.md)
