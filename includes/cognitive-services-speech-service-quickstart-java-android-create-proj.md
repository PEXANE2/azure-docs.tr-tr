---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 02/10/2020
ms.author: travisw
ms.openlocfilehash: a7ce6bc2b6fa272c8dd3c4bf95bc06a37d1fe28f
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77119727"
---
1. Android Studio başlatın ve **hoş geldiniz** penceresinde **Yeni bir Android Studio projesi Başlat** ' ı seçin.

    ![Android Studio Hoş Geldiniz penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. **Projenizi seçin** Sihirbazı görüntülenir. Etkinlik seçimi kutusunda **telefon ve tablet** ve **boş etkinlik** ' i seçin. **İleri**’yi seçin.

   ![Projenizin seçme Sihirbazı 'nın ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-02-target-android-devices.png)

1. **Projeyi Yapılandır** ekranında, **ad** olarak *hızlı başlangıç* girin ve **paket adı**olarak *Samples.Speech.cognitiveservices.Microsoft.com* girin. Ardından bir proje dizini seçin. **En düşük API düzeyi**için **API 23: Android 6,0 (sıralamada Mallow)** öğesini seçin. Diğer tüm onay kutularını temizleyin ve **son**' u seçin.

   ![Proje yapılandırma Sihirbazı ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-03-create-android-project.png)

Android Studio’nun yeni Android projenizi hazırlaması biraz zaman alır. Daha sonra, Azure bilişsel hizmetler konuşma SDK 'sını ve Java 8 ' i kullanmak için projeyi yapılandırın.

[!INCLUDE [License notice](cognitive-services-speech-service-license-notice.md)]

Bilişsel hizmetler konuşma SDK 'sının geçerli sürümü 1.9.0.

Android için konuşma SDK 'Sı, gerekli kitaplıkları ve gerekli Android izinlerini içeren bir [AAR (Android kitaplığı)](https://developer.android.com/studio/projects/android-library)olarak paketlenmiştir.
Https:\//csspeechstorage.blob.core.windows.net/maven/adresinden bir Maven deposunda barındırılır.

Konuşma SDK’sını kullanmak için projenizi ayarlayın. Android Studio menü çubuğundan **dosya** > **proje yapısı ' nı** seçerek **proje yapısı** penceresini açın. **Proje yapısı** penceresinde, aşağıdaki değişiklikleri yapın:

1. Pencerenin sol tarafındaki listede **Proje**’yi seçin. **Varsayılan kitaplık deposu** ayarlarını, tek tırnak işareti içine alınmış bir virgül ve Maven depo URL 'si ekleyerek düzenleyin: ' https:\//csspeechstorage.blob.Core.Windows.net/Maven/'

   ![Proje Yapısı penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-06-add-maven-repository.png)

1. Aynı ekranda, sol taraftaki **uygulama**' yı seçin. Sonra da, pencerenin en üstünde **Bağımlılıklar** sekmesini seçin. Yeşil artı işaretini ( **+** ) seçin ve açılan menüden **kitaplık bağımlılığı** ' nı seçin.

   ![Kitaplık bağımlılığının ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-07-add-module-dependency.png)

1. Görüntülenen pencerede, Android için konuşma SDK 'sının adını ve sürümünü girin, *com. Microsoft. biliveservices. Speech: Client-SDK: 1.9.0*. Sonra **Tamam**’ı seçin.
   Konuşma SDK 'Sı şu şekilde gösterildiği gibi bağımlılıklar listesine eklenmelidir:

   ![Bağımlılıklar listesinde konuşma SDK 'sının ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. **Özellikler** sekmesini seçin. **Kaynak uyumluluğu** ve **hedef uyumluluk**için **1,9**' yi seçin.

   ![Kaynak uyumluluğu ve hedef uyumluluğun ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-09-dependency-added.png)

1. **Proje yapısı** penceresini kapatmak ve değişikliklerinizi projeye uygulamak için **Tamam** ' ı seçin.
