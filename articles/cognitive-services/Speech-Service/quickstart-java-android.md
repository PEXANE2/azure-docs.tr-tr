---
title: 'Hızlı başlangıç: konuşmayı tanıma, Java (Android)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak Android 'te Java 'da konuşmayı tanımayı öğrenin
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: 2fb7bc0851868f6551ed57d742c4005550caec99
ms.sourcegitcommit: 9a4296c56beca63430fcc8f92e453b2ab068cc62
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2019
ms.locfileid: "72675548"
---
# <a name="quickstart-recognize-speech-in-java-on-android-by-using-the-speech-sdk"></a>Hızlı Başlangıç: Konuşma SDK’sını kullanarak Android üzerinde Java dilinde konuşma tanıma

Hızlı başlangıçlara [konuşma sensıs](quickstart-text-to-speech-java-android.md) ve [ses-ilk Sanal Yardımcısı](quickstart-virtual-assistant-java-android.md)için de erişilebilir.

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, konuşmayı metne eklemek için Azure bilişsel hizmetler konuşma SDK 'sını kullanarak Android için bir Java uygulaması geliştirmeyi öğreneceksiniz.

Uygulama, konuşma SDK Maven paketini ve Android Studio 3,3 ' i temel alır. Konuşma SDK 'Sı Şu anda 32 bit veya 64 bit ARM ve Intel x86/x64 uyumlu işlemciler içeren Android cihazlarla uyumludur.

> [!NOTE]
> Konuşma Cihazları SDK’sı ve Roobo cihazı için bkz. [Konuşma Cihazları SDK’sı](speech-devices-sdk.md).

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlayabilmeniz için bir konuşma Hizmetleri abonelik anahtarına ihtiyacınız vardır. Anahtarı ücretsiz alabilirsiniz. Daha fazla bilgi için bkz. [konuşma hizmetlerini ücretsiz deneyin](get-started.md).

## <a name="create-and-configure-a-project"></a>Projeyi oluşturma ve yapılandırma

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-java-android-create-proj.md)]

## <a name="create-a-user-interface"></a>Kullanıcı arabirimi oluşturma

Artık uygulama için temel bir kullanıcı arabirimi oluşturacağız. Ana etkinliğiniz `activity_main.xml` için düzende değişiklik yapın. Başlangıçta, düzen uygulamanızın adıyla bir başlık çubuğu ve "Merhaba Dünya!" metnini içeren bir TextView içerir.

* TextView öğesini seçin. Sağ üst köşedeki ID özniteliğini `hello` olarak değiştirin.

* `activity_main.xml` penceresinin sol üst köşesindeki paletten, bir düğmeyi metnin üstündeki boş alana sürükleyin.

* Sağ taraftaki düğme özniteliklerde, `onClick` özniteliği değeri için `onSpeechButtonClicked` girin. Düğme olayını işlemek için bu adla bir yöntem yazacağız. Sağ üst köşedeki ID özniteliğini `button` olarak değiştirin.

* Tasarımcının en üstündeki sihirli değnek simgesini kullanarak düzen kısıtlamalarını ortaya çıkarın.

  ![Sihirli değnek simgesinin ekran görüntüsü](media/sdk/qs-java-android-10-infer-layout-constraints.png)

UI 'nizin metin ve grafik gösterimi şu şekilde görünmelidir:

![Kullanıcı arabirimi](media/sdk/qs-java-android-11-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Örnek kodu ekleyin

1. `MainActivity.java` kaynak dosyasını açın. Bu dosyadaki tüm kodu aşağıdaki kodla değiştirin:

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java-android/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * `onCreate` yönteminde mikrofon ve İnternet izinleri isteyen ve yerel platform bağlaması başlatan kod vardır. Yerel platform bağlamaları tek bir kez yapılandırılmalıdır. Bu işlem uygulama başlatma sırasında erken bir aşamada yapılmalıdır.

   * `onSpeechButtonClicked` yöntemi daha önce de belirtildiği gibi düğme tıklama işleyicisidir. Bir düğme, konuşmadan metne dökümü tetiklemesini tetikler.

1. Aynı dosyada `YourSubscriptionKey` dizesini abonelik anahtarınız ile değiştirin.

1. Ayrıca dize `YourServiceRegion`, aboneliğiniz ile ilişkili [bölge](regions.md) ile değiştirin. Örneğin, ücretsiz deneme aboneliği için `westus` kullanın.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Android cihazınızı geliştirme bilgisayarınıza bağlayın. Cihazda [geliştirme modunu ve USB hata ayıklamayı](https://developer.android.com/studio/debug/dev-options) etkinleştirdiğinizden emin olun.

1. Uygulamayı derlemek için CTRL + F9 ' ı seçin veya menü çubuğundan **projeyi** oluştur > **Oluştur** ' u seçin.

1. Uygulamayı başlatmak için SHIFT + F10 ' i seçin veya **çalıştır** >  **' uygulama ' Çalıştır '** ı seçin.

1. Görüntülenen dağıtım hedefi penceresinde Android cihazınızı seçin.

   ![Dağıtım Hedefi Seç penceresinin ekran görüntüsü](media/sdk/qs-java-android-12-deploy.png)

Konuşma tanıma bölümüne başlamak için uygulamadaki düğmeyi seçin. Ingilizce konuşmayı izleyen 15 saniyelik bir konuşma, konuşma hizmetlerine ve yeniden yüklemeye gönderilir. Sonuç, Android uygulamasında ve Android Studio'daki logcat penceresinde gösterilir.

![Android uygulamasının ekran görüntüsü](media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub 'da Java örneklerini keşfet](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Ayrıca bkz.

- [Akustik modelleri özelleştirme](how-to-customize-acoustic-models.md)
- [Dil modellerini özelleştirme](how-to-customize-language-model.md)
