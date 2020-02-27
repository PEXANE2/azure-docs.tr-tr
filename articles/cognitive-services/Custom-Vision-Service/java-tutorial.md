---
title: 'Hızlı Başlangıç: Java için Özel Görüntü İşleme SDK’sı ile görüntü sınıflandırma projesi oluşturma'
titleSuffix: Azure Cognitive Services
description: Java SDK'sını kullanarak bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yükleyin, projenizi eğitin ve bir tahminde bulunun.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 02/25/2020
ms.author: areddish
ms.openlocfilehash: a0cfa49dfe247a71e5c2d546e2bb7e9d9b2efc18
ms.sourcegitcommit: f15f548aaead27b76f64d73224e8f6a1a0fc2262
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2020
ms.locfileid: "77616338"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-sdk-for-java"></a>Hızlı Başlangıç: Java için Özel Görüntü İşleme SDK’sı ile görüntü sınıflandırma projesi oluşturma

Bu makalede, bir görüntü sınıflandırma modeli oluşturmak için Özel Görüntü İşleme Java SDK 'sını kullanmaya nasıl başlacağınız gösterilmektedir. Oluşturulduktan sonra etiketler ekleyebilir, görüntüleri karşıya yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç nokta URL’sini alabilir ve bir görüntüyü programlama yoluyla test etmek için uç noktayı kullanabilirsiniz. Kendi Java uygulamanızı oluştururken bu örneği şablon olarak kullanın. Kod _içermeyen_ bir sınıflandırma modeli oluşturma ve kullama işlemi yapmak istiyorsanız, [tarayıcı tabanlı kılavuz](getting-started-build-a-classifier.md) konusuna bakın.

## <a name="prerequisites"></a>Önkoşullar

- Kendi seçtiğiniz bir Java IDE
- [JDK 7 veya 8](https://aka.ms/azure-jdks) yüklenmiş olmalıdır.
- [Maven](https://maven.apache.org/) yüklendi
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Özel Görüntü İşleme SDK’sını ve örnek kodu alma

Özel Görüntü İşleme kullanan bir Java uygulaması yazmak için Özel Görüntü İşleme maven paketlerine ihtiyacınız olacaktır. Bu paketler, indirileceği örnek projeye dahildir, ancak bunlara ayrı ayrı erişebilirsiniz.

Özel Görüntü İşleme SDK 'sını Maven merkezi deposunda bulabilirsiniz:

- [Eğitim SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Tahmin SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

[Bilişsel Hizmetler Java SDK'sı Örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master) projesini kopyalayın veya indirin. **Vision/CustomVision/** klasörüne gidin.

Bu Java projesi, __Özel Görüntü İşleme web sitesi__ üzerinden erişilebilen [Sample Java Project](https://customvision.ai/) adlı yeni bir Özel Görüntü İşleme görüntü sınıflandırma projesi oluşturur. Daha sonra bir sınıflandırıcıyı eğitip test etmek için görüntüleri karşıya yükler. Bu projede sınıflandırıcının, bir ağacın __Köknar__ mı yoksa __Japon Kirazı__ mı olduğunu belirlemesi hedeflenmiştir.

[!INCLUDE [get-keys](includes/get-keys.md)]

Program, anahtar verilerinize ortam değişkenleri olarak başvuracak şekilde yapılandırılmıştır. **Vision/customvision** klasörüne gidin ve ortam değişkenlerini ayarlamak Için aşağıdaki PowerShell komutlarını girin. 

> [!NOTE]
> Windows dışı bir işletim sistemi kullanıyorsanız, yönergeler için bkz. [ortam değişkenlerini yapılandırma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows#configure-an-environment-variable-for-authentication) .

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>Kodu anlama

Java IDE'nize `Vision/CustomVision` projesini yükleyin ve _CustomVisionSamples.java_ dosyasını açın. **RunSample** metodunu bulun ve **ObjectDetection_Sample** yöntem çağrısını not edin&mdash;bu yöntem, bu kılavuzda kapsanmayan nesne algılama senaryosunu yürütür. **ImageClassification_Sample** yöntemi bu örneğin birincil işlevini gerçekleştirir; yöntemin tanımına gidin ve kodu inceleyin.

### <a name="create-a-custom-vision-service-project"></a>Özel Görüntü İşleme Hizmeti projesi oluşturma

Bu ilk kod parçası bir görüntü sınıflandırma projesi oluşturur. Oluşturulan proje, daha önce ziyaret ettiğiniz [Özel Görüntü İşleme web sitesinde](https://customvision.ai/) gösterilir. Projenizi oluştururken diğer seçenekleri belirtmek için bkz. [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_) yöntemi aşırı yüklemeleri (sınıflandırıcı Web portalı [oluşturma](getting-started-build-a-classifier.md) kılavuzunda açıklanmıştır).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create)]

### <a name="create-tags-in-the-project"></a>Projede etiketler oluşturma

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags)]

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Örnek görüntüler, projenin **src/main/resources** klasörüne eklenmiştir. Bu görüntüler söz konusu konumdan okunur ve uygun etiketleriyle birlikte hizmete yüklenir.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload)]

Önceki kod parçacığı, görüntüleri kaynak akışları olarak alan ve hizmete yükleyen iki yardımcı işlevden yararlanmasına neden olur (tek bir toplu işte en fazla 64 görüntü yükleyebilirsiniz).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

### <a name="train-the-classifier-and-publish"></a>Sınıflandırıcıyı eğitme ve yayımlama

Bu kod, tahmin modelinin ilk yinelemesini oluşturur ve ardından bu yinelemeyi tahmin uç noktasına yayınlar. Yayımlanan yinelemeye verilen ad, tahmin istekleri göndermek için kullanılabilir. Bir yineleme, yayımlanana kadar tahmin uç noktasında kullanılamaz.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train)]

### <a name="use-the-prediction-endpoint"></a>Tahmin uç noktasını kullanma

Burada `predictor` nesnesiyle gösterilen tahmin uç noktası, bir görüntüyü geçerli modele göndermek ve sınıflandırma tahmini almak için kullandığınız başvurudur. Bu örnekte `predictor`, tahmin anahtarı ortam değişkeni kullanılarak başka bir yerde tanımlanır.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_predict)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Maven kullanarak çözümü derlemek ve çalıştırmak için, bir komut isteminde proje dizinine (**Vision/customvision**) gidin ve Çalıştır komutunu yürütün:

```bash
mvn compile exec:java
```

Uygulamanın konsol çıkışı aşağıdaki metne benzer olmalıdır:

```console
Creating project...
Adding images...
Adding image: hemlock_1.jpg
Adding image: hemlock_2.jpg
Adding image: hemlock_3.jpg
Adding image: hemlock_4.jpg
Adding image: hemlock_5.jpg
Adding image: hemlock_6.jpg
Adding image: hemlock_7.jpg
Adding image: hemlock_8.jpg
Adding image: hemlock_9.jpg
Adding image: hemlock_10.jpg
Adding image: japanese_cherry_1.jpg
Adding image: japanese_cherry_2.jpg
Adding image: japanese_cherry_3.jpg
Adding image: japanese_cherry_4.jpg
Adding image: japanese_cherry_5.jpg
Adding image: japanese_cherry_6.jpg
Adding image: japanese_cherry_7.jpg
Adding image: japanese_cherry_8.jpg
Adding image: japanese_cherry_9.jpg
Adding image: japanese_cherry_10.jpg
Training...
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

Bundan sonra test görüntüsü tahmininin (çıkışın son birkaç satırı) doğru olduğunu onaylayabilirsiniz.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kodda görüntü sınıflandırma işleminin her adımının nasıl uygulanabileceğini gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür ama modelinizin daha doğru olmasını sağlamak için çoğunlukla birden çok kez eğitmeniz ve test etmeniz gerekecektir.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)
