---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 5/23/2019
ms.author: travisw
ms.openlocfilehash: e1f9b561efb1fd88fe2dd00f8c175f1ebae848d8
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72391810"
---
1. Android Studio’yu başlatın ve Hoş Geldiniz penceresinde **Yeni bir Android Studio projesi başlat**’ı seçin.

    ![Android Studio Hoş Geldiniz penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. **Projenizi seçin** Sihirbazı görünür, etkinlik seçimi kutusunda **telefon ve tablet** ve **boş etkinlik** ' i seçin. **İleri**’yi seçin.

   ![Projenizin seçme Sihirbazı 'nın ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-02-target-android-devices.png)

1. **Projeyi Yapılandır** ekranında, **ad**olarak **hızlı başlangıç** girin, **paket adı**olarak **Samples.Speech.cognitiveservices.Microsoft.com** yazın ve bir proje dizini seçin. **En düşük API düzeyi** seçme **API 23: Android 6,0 (sıralamada Mallow)** için, diğer tüm onay kutularını işaretsiz bırakın ve **son**' u seçin.

   ![Proje yapılandırma Sihirbazı ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-03-create-android-project.png)

Android Studio’nun yeni Android projenizi hazırlaması biraz zaman alır. Daha sonra, projenizi yapılandırarak Konuşma SDK’sı hakkında bilgi edinmesini ve Java 8’i kullanmasını sağlayın.

[!INCLUDE [License Notice](cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.7.0`.

Android için konuşma SDK 'Sı, gerekli kitaplıkları ve gerekli Android izinlerini içeren bir [AAR (Android kitaplığı)](https://developer.android.com/studio/projects/android-library)olarak paketlenmiştir.
Https: \//csspeechstorage. blob. Core. Windows. net/Maven/konumundaki bir Maven deposunda barındırılır.

Konuşma SDK’sını kullanmak için projenizi ayarlayın. Android Studio menü çubuğundan **Dosya** > **Proje Yapısı**’nı seçerek Proje Yapısı penceresini açın. Proje Yapısı penceresinde aşağıdaki değişiklikleri yapın:

1. Pencerenin sol tarafındaki listede **Proje**’yi seçin. **Varsayılan Kitaplık Deposu** ayarlarını, bir virgül ve tek tırnak içine alınan Maven deposu URL'sini ekleyerek düzenleyin. ' https: \//csspeechstorage. blob. Core. Windows. net/Maven/'

   ![Proje Yapısı penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-06-add-maven-repository.png)

1. Aynı ekranın sol tarafında **uygulamayı** seçin. Sonra da, pencerenin en üstünde **Bağımlılıklar** sekmesini seçin. Yeşil artı işaretini (+) ve açılan menüden **Kitaplık bağımlılığı**’nı seçin.

   ![Proje Yapısı penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-07-add-module-dependency.png)

1. Açılan pencerede Android için Konuşma SDK’mızın adını ve sürümünü (`com.microsoft.cognitiveservices.speech:client-sdk:1.7.0`) girin. Sonra **Tamam**’ı seçin.
   Artık, aşağıda gösterildiği gibi Konuşma SDK’sı bağımlılıklar listesine eklenmiş olmalıdır:

   ![Proje Yapısı penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. **Özellikler** sekmesini seçin. **Kaynak uyumluluğu** ve **hedef uyumluluk**için **1,8**' yi seçin.

   ![](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-09-dependency-added.png)

1. Proje Yapısı penceresini kapatmak ve değişikliklerinizi projeye uygulamak için **Tamam**’ı seçin.
