---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 47cb20f3a23caf586777523e56902af20b747ea1
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399984"
---
:::row:::
    :::column span="3":::
        Android için Java SDK, gerekli kitaplıkları ve gerekli Android izinlerini içeren bir <a href="https://developer.android.com/studio/projects/android-library" target="_blank">AAR (Android <span class="docon docon-navigate-external x-hidden-focus"> </span>Kütüphanesi) </a>olarak paketlenmiştir. Maven deposunda paket `https://csspeechstorage.blob.core.windows.net/maven/` `com.microsoft.cognitiveservices.speech:client-sdk:1.11.0`olarak barındırılan.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Java" src="https://docs.microsoft.com/media/logos/logo_java.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

Android Studio projenizdeki paketi tüketmek için aşağıdaki değişiklikleri yapın:

1. Proje düzeyinde *build.gradle* dosyasında, `repository` bölüme aşağıdakileri ekleyin:
  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

2. Modül düzeyinde *build.gradle* dosyasında, `dependencies` bölüme aşağıdakileri ekleyin:
  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.11.0'
  ```

Java SDK da Konuşma [Cihazları SDK](../speech-devices-sdk.md)parçasıdır.

#### <a name="additional-resources"></a>Ek kaynaklar

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android" target="_blank">Android özel Java quickstart kaynak kodu<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/jre" target="_blank">Java Runtime (JRE) quickstart kaynak kodu<span class="docon docon-navigate-external x-hidden-focus"></span></a>