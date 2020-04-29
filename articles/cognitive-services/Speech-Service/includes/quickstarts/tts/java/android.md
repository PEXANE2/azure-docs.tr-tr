---
title: 'Hızlı başlangıç: Sentezleştirme konuşma, Java (Android)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak Android 'de konuşmayı nasıl sentezleyeceğinizi öğrenin
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 04/04/2020
ms.author: yulili
ms.openlocfilehash: d114e75a08f31a664772b84e19ec4d93b453af0b
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81275424"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunları yaptığınızdan emin olun:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurun ve boş bir proje oluşturun](../../../../quickstarts/setup-platform.md?tabs=android&pivots=programming-language-java)

## <a name="create-user-interface"></a>Kullanıcı arabirimi oluşturma

Uygulama için temel bir kullanıcı arabirimi oluşturacağız. Ana etkinliğiniz `activity_main.xml` için düzende değişiklik yapın. Başlangıçta, düzen uygulamanızın adıyla bir başlık çubuğu ve "Merhaba Dünya!" metnini içeren bir TextView içerir.

1. TextView öğesine tıklayın. Sağ üst köşedeki ID özniteliğini olarak `outputMessage`değiştirin ve daha düşük ekrana sürükleyin. Metnini silin.

1. `activity_main.xml` penceresinin sol üst tarafındaki Paletten bir düğmeyi metnin üst kısmındaki boş alana sürükleyin.

1. Sağ taraftaki düğme özniteliklerde, `onClick` özniteliği değeri için `onSpeechButtonClicked` girin. Düğme olayını işlemek için bu adla bir yöntem yazacağız.  Sağ üst köşedeki ID özniteliğini `button` olarak değiştirin.

1. Bir düz metni düğmenin üzerindeki alana sürükleyin; ID özniteliğini olarak `speakText`değiştirin ve metin özniteliğini olarak `Hi there!`değiştirin.

1. Tasarımcının en üstündeki sihirli değnek simgesini kullanarak düzen kısıtlamalarını ortaya çıkarın.


    ![Sihirli değnek simgesinin ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-10-infer-layout-constraints.png)

UI 'nizin metin ve grafik gösterimi şu şekilde görünmelidir:

![](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-11-2-tts-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/text-to-speech/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. `MainActivity.java` kaynak dosyasını açın. Bu dosyanın içindeki kodun tamamını aşağıdakiyle değiştirin.

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/text-to-speech/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * `onSpeechButtonClicked` yöntemi daha önce de belirtildiği gibi düğme tıklama işleyicisidir. Bir düğme, konuşma senşlerini tetikler.

1. Aynı dosyada, `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. Ayrıca `YourServiceRegion` dizesini de aboneliğinizle ilişkili [bölge](~/articles/cognitive-services/Speech-Service/regions.md) ile (örneğin ücretsiz deneme aboneliğinde `westus`) değiştirin.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Android cihazınızı geliştirme bilgisayarınıza bağlayın. Cihazda [geliştirme modunu ve USB hata ayıklamasını](https://developer.android.com/studio/debug/dev-options) etkinleştirdiğinizden emin olun. Alternatif olarak, bir [Android öykünücüsü](https://developer.android.com/studio/run/emulator)oluşturun.

1. Uygulamayı derlemek için CTRL + F9 tuşlarına basın veya menü çubuğunda projeyi **Oluştur ' u seçin.** > **Make Project**

1. Uygulamayı başlatmak için SHIFT + F10 tuşlarına basın **veya çalıştırmayı Çalıştır** > **' uygulama '** yı seçin.

1. Görüntülenen dağıtım hedefi penceresinde, Android cihazınızı veya öykünücüyü seçin.

   ![Dağıtım Hedefi Seç penceresinin ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-12-deploy.png)

Konuşma birleştirme bölümüne başlamak için bir metin girin ve uygulamadaki düğmeye basın. Varsayılan konuşmacının birleştirilmiş sesini duyacaktır ve ekrandaki `speech synthesis succeeded` bilgileri görürsünüz.

![Android uygulamasının ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-13-2-gui-on-device-tts.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech synthesis basics](../../text-to-speech-next-steps.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Özel bir ses oluşturun](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Özel ses örneklerini Kaydet](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
