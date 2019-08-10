---
title: "Hızlı Başlangıç: İçin Özel Görüntü İşleme SDK 'Sı ile bir nesne algılama projesi oluşturunC#"
titleSuffix: Azure Cognitive Services
description: C# ile .NET SDK'sını kullanarak bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yükleyin, projenizi eğitin ve nesneleri algılayın.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: areddish
ms.openlocfilehash: 34b814e854a1576fcf55d14ddc5ac213d8f87070
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945154"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-net-sdk"></a>Hızlı Başlangıç: Özel Görüntü İşleme .NET SDK ile bir nesne algılama projesi oluşturma

Bu makalede, Özel Görüntü İşleme SDK'sını C# ile kullanarak nesne algılama modeli oluşturmaya başlarken size yardımcı olacak bilgiler ve örnek kod sağlanır. Oluşturulduktan sonra etiketlenmiş bölgeler ekleyebilir, görüntüleri karşıya yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç nokta URL’sini alabilir ve bir görüntüyü programlama yoluyla test etmek için uç noktayı kullanabilirsiniz. Kendi .NET uygulamanızı oluştururken bu örneği şablon olarak kullanın. 

## <a name="prerequisites"></a>Önkoşullar

- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Özel Görüntü İşleme SDK’sını ve örnek kodu alma

Özel Görüntü İşleme kullanan bir .NET uygulaması yazmak için Özel Görüntü İşleme NuGet paketlerine ihtiyacınız olacaktır. Bu paketler, indirileceği örnek projeye dahildir, ancak bunlara ayrı ayrı erişebilirsiniz.

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

[Bilişsel Hizmetler .NET Örnekleri](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples) projesini kopyalayın veya indirin. **Customvision/objectdetection** klasörüne gidin ve Visual Studio 'Da _objectdetection. csproj_ ' yı açın.

Bu Visual Studio projesi, [Özel Görüntü İşleme web sitesi](https://customvision.ai/) üzerinden erişilebilen __My New Project__ adlı yeni bir proje oluşturur. Daha sonra nesne algılama modelini eğitip test etmek için görüntüleri karşıya yükler. Bu projede model, görüntülerdeki çatalları ve makasları algılamak için eğitilir.

[!INCLUDE [get-keys](includes/get-keys.md)]

## <a name="understand-the-code"></a>Kodu anlama

_Program.cs_ dosyasını açın ve kodu inceleyin. Abonelik anahtarlarınızı **Main** yöntemindeki uygun tanımlara ekleyin.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=18-27)]

Endpoint parametresi, Özel Görüntü İşleme kaynaklarını içeren Azure Kaynak grubunun içinde oluşturulduğu bölgeye işaret etmelidir. Bu örnekte, Orta Güney ABD bölgeyi kabul ediyoruz ve şunları kullanacaksınız:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=14-14)]

### <a name="create-a-new-custom-vision-service-project"></a>Yeni bir Özel Görüntü İşleme Hizmeti projesi oluşturma

Bu sonraki küçük kod bir nesne algılama projesi oluşturur. Oluşturulan proje, daha önce ziyaret ettiğiniz [Özel Görüntü İşleme web sitesinde](https://customvision.ai/) gösterilir. Projenizi oluştururken diğer seçenekleri belirtmek için bkz. [CreateProject](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.customvision.training.customvisiontrainingclientextensions.createproject?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_CustomVisionTrainingClientExtensions_CreateProject_Microsoft_Azure_CognitiveServices_Vision_CustomVision_Training_ICustomVisionTrainingClient_System_String_System_String_System_Nullable_System_Guid__System_String_System_Collections_Generic_IList_System_String__) yöntemi ( [bir algılayıcı derleme](get-started-build-detector.md) Web portalı kılavuzunda açıklanmıştır).  

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=29-35)]

### <a name="add-tags-to-the-project"></a>Projeye etiketleri ekleme

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=37-39)]

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Nesne algılama projelerinde görüntüleri etiketlediğinizde etiketli her nesnenin bölgesini normalleştirilmiş koordinatları kullanarak belirtmeniz gerekir. Aşağıdaki kod, örnek görüntülerin her birini etiketlendikleri bölgeyle ilişkilendirir.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=41-84)]

Ardından her örnek görüntüyü bölge koordinatlarıyla karşıya yüklemek için bu ilişki haritası kullanılır. Tek bir toplu işte en fazla 64 görüntü yükleyebilirsiniz.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=86-104)]

Bu noktada, tüm örnek görüntüler karşıya yüklenmiştir; her birinin etiketi (**çatal** veya **makas**) ve söz konusu etiket için ilişkili piksel dikdörtgeni vardır.

### <a name="train-the-project"></a>Projeyi eğitme

Bu kod projede ilk eğitim yinelemesini oluşturur.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=106-117)]

### <a name="publish-the-current-iteration"></a>Geçerli yinelemeyi Yayımla

Yayımlanan yinelemeye verilen ad, tahmin istekleri göndermek için kullanılabilir. Bir yineleme, yayımlanana kadar tahmin uç noktasında kullanılamaz.

```csharp
// The iteration is now trained. Publish it to the prediction end point.
var publishedModelName = "treeClassModel";
var predictionResourceId = "<target prediction resource ID>";
trainingApi.PublishIteration(project.Id, iteration.Id, publishedModelName, predictionResourceId);
Console.WriteLine("Done!\n");
```

### <a name="create-a-prediction-endpoint"></a>Tahmin uç noktası oluşturma

```csharp
// Create a prediction endpoint, passing in the obtained prediction key
CustomVisionPredictionClient endpoint = new CustomVisionPredictionClient()
{
        ApiKey = predictionKey,
        Endpoint = SouthCentralUsEndpoint
};
```

### <a name="use-the-prediction-endpoint"></a>Tahmin uç noktasını kullanma

Betiğin bu bölümü test görüntüsünü yükler, model uç noktasını sorgular ve tahmin verilerinin çıkışını konsola gönderir.

```csharp
// Make a prediction against the new project
Console.WriteLine("Making a prediction:");
var imageFile = Path.Combine("Images", "test", "test_image.jpg");
using (var stream = File.OpenRead(imageFile))
{
        var result = endpoint.DetectImage(project.Id, publishedModelName, stream);

        // Loop over each prediction and write out the results
        foreach (var c in result.Predictions)
        {
                Console.WriteLine($"\t{c.TagName}: {c.Probability:P1} [ {c.BoundingBox.Left}, {c.BoundingBox.Top}, {c.BoundingBox.Width}, {c.BoundingBox.Height} ]");
        }
}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama çalışırken bir konsol penceresi açmalı ve aşağıdaki çıkışı yazmalıdır:

```console
Creating new project:
        Training
Done!

Making a prediction:
        fork: 98.2% [ 0.111609578, 0.184719115, 0.6607002, 0.6637112 ]
        scissors: 1.2% [ 0.112389535, 0.119195729, 0.658031344, 0.7023591 ]
```

Ardından test görüntüsünün (**Images/Test/** yolunda bulunur) uygun etiketlendiğini ve algılama bölgesinin doğru olduğunu onaylayabilirsiniz. Bu noktada, uygulamadan çıkmak için herhangi bir tuşa basabilirsiniz.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kodda nesne algılama işleminin her adımının nasıl uygulanabileceğini gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür ama modelinizin daha doğru olmasını sağlamak için çoğunlukla birden çok kez eğitmeniz ve test etmeniz gerekecektir. Sonraki kılavuzda görüntü sınıflandırma konusu üstünde durulur ancak temel ilkeleri nesne algılamaya benzer.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)
