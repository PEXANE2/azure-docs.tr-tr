---
title: 'Öğretici: Android SDK ile görüntüdeki yüzleri algılama ve çerçeveleme'
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, görüntüdeki yüzleri algılamak ve çerçevelemek için Face hizmetini kullanan basit bir Android uygulaması oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: dd986a7557f468b939aefe1da825c9834618047f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81402911"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Öğretici: Resimdeki yüzleri algılamak ve çerçeve içine almak için Android uygulaması oluşturma

Bu eğitimde, görüntüdeki insan yüzlerini algılamak için Java SDK aracılığıyla Azure Yüz hizmetini kullanan bir Android uygulaması oluşturursunuz. Uygulama seçili bir görüntü görüntüler ve algılanan her yüzün etrafına bir çerçeve çizer.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> - Android uygulaması oluşturma
> - Yüz istemci kitaplığını yükleme
> - İstemci kitaplığını kullanarak resimdeki yüzleri algılama
> - Algılanan her yüzün çevresine bir çerçeve çizme

![Yüzleri kırmızı dikdörtgenle çerçeve içine alınmış bir fotoğrafın Android ekran görüntüsü](../Images/android_getstarted2.1.PNG)

Tam örnek kodu GitHub'daki [Bilişsel Hizmetler Yüz Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample) deposunda mevcuttur.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

- Yüz abonelik anahtarı. [Bilişsel Hizmetleri Deneyin](https://azure.microsoft.com/try/cognitive-services/?api=face-api)ücretsiz deneme abonelik anahtarı alabilirsiniz. Veya Face hizmetine abone olmak ve anahtarınızı almak için [Bilişsel Hizmetler Oluştur hesabındaki](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) yönergeleri izleyin. Ardından, sırasıyla ve sırasıyla anahtar `FACE_SUBSCRIPTION_KEY` ve `FACE_ENDPOINT`hizmet bitiş noktası dizesi için ortam [değişkenleri oluşturun.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication)
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.
- API düzeyi 22 veya daha sonra android [studio.](https://developer.android.com/studio/)

## <a name="create-the-android-studio-project"></a>Android Studio projesini oluşturun

Yeni bir Android uygulama projesi oluşturmak için aşağıdaki adımları izleyin.

1. Android Studio'da **yeni bir Android Studio projesi başlat'ı**seçin.
1. **Android Projesi Oluştur** ekranında, gerekirse varsayılan alanları değiştirin ve sonra da **İleri**'ye tıklayın.
1. Hedef **Android Cihazlar** ekranında, **API 22** veya daha sonra sını seçmek için açılır bırakma seçicisini kullanın ve **ardından İleri'yi**tıklatın.
1. **Boş Etkinlik** öğesini seçin ve **İleri**'ye tıklayın.
1. **Geriye Dönük Uyumluluk** öğesinin işaretini kaldırın ve **Son**'a tıklayın.

## <a name="add-the-initial-code"></a>Başlangıç kodunu ekleme

### <a name="create-the-ui"></a>UI oluşturma

*Açık activity_main.xml*. Düzen Düzenleyicisi'nde **Metin** sekmesini seçin ve ardından içindekileri aşağıdaki kodla değiştirin.

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/res/layout/activity_main.xml?name=snippet_activitymain)]

### <a name="create-the-main-class"></a>Ana sınıfı oluşturma

*MainActivity.java'yı* açın `import` ve varolan ifadeleri aşağıdaki kodla değiştirin.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_imports)]

Ardından, **MainActivity** sınıfının içeriğini aşağıdaki kodla değiştirin. Bu, **düğmede** kullanıcının resim seçmesine izin vermek için yeni bir etkinlik başlatan bir olay işleyicisi oluşturur. **Resmi ImageView'da**görüntüler.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_mainactivity_methods)]

### <a name="try-the-app"></a>Uygulamayı deneyin

**OnActivityResult** yönteminde **DetectAndFrame** için çağrıyı yorumla. Ardından uygulamanızı test etmek için menüde **Çalıştır'a** basın. Uygulama açıldığında, bir emülatörde veya bağlı bir cihazda, alttaki **Gözat'ı** tıklatın. Aygıtın dosya seçimi iletişim kutusu görünmelidir. Bir resim seçin ve pencerede görüntüleninip görüntülemeyi doğrulayın. Ardından uygulamayı kapatın ve bir sonraki adıma geçin.

![Yüzler içeren bir fotoğrafın Android ekran görüntüsü](../Images/android_getstarted1.1.PNG)

## <a name="add-the-face-sdk"></a>Yüz SDK ekle

### <a name="add-the-gradle-dependency"></a>Gradle bağımlılığını ekleme

**Proje** bölmesinde, aşağı açılan seçiciyi kullanarak **Android**'i seçin. **Gradle Scripts**'i genişletin, sonra *build.gradle (Modül: uygulama)* öğesini açın. Aşağıdaki ekran görüntüsünde gösterildiği gibi Yüz Tanıma istemci kitaplığı `com.microsoft.projectoxford:face:1.4.3` için bir bağımlılık ekleyin, sonra da **Şimdi Eşitle**'ye tıklayın.

![Uygulama build.gradle dosyasının Android Studio ekran görüntüsü](../Images/face-tut-java-gradle.png)

### <a name="add-the-face-related-project-code"></a>Yüzle ilgili proje kodunu ekleme

**MainActivity.java'ya** geri dön `import` ve aşağıdaki ifadeleri ekleyin:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_face_imports)]

Ardından, **onCreate** yönteminin üzerine **MainActivity** sınıfına aşağıdaki kodu ekleyin:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_mainactivity_fields)]

**Proje** bölmesinde **uygulamayı**, sonra da **bildirimleri** genişletin ve *AndroidManifest.xml* dosyasını açın. Aşağıdaki öğeyi `manifest` öğesinin doğrudan alt öğesi olarak ekleyin:

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/AndroidManifest.xml?name=snippet_manifest_entry)]

## <a name="upload-image-and-detect-faces"></a>Görüntü yükleme ve yüzleri algılama

Uygulamanız **faceClient.Face.DetectWithStreamAsync** yöntemini arayarak yüzleri algılar, bu yöntem [REST](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API'yi algılar ve **Yüz** örneklerinin bir listesini döndürür.

Döndürülen her **Yüz,** bir dizi isteğe bağlı yüz öznitelikleriyle birlikte konumunu belirtmek için bir dikdörtgen içerir. Bu örnekte, yalnızca yüz dikdörtgenleri istenir.

**MainActivity** sınıfına aşağıdaki iki yöntemi ekleyin. Yüz algılama tamamlandığında, uygulama **ImageView**değiştirmek için **drawFaceRectanglesOnBitmap** yöntemi çağırır. Bundan sonra bu yöntemi tanımlarsınız.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_detection_methods)]

## <a name="draw-face-rectangles"></a>Yüz dikdörtgenleri çizin

**MainActivity** sınıfına aşağıdaki yardımcı yöntemi ni ekleyin. Bu yöntem, her **Yüz** örneğinin dikdörtgen koordinatlarını kullanarak algılanan her yüzün etrafına bir dikdörtgen çizer.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_drawrectangles)]

Son olarak, **onActivityResult'daki** **detectAndFrame** yöntemine yapılan çağrıyı açıklamamak.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın ve içinde yüzlerin yer aldığı bir resim bulun. Yüz Tanıma hizmetinin yanıt vermesi için birkaç saniye bekleyin. Görüntüdeki yüzlerin her birinde kırmızı bir dikdörtgen görmeniz gerekir.

![Etraflarında kırmızı dikdörtgenler çizilmiş yüzlerin Android ekran görüntüsü](../Images/android_getstarted2.1.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, görüntüdeki yüzleri algılamak ve çerçevelemek için bir uygulama oluşturmak için Face Java SDK'yı kullanmayı öğrendiniz. Ardından, yüz algılama ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Resimdeki Yüzleri Tanıma](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
