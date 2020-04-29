---
title: 'Hızlı başlangıç: bir mikrofondan konuşmayı tanıma, Java (Android)-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK 'sını kullanarak Android 'te Java 'da konuşmayı tanımayı öğrenin
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/05/2019
ms.author: wolfma
ms.openlocfilehash: 2641464e52b80c59b6c6b5a123ad1605c1d41495
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81275249"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure konuşma kaynağı oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kurma](../../../../quickstarts/setup-platform.md?tabs=android&pivots=programming-language-java)
> * Ses yakalama için bir mikrofona erişiminizin olduğundan emin olun

## <a name="create-a-user-interface"></a>Kullanıcı arabirimi oluşturma

Artık uygulama için temel bir kullanıcı arabirimi oluşturacağız. Ana etkinliğiniz `activity_main.xml` için düzende değişiklik yapın. Başlangıçta, düzen uygulamanızın adıyla bir başlık çubuğu ve "Merhaba Dünya!" metnini içeren bir TextView içerir.

* TextView öğesini seçin. Sağ üst köşedeki ID özniteliğini `hello` olarak değiştirin.

* `activity_main.xml` Pencerenin sol üst kısmındaki paletten bir düğmeyi metnin üstündeki boş alana sürükleyin.

* Sağ taraftaki düğme özniteliklerde, `onClick` özniteliği değeri için `onSpeechButtonClicked` girin. Düğme olayını işlemek için bu adla bir yöntem yazacağız. Sağ üst köşedeki ID özniteliğini `button` olarak değiştirin.

* Tasarımcının en üstündeki sihirli değnek simgesini kullanarak düzen kısıtlamalarını ortaya çıkarın.

  ![Sihirli değnek simgesinin ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-10-infer-layout-constraints.png)

UI 'nizin metin ve grafik gösterimi şu şekilde görünmelidir:

![Kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-11-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/from-microphone/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. `MainActivity.java` kaynak dosyasını açın. Bu dosyadaki tüm kodu aşağıdaki kodla değiştirin:

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/from-microphone/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * `onCreate` yönteminde mikrofon ve İnternet izinleri isteyen ve yerel platform bağlaması başlatan kod vardır. Yerel platform bağlamaları tek bir kez yapılandırılmalıdır. Bu işlem uygulama başlatma sırasında erken bir aşamada yapılmalıdır.

   * `onSpeechButtonClicked` yöntemi daha önce de belirtildiği gibi düğme tıklama işleyicisidir. Bir düğme, konuşmadan metne dökümü tetiklemesini tetikler.

1. Aynı dosyada, `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. Ayrıca dizeyi `YourServiceRegion` , [aboneliğinizle ilişkili olan bölge](https://aka.ms/speech/sdkregion) **tanımlayıcısı** ile değiştirin. Örneğin, ücretsiz deneme `westus` aboneliği için kullanın.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Android cihazınızı geliştirme bilgisayarınıza bağlayın. Cihazda [geliştirme modunu ve USB hata ayıklamayı](https://developer.android.com/studio/debug/dev-options) etkinleştirdiğinizden emin olun.

1. Uygulamayı derlemek için CTRL + F9 ' ı seçin veya menü çubuğundan**projeyi** **Oluştur ' u seçin.** > 

1. Uygulamayı başlatmak için SHIFT + F10 ' i seçin **veya çalıştırmayı Çalıştır** > **' uygulama '** yı seçin.

1. Görüntülenen dağıtım hedefi penceresinde Android cihazınızı seçin.

   ![Dağıtım Hedefi Seç penceresinin ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-12-deploy.png)

Konuşma tanıma bölümüne başlamak için uygulamadaki düğmeyi seçin. Bunu izleyen 15 saniyelik İngilizce konuşma Konuşma hizmetine gönderilir ve transkripsiyonu yapılır. Sonuç, Android uygulamasında ve Android Studio'daki logcat penceresinde gösterilir.

![Android uygulamasının ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [Speech recognition basics](../../speech-to-text-next-steps.md)]
