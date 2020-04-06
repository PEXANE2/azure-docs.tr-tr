---
title: 'Quickstart: Bir mikrofon, Java (Android) konuşma tanıma - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Konuşma SDK'yı kullanarak Android'de Java'daki konuşmayı nasıl tanıyın
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 11/05/2019
ms.author: wolfma
ms.openlocfilehash: c37a52ea2a7d5f1d3e325c2f6d17bc075f50c5e0
ms.sourcegitcommit: 67addb783644bafce5713e3ed10b7599a1d5c151
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2020
ms.locfileid: "80671677"
---
## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce:

> [!div class="checklist"]
> * [Azure Konuşma Kaynağı Oluşturma](../../../../get-started.md)
> * [Geliştirme ortamınızı kur](../../../../quickstarts/setup-platform.md?tabs=android&pivots=programming-language-java)
> * Ses çekimi için mikrofona erişebildiğinizden emin olun

## <a name="create-a-user-interface"></a>Kullanıcı arabirimi oluşturma

Şimdi uygulama için temel bir kullanıcı arabirimi oluşturacağız. Ana etkinliğiniz `activity_main.xml` için düzende değişiklik yapın. Başlangıçta, düzen, uygulamanızın adını içeren bir başlık çubuğu ve "Hello World!" metnini içeren bir TextView içerir.

* TextView öğesini seçin. Sağ üst köşedeki ID özniteliğini `hello` olarak değiştirin.

* `activity_main.xml` Pencerenin sol üst kısmındaki paletten, düğmeyi metnin üzerindeki boş boş alana sürükleyin.

* Sağ taraftaki düğme özniteliklerde, `onClick` özniteliği değeri için `onSpeechButtonClicked` girin. Düğme olayını işlemek için bu adla bir yöntem yazacağız. Sağ üst köşedeki ID özniteliğini `button` olarak değiştirin.

* Tasarımcının en üstündeki sihirli değnek simgesini kullanarak düzen kısıtlamalarını ortaya çıkarın.

  ![Sihirli değnek simgesinin ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-10-infer-layout-constraints.png)

Kullanıcı arabiriminizin metin ve grafik gösterimi şimdi şu şekilde görünmelidir:

![Kullanıcı arabirimi](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-11-gui.png)

[!code-xml[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/from-microphone/app/src/main/res/layout/activity_main.xml)]

## <a name="add-sample-code"></a>Örnek kod ekleme

1. `MainActivity.java` kaynak dosyasını açın. Bu dosyadaki tüm kodu aşağıdakilerle değiştirin:

   [!code-java[](~/samples-cognitive-services-speech-sdk/quickstart/java/android/from-microphone/app/src/main/java/com/microsoft/cognitiveservices/speech/samples/quickstart/MainActivity.java#code)]

   * `onCreate` yönteminde mikrofon ve İnternet izinleri isteyen ve yerel platform bağlaması başlatan kod vardır. Yerel platform bağlamaları tek bir kez yapılandırılmalıdır. Bu işlem uygulama başlatma sırasında erken bir aşamada yapılmalıdır.

   * `onSpeechButtonClicked` yöntemi daha önce de belirtildiği gibi düğme tıklama işleyicisidir. Bir düğmeye basmak konuşmadan metne transkripsiyonu tetikler.

1. Aynı dosyada, `YourSubscriptionKey` dizesini abonelik anahtarınızla değiştirin.

1. Ayrıca dizeyi `YourServiceRegion` aboneliğinizle ilişkili [bölgeden](https://aka.ms/speech/sdkregion) **Bölge tanımlayıcısıyla** değiştirin. Örneğin, ücretsiz `westus` deneme aboneliği için kullanın.

## <a name="build-and-run-the-app"></a>Uygulamayı derleme ve çalıştırma

1. Android cihazınızı geliştirme bilgisayarınıza bağlayın. Aygıtta geliştirme [modunu ve USB hata ayıklamayı](https://developer.android.com/studio/debug/dev-options) etkinleştirdiğinizden emin olun.

1. Uygulamayı oluşturmak için Ctrl+F9'u seçin veya menü çubuğundan**Yapı Projesi'ni** **seçin.** > 

1. Uygulamayı başlatmak için Shift+F10'u seçin veya **Çalıştır** > **'uygulamasını'** seçin.

1. Görünen dağıtım hedef penceresinde Android cihazınızı seçin.

   ![Dağıtım Hedefi Seç penceresinin ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-12-deploy.png)

Konuşma tanıma bölümüne başlamak için uygulamadaki düğmeyi seçin. Bunu izleyen 15 saniyelik İngilizce konuşma Konuşma hizmetine gönderilir ve transkripsiyonu yapılır. Sonuç, Android uygulamasında ve Android Studio'daki logcat penceresinde gösterilir.

![Android uygulamasının ekran görüntüsü](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-13-gui-on-device.png)

## <a name="next-steps"></a>Sonraki adımlar

[!INCLUDE [footer](./footer.md)]

