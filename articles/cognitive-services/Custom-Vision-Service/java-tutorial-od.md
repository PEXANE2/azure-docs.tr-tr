---
title: 'Hızlı Başlangıç: Java için Özel Görüntü İşleme SDK’sı ile nesne algılama projesi oluşturma'
titleSuffix: Azure Cognitive Services
description: Java SDK'sını kullanarak bir proje oluşturun, etiketler ekleyin, görüntüleri karşıya yükleyin, projenizi eğitin ve nesneleri algılayın.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 02/25/2020
ms.author: areddish
ms.openlocfilehash: 78db95240974d1c9ca07546f8237eca2b564ecb2
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77616318"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-sdk-for-java"></a>Hızlı Başlangıç: Java için Özel Görüntü İşleme SDK’sı ile nesne algılama projesi oluşturma

Bu makalede, bir nesne algılama modeli oluşturmak için Java ile Özel Vizyon SDK kullanmaya başlamak için nasıl gösterir. Oluşturulduktan sonra etiketli bölgeler ekleyebilir, görüntüleri yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç noktası URL 'sini alabilir ve bir görüntüyü programlı bir şekilde test etmek için uç noktayı kullanabilirsiniz. Kendi Java uygulamanızı oluştururken bu örneği şablon olarak kullanın.

## <a name="prerequisites"></a>Ön koşullar

- Kendi seçtiğiniz bir Java IDE
- [JDK 7 veya 8](https://aka.ms/azure-jdks) yüklü.
- [Maven](https://maven.apache.org/) yüklü
- [!INCLUDE [create-resources](includes/create-resources.md)]

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>Özel Görüntü İşleme SDK 'sını ve örnek kodu alın

Özel Görüntü İşleme kullanan bir Java uygulaması yazmak için Özel Görüntü İşleme maven paketlerine ihtiyacınız olacaktır. Bu paketler indireceğiniz örnek projeye dahildir, ancak bunlara buradan tek tek erişebilirsiniz.

Özel Vizyon SDK'yı maven merkezi deposunda bulabilirsiniz:
- [Eğitim SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Tahmin SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

[Bilişsel Hizmetler Java SDK'sı Örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master) projesini kopyalayın veya indirin. **Vision/CustomVision/** klasörüne gidin.

Bu Java projesi, [Özel Görüntü İşleme web sitesi](https://customvision.ai/) üzerinden erişilebilen __Sample Java OD Project__ adlı yeni bir Özel Görüntü İşleme nesne algılama projesi oluşturur. Daha sonra bir sınıflandırıcıyı eğitip test etmek için görüntüleri karşıya yükler. Bu projede, sınıflandırıcı bir nesnenin **çatal** veya **makas**olup olmadığını belirlemek için tasarlanmıştır.

[!INCLUDE [get-keys](includes/get-keys.md)]

Program, anahtar verilerinizi ortam değişkenleri olarak referans almak üzere yapılandırılmıştır. **Vision/CustomVision** klasörüne gidin ve ortam değişkenlerini ayarlamak için aşağıdaki PowerShell komutlarını girin. 

> [!NOTE]
> Windows'a uygun olmayan bir işletim sistemi kullanıyorsanız, yönergeler için [ortam değişkenlerini yapılandır'a](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows#configure-an-environment-variable-for-authentication) bakın.

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>Kodu anlama

Java IDE'nize `Vision/CustomVision` projesini yükleyin ve _CustomVisionSamples.java_ dosyasını açın. **runSample** yöntemini bulun ve bu **yöntem,** bu kılavuzda kapsanmayan görüntü sınıflandırma senaryosunu yürütür ImageClassification_Sample yöntemi arayın.&mdash; **ObjectDetection_Sample** yöntemi bu hızlı başlangıcın birincil işlevini gerçekleştirir; yöntemin tanımına gidin ve kodu inceleyin. 

### <a name="create-a-new-custom-vision-service-project"></a>Yeni bir Özel Görüntü İşleme Hizmeti projesi oluştur

Eğitim istemcisi ve nesne algılama projesini oluşturan kod bloğuna gidin. Oluşturulan proje, daha önce ziyaret ettiğiniz [Özel Görüntü İşleme web sitesinde](https://customvision.ai/) gösterilir. Projenizi oluştururken diğer seçenekleri belirtmek için [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_) yönteminin aşırı yüklenir [(dedektör](get-started-build-detector.md) web portalı oluştur kılavuzunda açıklanmıştır).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create_od)]

### <a name="add-tags-to-your-project"></a>Projenize etiketler ekleme

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags_od)]

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Nesne algılama projelerinde resimleri etiketlediğinizde, her etiketlenmiş nesnenin bölgesini normalleştirilmiş koordinatları kullanarak belirtmeniz gerekir. `regionMap` Haritasının tanımına gidin. Bu kod, örnek görüntülerin her birini etiketlendikleri bölgeyle ilişkilendirir.

> [!NOTE]
> Bölgelerin koordinatlarını işaretlemek için tıkla ve sürükley yardımcı programınız yoksa, web Web Arama Sayfasını [Customvision.ai'da](https://www.customvision.ai/)kullanabilirsiniz. Bu örnekte, koordinatlar zaten sağlanmıştır.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_od_mapping)]

Ardından projeye görüntüleri ekleyen kod bloğuna atlayın. Görüntüler projenin **src/main/resources** klasöründen okunur ve uygun etiketleri ve bölge koordinatlarıyla hizmete yüklenir.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload_od)]

Önceki kod snippet kaynak akışları olarak görüntüleri almak ve hizmete yüklemek iki yardımcı işlevleri kullanır (tek bir toplu iş halinde 64 görüntüye kadar yükleyebilirsiniz).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

### <a name="train-the-project-and-publish"></a>Projeyi eğitin ve yayınlayın

Bu kod, tahmin modelinin ilk yinelemesini oluşturur ve sonra bu yinelemeyi tahmin bitiş noktasına yayımlar. Yayımlanan yinelemeye verilen ad, tahmin istekleri göndermek için kullanılabilir. Bir yineleme, yayımlanana kadar tahmin bitiş noktasında kullanılamaz.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train_od)]

### <a name="use-the-prediction-endpoint"></a>Tahmin uç noktasını kullanma

Burada `predictor` nesnesiyle gösterilen tahmin uç noktası, bir görüntüyü geçerli modele göndermek ve sınıflandırma tahmini almak için kullandığınız başvurudur. Bu örnekte `predictor`, tahmin anahtarı ortam değişkeni kullanılarak başka bir yerde tanımlanır.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_prediction_od)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Çözümü maven kullanarak derlemek ve çalıştırmak için, komut isteminde proje dizinine **(Vision/CustomVision)** gidin ve çalıştır komutunu çalıştırın:

```bash
mvn compile exec:java
```

Günlüğe kaydetme ve tahmin sonuçları için konsol çıkışını görüntüleyin. Ardından test görüntüsünün etiketlendiğini ve algılama bölgesinin doğru olduğunu onaylayabilirsiniz.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kodda nesne algılama işleminin her adımının nasıl uygulanabileceğini gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür ama modelinizin daha doğru olmasını sağlamak için çoğunlukla birden çok kez eğitmeniz ve test etmeniz gerekecektir. Sonraki kılavuzda görüntü sınıflandırma konusu üstünde durulur ancak temel ilkeleri nesne algılamaya benzer.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](test-your-model.md)
