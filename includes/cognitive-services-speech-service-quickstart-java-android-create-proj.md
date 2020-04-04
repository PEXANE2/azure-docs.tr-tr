---
author: trrwilson
ms.service: cognitive-services
ms.topic: include
ms.date: 02/10/2020
ms.author: travisw
ms.openlocfilehash: 8b187e058299f8aa8b762231c0ed1e708e5ad9d1
ms.sourcegitcommit: 0450ed87a7e01bbe38b3a3aea2a21881f34f34dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80659338"
---
1. Android Studio'yu başlatın ve **Hoş Geldiniz** penceresinde yeni bir Android Studio **projesi başlat'ı** seçin.

    ![Android Studio Hoş Geldiniz penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-01-start-new-android-studio-project.png)

1. **Proje sihirbazınızı seç** görüntülenir. Etkinlik seçim kutusunda **Telefon ve Tablet ve** Boş **Etkinlik'i** seçin. **Sonraki'ni**seçin.

   ![Proje sihirbazınızı seçin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-02-target-android-devices.png)

1. Proje **ekranınızı Yapılandır' da,** **Quickstart'ı Ad** olarak girin ve **paket adı**olarak *samples.speech.cognitiveservices.microsoft.com* girin. *Quickstart* Ardından bir proje dizini seçin. **Minimum API düzeyi**için **API 23: Android 6.0 (Marshmallow)** seçeneğini belirleyin. Diğer tüm onay kutularını açık bırakın ve **Finish'i**seçin.

   ![Proje sihirbazınızı Yapılandırın ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-03-create-android-project.png)

Android Studio’nun yeni Android projenizi hazırlaması biraz zaman alır. Ardından, projeyi Azure Bilişsel Hizmetler Konuşma SDK'sını öğrenmek ve Java 8'i kullanmak üzere yapılandırın.

[!INCLUDE [License notice](cognitive-services-speech-service-license-notice.md)]

Bilişsel Hizmetler Konuşma SDK geçerli sürümü 1.11.0 olduğunu.

Android için Konuşma SDK bir [AAR (Android Kütüphanesi)](https://developer.android.com/studio/projects/android-library)olarak paketlenmiştir, gerekli kütüphaneler ve gerekli Android izinleri içerir.
Https:\//csspeechstorage.blob.core.windows.net/maven/ adresindeki Bir Maven deposunda barındırılan.'s.

Konuşma SDK’sını kullanmak için projenizi ayarlayın. Android Studio menü çubuğundan **Dosya** > **Proje Yapısı'nı** seçerek Proje **Yapısı** penceresini açın. Proje **Yapısı** penceresinde aşağıdaki değişiklikleri yapın:

1. Pencerenin sol tarafındaki listede **Proje**’yi seçin. Bir virgül ve Maven depo URL'mizi tek tırnak işaretleriyle ekleyerek **Varsayılan Kitaplık Deposu** ayarlarını düzenleme: 'https:\//csspeechstorage.blob.core.windows.net/maven/'

   ![Proje Yapısı penceresinin ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-06-add-maven-repository.png)

1. Aynı ekranda, sol tarafta, **uygulama**seçin. Sonra da, pencerenin en üstünde **Bağımlılıklar** sekmesini seçin. Yeşil artı işaretini**+**(), ve açılan menüden **Kitaplık bağımlılığı'nı** seçin.

   ![Kitaplık bağımlılığının ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-07-add-module-dependency.png)

1. Görünen pencereye, Android için Konuşma SDK adını ve sürümünü girin, *com.microsoft.cognitiveservices.speech:client-sdk:1.11.0*. Sonra **Tamam**’ı seçin.
   Konuşma SDK şimdi, gösterildiği gibi bağımlılıklar listesine eklenmelidir:

   ![Bağımlılıklar listesinde Konuşma SDK ekran görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-08-dependency-added-1.0.0.png)

1. **Özellikler** sekmesini seçin. Hem **Kaynak Uyumluluğu** hem de **Hedef Uyumluluğu**için **1,9'u**seçin.

   ![Kaynak Uyumluluğu ve Hedef Uyumluluğu Ekran Görüntüsü](../articles/cognitive-services/Speech-Service/media/sdk/qs-java-android-09-dependency-added.png)

1. **Proje Yapısı** penceresini kapatmak ve değişikliklerinizi projeye uygulamak için **Tamam'ı** seçin.
