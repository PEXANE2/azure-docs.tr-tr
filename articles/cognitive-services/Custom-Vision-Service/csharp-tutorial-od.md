---
title: Hızlı başlangıç Özel Görüntü İşleme SDK 'Sı C# ile bir nesne algılama projesi oluşturma
titleSuffix: Azure Cognitive Services
description: İle C#.NET SDK kullanarak bir proje oluşturun, Etiketler ekleyin, görüntü yükleyin, projenizi eğitin ve nesneleri algılayın.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 12/05/2019
ms.author: areddish
ms.openlocfilehash: c6aaf69ba3ed682a00a203079b024a47121334e3
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76170060"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-net-sdk"></a>Hızlı başlangıç Özel Görüntü İşleme .NET SDK ile bir nesne algılama projesi oluşturma

Bu makalede, bir nesne algılama modeli oluşturmak için ile C# özel görüntü işleme SDK 'sını kullanmaya nasıl başlacağınız gösterilmektedir. Oluşturulduktan sonra etiketli bölgeler ekleyebilir, görüntüleri yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç noktası URL 'sini alabilir ve bir görüntüyü programlı bir şekilde test etmek için uç noktayı kullanabilirsiniz. Bu örneği kendi .NET uygulamanızı oluşturmak için bir şablon olarak kullanın. 

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/) herhangi bir sürümü
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Özel Görüntü İşleme SDK 'sını ve örnek kodu alın

Özel Görüntü İşleme kullanan bir .NET uygulaması yazmak için Özel Görüntü İşleme NuGet paketleri gerekir. Bu paketler, indirileceği örnek projeye dahildir, ancak bunlara ayrı ayrı erişebilirsiniz.

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

[Bilişsel Hizmetler .NET Örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples) projesini kopyalayın veya indirin. **CustomVision/ObjectDetection** klasörüne gidin ve Visual Studio'da _ObjectDetection.csproj'u_ açın.

Bu Visual Studio projesi, [Özel Görüntü İşleme web sitesi](https://customvision.ai/) üzerinden erişilebilen __My New Project__ adlı yeni bir proje oluşturur. Daha sonra nesne algılama modelini eğitip test etmek için görüntüleri karşıya yükler. Bu projede model, görüntülerdeki çatalları ve makasları algılamak için eğitilir.

[!INCLUDE [get-keys](includes/get-keys.md)]

## <a name="understand-the-code"></a>Kodu anlama

_Program.cs_ dosyasını açın ve kodu inceleyin. Sırasıyla ve sırasıyla eğitim `CUSTOM_VISION_TRAINING_KEY` `CUSTOM_VISION_PREDICTION_KEY`ve tahmin anahtarlarınız için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) Komut dosyası bu değişkenleri arayacaktır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_keys)]

Ayrıca, Özel Görüntü İşleme Web sitesinin ayarlar sayfasından uç nokta URL 'nizi alın. `CUSTOM_VISION_ENDPOINT`adlı bir ortam değişkenine kaydedin. Betik, sınıfınızın kökünde buna bir başvuru kaydeder.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_endpoint)]

### <a name="create-a-new-custom-vision-service-project"></a>Yeni bir Özel Görüntü İşleme Hizmeti projesi oluştur

Bu sonraki küçük kod bir nesne algılama projesi oluşturur. Oluşturulan proje, daha önce ziyaret ettiğiniz [Özel Görüntü İşleme web sitesinde](https://customvision.ai/) gösterilir. Projenizi oluştururken diğer seçenekleri belirtmek için [CreateProject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.createproject?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_CreateProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_String_System_String_System_Nullable_System_Guid__System_String_System_Collections_Generic_IList_System_String__) yöntemine bakın [(dedektör](get-started-build-detector.md) web portalı oluştur kılavuzunda açıklanmıştır).  

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_create)]


### <a name="add-tags-to-the-project"></a>Projeye Etiketler ekleyin

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_tags)]

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Nesne algılama projelerinde resimleri etiketlediğinizde, her etiketlenmiş nesnenin bölgesini normalleştirilmiş koordinatları kullanarak belirtmeniz gerekir. Aşağıdaki kod, örnek görüntülerin her birini etiketli bölgesiyle ilişkilendirir.

> [!NOTE]
> Bölgelerin koordinatlarını işaretlemek için tıkla ve sürükley yardımcı programınız yoksa, web Web Arama Sayfasını [Customvision.ai'da](https://www.customvision.ai/)kullanabilirsiniz. Bu örnekte, koordinatlar zaten sağlanmıştır.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload_regions)]

Ardından her örnek görüntüyü bölge koordinatlarıyla karşıya yüklemek için bu ilişki haritası kullanılır. Tek bir toplu iş halinde en fazla 64 resim yükleyebilirsiniz.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_upload)]

Bu noktada, tüm örnek görüntülerini karşıya yüklediniz ve her birini (**çatal** veya **makas**) ilişkili bir piksel dikdörtgeniyle etiketledi.

### <a name="train-the-project"></a>Projeyi eğitme

Bu kod, projedeki ilk eğitim yinelemesini oluşturur.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_train)]

### <a name="publish-the-current-iteration"></a>Geçerli yinelemeyi Yayımla

Yayımlanan yinelemeye verilen ad, tahmin istekleri göndermek için kullanılabilir. Bir yineleme, yayınlanana kadar tahmin uç noktasında kullanılamaz.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_publish)]

### <a name="create-a-prediction-endpoint"></a>Tahmin uç noktası oluşturma

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction_endpoint)]

### <a name="use-the-prediction-endpoint"></a>Tahmin uç noktasını kullanma

Komut dosyasının bu bölümü, test görüntüsünü yükler, model uç noktasını sorgular ve tahmin verilerinin konsola çıktısını verir.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?name=snippet_prediction)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama çalışırken, bir konsol penceresi açıp aşağıdaki çıktıyı yazmalıdır:

```console
Creating new project:
        Training
Done!

Making a prediction:
        fork: 98.2% [ 0.111609578, 0.184719115, 0.6607002, 0.6637112 ]
        scissors: 1.2% [ 0.112389535, 0.119195729, 0.658031344, 0.7023591 ]
```

Daha sonra test görüntüsünün ( **görüntülerde/testte**bulunan) uygun şekilde etiketlendiğini ve algılama bölgesinin doğru olduğunu doğrulayabilirsiniz. Bu noktada uygulamadan çıkmak için herhangi bir tuşa basabilirsiniz.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Şimdi kod nesne algılama sürecinin her adımı yapmak için nasıl gördüm. Bu örnek tek bir eğitim yinelemesini yürütür, ancak genellikle modelinizi daha doğru hale getirmek için birden çok kez eğitmeniz ve test etmeniz gerekir. Sonraki kılavuzda görüntü sınıflandırma konusu üstünde durulur ancak temel ilkeleri nesne algılamaya benzer.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)
