---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 2ab636679e59536a2ddfaa8603dc2da45811cd2f
ms.sourcegitcommit: d1e56036f3ecb79bfbdb2d6a84e6932ee6a0830e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99213798"
---
:::row:::
    :::column span="3":::
        Android için Java SDK 'Sı, gerekli kitaplıkları ve gerekli Android izinlerini içeren bir <a href="https://developer.android.com/studio/projects/android-library" target="_blank">AAR (Android kitaplığı) <span class="docon docon-navigate-external x-hidden-focus"></span> </a>olarak paketlenmiştir. Paket olarak bir Maven deposunda barındırılır `https://csspeechstorage.blob.core.windows.net/maven/` `com.microsoft.cognitiveservices.speech:client-sdk:1.15.0` .
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Java" src="https://docs.microsoft.com/media/logos/logo_java.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

Android Studio projenizden paketi kullanmak için aşağıdaki değişiklikleri yapın:

1. Proje düzeyi *Build. Gradle* dosyasında aşağıdakileri `repositories` bölümüne ekleyin:
  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

2. Modül düzeyi *Build. Gradle* dosyasında, bölümüne aşağıdakini ekleyin `dependencies` :
  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.15.0'
  ```

Java SDK 'sı Ayrıca [konuşma cihazları SDK 'sının](../speech-devices-sdk.md)bir parçasıdır.

#### <a name="additional-resources"></a>Ek kaynaklar

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/android" target="_blank">Android 'e özgü Java hızlı başlangıç kaynak kodu <span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/java/jre" target="_blank">Java çalışma zamanı (JRE) hızlı başlangıç kaynak kodu <span class="docon docon-navigate-external x-hidden-focus"></span></a>
