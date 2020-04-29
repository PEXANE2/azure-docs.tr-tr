---
title: 'Öğretici: Android SDK ile görüntüdeki yüzleri algılama ve çerçeveleme'
titleSuffix: Azure Cognitive Services
description: Bu öğreticide, bir görüntüdeki yüzeyleri algılamak ve çerçeveye eklemek için yüz hizmetini kullanan basit bir Android uygulaması oluşturacaksınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 04/14/2020
ms.author: pafarley
ms.openlocfilehash: dd986a7557f468b939aefe1da825c9834618047f
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81402911"
---
# <a name="tutorial-create-an-android-app-to-detect-and-frame-faces-in-an-image"></a>Öğretici: Resimdeki yüzleri algılamak ve çerçeve içine almak için Android uygulaması oluşturma

Bu öğreticide, bir görüntüdeki insan yüzlerini algılamak için Java SDK 'Sı aracılığıyla Azure yüz hizmetini kullanan bir Android uygulaması oluşturacaksınız. Uygulama seçili bir görüntüyü görüntüler ve algılanan her bir yüzeyi etrafında bir çerçeve çizer.

Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> - Android uygulaması oluşturma
> - Yüz istemci kitaplığını yükler
> - İstemci kitaplığını kullanarak resimdeki yüzleri algılama
> - Algılanan her yüzün çevresine bir çerçeve çizme

![Yüzleri kırmızı dikdörtgenle çerçeve içine alınmış bir fotoğrafın Android ekran görüntüsü](../Images/android_getstarted2.1.PNG)

Örnek kodun tamamı GitHub 'daki bilişsel [Hizmetler yüz Android](https://github.com/Azure-Samples/cognitive-services-face-android-sample) deposunda bulunur.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

- Yüz abonelik anahtarı. Deneme bilişsel [Hizmetler](https://azure.microsoft.com/try/cognitive-services/?api=face-api)'den ücretsiz bir deneme aboneliği anahtarı edinebilirsiniz. Ya da yüz hizmetine abone olmak ve anahtarınızı almak için bilişsel [Hizmetler oluşturma](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) ' daki yönergeleri izleyin. Ardından, sırasıyla ve `FACE_SUBSCRIPTION_KEY` `FACE_ENDPOINT`olarak adlandırılan anahtar ve hizmet uç noktası dizesi için [ortam değişkenleri oluşturun](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#configure-an-environment-variable-for-authentication) .
- [Visual Studio 2015 veya 2017](https://www.visualstudio.com/downloads/)'nin herhangi bir sürümü.
- API düzeyi 22 veya üzeri ile [Android Studio](https://developer.android.com/studio/) .

## <a name="create-the-android-studio-project"></a>Android Studio projesi oluşturma

Yeni bir Android uygulaması projesi oluşturmak için aşağıdaki adımları izleyin.

1. Android Studio **Yeni bir Android Studio projesi Başlat**' ı seçin.
1. **Android Projesi Oluştur** ekranında, gerekirse varsayılan alanları değiştirin ve sonra da **İleri**'ye tıklayın.
1. **Hedef Android cihazları** ekranında, **API 22** veya üzerini seçmek için açılan seçiciyi kullanın, ardından **İleri**' ye tıklayın.
1. **Boş Etkinlik** öğesini seçin ve **İleri**'ye tıklayın.
1. **Geriye Dönük Uyumluluk** öğesinin işaretini kaldırın ve **Son**'a tıklayın.

## <a name="add-the-initial-code"></a>Başlangıç kodunu ekleme

### <a name="create-the-ui"></a>Kullanıcı arabirimini oluşturma

*Activity_main. xml*' i açın. Düzen Düzenleyicisi 'nde **metin** sekmesini seçin, sonra içeriği aşağıdaki kodla değiştirin.

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/res/layout/activity_main.xml?name=snippet_activitymain)]

### <a name="create-the-main-class"></a>Ana sınıfı oluşturma

*MainActivity. Java* ' yı açın ve var `import` olan deyimleri aşağıdaki kodla değiştirin.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_imports)]

Ardından, **MainActivity** sınıfının içeriğini aşağıdaki kodla değiştirin. Bu, kullanıcının bir resim seçmesine izin vermek için yeni bir etkinlik başlatan **düğme** üzerinde bir olay işleyicisi oluşturur. Resmi **ImageView**içinde görüntüler.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_mainactivity_methods)]

### <a name="try-the-app"></a>Uygulamayı deneyin

**Onactivityresult** yönteminde **detectandframe** çağrısını not edin. Ardından, uygulamanızı test etmek için menüdeki **Çalıştır** ' a basın. Uygulama açıldığında, öykünücü veya bağlı bir cihazda, en alta **gözatıp** ' ye tıklayın. Cihazın dosya seçimi iletişim kutusu görünmelidir. Bir görüntü seçin ve pencerede görüntülendiğini doğrulayın. Ardından, uygulamayı kapatın ve sonraki adıma ilerleyin.

![Yüzler içeren bir fotoğrafın Android ekran görüntüsü](../Images/android_getstarted1.1.PNG)

## <a name="add-the-face-sdk"></a>Yüz SDK 'sını ekleme

### <a name="add-the-gradle-dependency"></a>Gradle bağımlılığını ekleme

**Proje** bölmesinde, aşağı açılan seçiciyi kullanarak **Android**'i seçin. **Gradle Scripts**'i genişletin, sonra *build.gradle (Modül: uygulama)* öğesini açın. Aşağıdaki ekran görüntüsünde gösterildiği gibi Yüz Tanıma istemci kitaplığı `com.microsoft.projectoxford:face:1.4.3` için bir bağımlılık ekleyin, sonra da **Şimdi Eşitle**'ye tıklayın.

![Uygulama build.gradle dosyasının Android Studio ekran görüntüsü](../Images/face-tut-java-gradle.png)

### <a name="add-the-face-related-project-code"></a>Yüz ile ilgili proje kodunu ekleme

**MainActivity. Java** ' ya dönün ve aşağıdaki `import` deyimleri ekleyin:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_face_imports)]

Ardından, aşağıdaki kodu **MainActivity** sınıfına, **OnCreate** yönteminin üstüne ekleyin:

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_mainactivity_fields)]

**Proje** bölmesinde **uygulamayı**, sonra da **bildirimleri** genişletin ve *AndroidManifest.xml* dosyasını açın. Aşağıdaki öğeyi `manifest` öğesinin doğrudan alt öğesi olarak ekleyin:

[!code-xml[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/AndroidManifest.xml?name=snippet_manifest_entry)]

## <a name="upload-image-and-detect-faces"></a>Görüntüyü karşıya yükleme ve yüzeyleri algılama

Uygulamanız, [algıla](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) REST API sarmalayan ve bir **yüz** örnekleri listesi döndüren **Faceclient. Face. detectwithstreamasync** yöntemini çağırarak yüzeyleri algılar.

Döndürülen her **yüz** , konumunu belirten, isteğe bağlı bir yüz öznitelikleri serisiyle birleştirilmiş bir dikdörtgen içerir. Bu örnekte, yalnızca yüz dikdörtgenler istenir.

Aşağıdaki iki yöntemi **MainActivity** sınıfına ekleyin. Yüz algılama işlemi tamamlandığında, uygulama **ImageView**'ı değiştirmek Için **Drawfacerectanglesonbitmap** yöntemini çağırır. Bu yöntemi bir sonraki adımda tanımlayacaksınız.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_detection_methods)]

## <a name="draw-face-rectangles"></a>Yüz yüze çiz

Aşağıdaki yardımcı yöntemi **MainActivity** sınıfına ekleyin. Bu yöntem her bir **yüz** örneğinin dikdörtgen koordinatlarını kullanarak algılanan her bir yüzey etrafında bir dikdörtgen çizer.

[!code-java[](~/cognitive-services-face-android-detect/FaceTutorial/app/src/main/java/com/contoso/facetutorial/MainActivity.java?name=snippet_drawrectangles)]

Son olarak, **Onactivityresult**Içindeki **detectandframe** yöntemine yapılan çağrının açıklamasını kaldırın.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Uygulamayı çalıştırın ve içinde yüzlerin yer aldığı bir resim bulun. Yüz Tanıma hizmetinin yanıt vermesi için birkaç saniye bekleyin. Görüntüdeki her yüzey üzerinde kırmızı bir dikdörtgen görmeniz gerekir.

![Kırmızı dikdörtgenlerin çevresinde çizmiş yüzlerinin Android ekran görüntüsü](../Images/android_getstarted2.1.PNG)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir görüntüdeki yüzeyleri tespit etmek ve çerçeveye eklemek üzere bir uygulama oluşturmak için yüz Java SDK 'sını nasıl kullanacağınızı öğrendiniz. Sonra, yüz algılama 'nın ayrıntıları hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Resimdeki Yüzleri Tanıma](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
