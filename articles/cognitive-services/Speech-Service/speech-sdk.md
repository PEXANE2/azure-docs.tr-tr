---
title: Konuşma SDK 'Sı-konuşma hizmeti hakkında
titleSuffix: Azure Cognitive Services
description: Konuşma yazılım geliştirme seti (SDK), uygulamalarınızın konuşma hizmeti işlevlerine yerel erişim sağlayarak yazılım geliştirmeyi kolaylaştırır. Bu makalede, SDK'sı için Windows, Linux ve Android hakkında ek ayrıntılar sağlar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/10/2019
ms.author: wolfma
ms.openlocfilehash: 7bd4541d135020e297bef26f234ae35d953abfce
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75446810"
---
# <a name="about-the-speech-sdk"></a>Konuşma SDK'sı hakkında

Konuşma Yazılım Geliştirme Seti (SDK), konuşma özellikli yazılım geliştirmeyi kolaylaştıran konuşma hizmeti işleve, uygulamaları erişmenizi sağlar. Şu anda SDK 'lar, **konuşmadan metne**, **metinden konuşmaya**, **konuşma çevirisi**, **Amaç tanıma**ve **bot çerçevesinin doğrudan hat konuşma kanalına**erişim sağlar.

Konuşmayı bir mikrofondan kolayca yakalayabilir, bir akıştan okuyabilir veya ses dosyalarına konuşma SDK 'Sı ile erişebilirsiniz. Konuşma SDK'sı WAV/PCM 16 bit, 16 kHz/8 kHz, tek kanallı sesten konuşma tanıma gerçekleştirebilir. Ek ses biçimleri, [konuşmadan metne REST uç noktası](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) veya [toplu iş dökümü hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats)kullanılarak desteklenir.

Yetenekler ve desteklenen platformlar hakkında genel bir bakış belge [giriş sayfasında](https://aka.ms/csspeech)bulunabilir.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>SDK'yı edinme

### <a name="windows"></a>Windows

Windows için aşağıdaki dilleri destekliyoruz:

* C# (UWP ve .NET), C++: başvuru ve bizim konuşma SDK'sı NuGet paketi en son sürümünü kullanın. Paket, 32-bit ve 64 bit istemci kitaplıkları ve yönetilen (.NET) kitaplıkları içerir. SDK, Visual Studio 'da NuGet, [Microsoft. Biliveservices. Speech](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech)kullanılarak yüklenebilir.

* Java: Başvurabilir ve yalnızca Windows x64 destekleyen bizim konuşma SDK Maven paketini en son sürümünü kullanın. Maven projenizde ekleyin `https://csspeechstorage.blob.core.windows.net/maven/` bir ek bir depo ve başvuru olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.8.0` bağımlılık olarak.

### <a name="linux"></a>Linux

> [!NOTE]
> Şu anda, aşağıdaki hedef mimarilerde yalnızca Ubuntu 16,04, Ubuntu 18,04 ve detik 9 ' u destekliyoruz:
> - geliştirme için C++ x86, x64 ve ARM64
> - Java için x64 ve ARM64
> - .NET Core ve Python için x64

Aşağıdaki kabuk komutlarını çalıştırarak gerekli kitaplıkların yüklü olduğundan emin olun:

Ubuntu 'da:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

On yıl 9 ' da:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

* C# ' ta: Başvuru ve bizim konuşma SDK'sı NuGet paketi en son sürümünü kullanın. SDK'ya başvurmak için şu paket başvuruyu projenize ekleyin:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.8.0" />
  ```

* Java: Başvurabilir ve bizim konuşma SDK Maven paketini en son sürümünü kullanın. Maven projenizde ekleyin `https://csspeechstorage.blob.core.windows.net/maven/` bir ek bir depo ve başvuru olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0` bağımlılık olarak.

* C++: SDK'yı indirin bir [.tar paketinin](https://aka.ms/csspeech/linuxbinary) ve tercih ettiğiniz bir dizindeki dosyaları paket. Aşağıdaki tablo, SDK'sı klasör yapısını gösterir:

  |Yol|Açıklama|
  |-|-|
  |`license.md`|Lisans|
  |`ThirdPartyNotices.md`|Üçüncü taraf bildirimleri|
  |`include`|C ve C++ üstbilgi dosyaları|
  |`lib/x64`|Yerel x64 uygulamanızla bağlama kitaplığı|
  |`lib/x86`|Yerel x86 uygulamanızla bağlama kitaplığı|

  Bir uygulama oluşturmak için kopyalayabilir veya gerekli ikili dosyaların (ve kitaplıkları) geliştirme ortamınıza taşıyabilirsiniz. Yapı işleminiz için gereken şekilde bunları ekleyebilirsiniz.

### <a name="android"></a>Android

Android için Java SDK 'Sı, gerekli kitaplıkları ve gerekli Android izinlerini içeren bir [AAR (Android kitaplığı)](https://developer.android.com/studio/projects/android-library)olarak paketlenmiştir. Maven deponun barındırılan `https://csspeechstorage.blob.core.windows.net/maven/` paketi olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0`.

Android Studio projenizden paketi kullanmak için aşağıdaki değişiklikleri yapın:

* Proje düzeyi build.gradle dosyasına ekleyin `repository` bölümü:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Modül düzeyi build.gradle dosyasına ekleyin `dependencies` bölümü:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.7.0'
  ```

Java SDK'sını da olan parçası [konuşma cihaz SDK'sı](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
