---
author: areddish
ms.custom: devx-track-java
ms.author: areddish
ms.service: cognitive-services
ms.date: 08/17/2020
ms.openlocfilehash: a822ab52024a801f4443a9dd864b4b96ec52d000
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88511338"
---
Bu makalede bir nesne algılama modeli oluşturmak için Java ile Özel Görüntü İşleme istemci kitaplığını kullanmaya nasıl başlacağınız gösterilmektedir. Oluşturulduktan sonra etiketli bölgeler ekleyebilir, görüntüleri yükleyebilir, projeyi eğitebilir, projenin varsayılan tahmin uç noktası URL 'sini alabilir ve bir görüntüyü programlı bir şekilde test etmek için uç noktayı kullanabilirsiniz. Kendi Java uygulamanızı oluştururken bu örneği şablon olarak kullanın.

## <a name="prerequisites"></a>Ön koşullar

- Kendi seçtiğiniz bir Java IDE
- [JDK 7 veya 8](https://aka.ms/azure-jdks) yüklendi.
- [Maven](https://maven.apache.org/) yüklendi
- [!INCLUDE [create-resources](../../includes/create-resources.md)]

## <a name="get-the-custom-vision-client-library-and-sample-code"></a>Özel Görüntü İşleme istemci kitaplığı ve örnek kodu alın

Özel Görüntü İşleme kullanan bir Java uygulaması yazmak için Özel Görüntü İşleme maven paketlerine ihtiyacınız olacaktır. Bu paketler, indirileceği örnek projeye dahildir, ancak bunlara ayrı ayrı erişebilirsiniz.

Özel Görüntü İşleme istemci kitaplığını Maven merkezi deposunda bulabilirsiniz:
- [Eğitim SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
- [Tahmin SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

[Bilişsel Hizmetler Java SDK'sı Örnekleri](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master) projesini kopyalayın veya indirin. **Vision/CustomVision/** klasörüne gidin.

Bu Java projesi, [Özel Görüntü İşleme web sitesi](https://customvision.ai/) üzerinden erişilebilen __Sample Java OD Project__ adlı yeni bir Özel Görüntü İşleme nesne algılama projesi oluşturur. Daha sonra bir sınıflandırıcıyı eğitip test etmek için görüntüleri karşıya yükler. Bu projede, sınıflandırıcının bir nesne **çatalla** mı yoksa **makas**mi olduğunu belirlemesi amaçlanmıştır.

[!INCLUDE [get-keys](../../includes/get-keys.md)]

Program, anahtar verilerinize ortam değişkenleri olarak başvuracak şekilde yapılandırılmıştır. **Vision/customvision** klasörüne gidin ve ortam değişkenlerini ayarlamak Için aşağıdaki PowerShell komutlarını girin. 

> [!NOTE]
> Windows dışı bir işletim sistemi kullanıyorsanız, yönergeler için bkz. [ortam değişkenlerini yapılandırma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account?tabs=multiservice%2Cwindows#configure-an-environment-variable-for-authentication) .

```powershell
$env:AZURE_CUSTOMVISION_TRAINING_API_KEY ="<your training api key>"
$env:AZURE_CUSTOMVISION_PREDICTION_API_KEY ="<your prediction api key>"
```

## <a name="understand-the-code"></a>Kodu anlama

Java IDE'nize `Vision/CustomVision` projesini yükleyin ve _CustomVisionSamples.java_ dosyasını açın. **RunSample** yöntemini bulun ve **ImageClassification_Sample** yöntemi çağırın &mdash; . Bu yöntem, bu kılavuzda kapsanmayan görüntü sınıflandırma senaryosunu yürütür. **ObjectDetection_Sample** yöntemi bu hızlı başlangıcın birincil işlevini gerçekleştirir; yöntemin tanımına gidin ve kodu inceleyin. 

### <a name="create-a-new-custom-vision-service-project"></a>Yeni bir Özel Görüntü İşleme Hizmeti projesi oluştur

Eğitim istemcisi ve nesne algılama projesini oluşturan kod bloğuna gidin. Oluşturulan proje, daha önce ziyaret ettiğiniz [Özel Görüntü İşleme web sitesinde](https://customvision.ai/) gösterilir. Projenizi oluştururken diğer seçenekleri belirtmek için bkz. [CreateProject](https://docs.microsoft.com/java/api/com.microsoft.azure.cognitiveservices.vision.customvision.training.trainings.createproject?view=azure-java-stable#com_microsoft_azure_cognitiveservices_vision_customvision_training_Trainings_createProject_String_CreateProjectOptionalParameter_) yöntemi aşırı yüklemeleri ( [bir algılayıcı derleme](../../get-started-build-detector.md) Web portalı kılavuzunda açıklanmıştır).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_create_od)]

### <a name="add-tags-to-your-project"></a>Projenize etiketler ekleme

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_tags_od)]

### <a name="upload-and-tag-images"></a>Görüntüleri karşıya yükleme ve etiketleme

Nesne algılama projelerinde resimleri etiketlediğinizde, her etiketlenmiş nesnenin bölgesini normalleştirilmiş koordinatları kullanarak belirtmeniz gerekir. `regionMap` Haritasının tanımına gidin. Bu kod, örnek görüntülerin her birini etiketlendikleri bölgeyle ilişkilendirir.

> [!NOTE]
> Bölgelerin koordinatlarını işaretlemek için bir tıklama ve sürükleme yardımcı programına sahip değilseniz, [Customvision.ai](https://www.customvision.ai/)adresindeki Web Kullanıcı arabirimini kullanabilirsiniz. Bu örnekte, koordinatlar zaten sağlanmış.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_od_mapping)]

Ardından projeye görüntüleri ekleyen kod bloğuna atlayın. Görüntüler projenin **src/main/resources** klasöründen okunur ve uygun etiketleri ve bölge koordinatlarıyla hizmete yüklenir.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_upload_od)]

Önceki kod parçacığı, görüntüleri kaynak akışları olarak alan ve hizmete yükleyen iki yardımcı işlevden yararlanmasına neden olur (tek bir toplu işte en fazla 64 görüntü yükleyebilirsiniz).

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_helpers)]

### <a name="train-the-project-and-publish"></a>Projeyi eğitme ve yayımlama

Bu kod, tahmin modelinin ilk yinelemesini oluşturur ve ardından bu yinelemeyi tahmin uç noktasına yayınlar. Yayımlanan yinelemeye verilen ad, tahmin istekleri göndermek için kullanılabilir. Bir yineleme, yayımlanana kadar tahmin uç noktasında kullanılamaz.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_train_od)]

### <a name="use-the-prediction-endpoint"></a>Tahmin uç noktasını kullanma

Burada `predictor` nesnesiyle gösterilen tahmin uç noktası, bir görüntüyü geçerli modele göndermek ve sınıflandırma tahmini almak için kullandığınız başvurudur. Bu örnekte `predictor`, tahmin anahtarı ortam değişkeni kullanılarak başka bir yerde tanımlanır.

[!code-java[](~/cognitive-services-java-sdk-samples/Vision/CustomVision/src/main/java/com/microsoft/azure/cognitiveservices/vision/customvision/samples/CustomVisionSamples.java?name=snippet_prediction_od)]

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Maven kullanarak çözümü derlemek ve çalıştırmak için, bir komut isteminde proje dizinine (**Vision/customvision**) gidin ve Çalıştır komutunu yürütün:

```bash
mvn compile exec:java
```

Günlüğe kaydetme ve tahmin sonuçları için konsol çıkışını görüntüleyin. Ardından test görüntüsünün etiketlendiğini ve algılama bölgesinin doğru olduğunu onaylayabilirsiniz.

[!INCLUDE [clean-od-project](../../includes/clean-od-project.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık, nesne algılama işleminin her adımının kodda nasıl yapılabileceğini gördünüz. Bu örnek tek bir eğitim yinelemesi yürütür, ancak genellikle modelinizi daha doğru hale getirmek için birden çok kez eğitmeniz ve test etmeniz gerekir. Aşağıdaki eğitim kılavuzu, görüntü sınıflandırmasıyla ilgilidir, ancak ilkeleri nesne algılamasına benzer.

> [!div class="nextstepaction"]
> [Modeli test etme ve yeniden eğitme](../../test-your-model.md)
