---
title: Konuşma SDK Hakkında - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Konuşma Yazılımı Geliştirme Kiti (SDK), uygulamalarınıza Konuşma hizmetinin işlevlerine yerel erişim sağlayarak yazılım geliştirmeyi kolaylaştırır. Bu makalede, Windows, Linux ve Android için SDK hakkında ek ayrıntılar verilmektedir.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: dapine
ms.openlocfilehash: 984d2dfe07faa22756b4be167aa86a69806b1a84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78331102"
---
# <a name="about-the-speech-sdk"></a>Konuşma SDK'sı hakkında

Konuşma Yazılımı Geliştirme Kiti (SDK), uygulamalarınıza Konuşma hizmetinin işlevlerine erişim sağlayarak konuşma özellikli yazılım geliştirmeyi kolaylaştırır. Şu anda, SDK'lar **konuşma-to-metin,** **metin-konuşma,** **konuşma çevirisi,** **niyet tanıma**ve **Bot Framework's Direct Line Konuşma kanalına**erişim sağlar.

Mikrofondan kolayca ses yakalayabilir, bir akıştan okuyabilir veya Speech SDK ile depolamadan ses dosyalarına erişebilirsiniz. Konuşma SDK WAV/PCM 16-bit, 16 kHz/8 kHz, konuşma tanıma için tek kanallı ses destekler. Ek ses biçimleri [konuşma-to-metin REST bitiş noktası](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) veya [toplu transkripsiyon hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats)kullanılarak desteklenir.

Özellikler ve desteklenen platformlar hakkında genel bir genel bakış belge [giriş sayfasında](https://aka.ms/csspeech)bulunabilir.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>SDK'yı alın

# <a name="windows"></a>[Windows](#tab/windows)

> [!WARNING]
> Konuşma SDK, Windows 10 veya sonraki sürümleri destekler. Önceki Windows sürümleri **desteklenmez.**

Windows için aşağıdaki dilleri destekliyoruz:

* C# (UWP ve .NET), C++: Konuşma SDK NuGet paketimizin en son sürümüne başvuruyapabilir ve kullanabilirsiniz. Paket, 32 bit ve 64 bit istemci kitaplıkları ve yönetilen (.NET) kitaplıklarını içerir. SDK Visual Studio NuGet, [Microsoft.CognitiveServices.Speech](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech)kullanılarak yüklenebilir.

* Java: Yalnızca Windows x64'ü destekleyen Konuşma SDK Maven paketimizin en son sürümüne başvurup kullanabilirsiniz. Maven projenizde, `https://csspeechstorage.blob.core.windows.net/maven/` bağımlılık olarak ek bir depo `com.microsoft.cognitiveservices.speech:client-sdk:1.8.0` ve başvuru olarak ekleyin.

# <a name="linux"></a>[Linux](#tab/linux)

> [!NOTE]
> Şu anda sadece Ubuntu 16.04, Ubuntu 18.04, Debian 9, Red Hat Enterprise Linux (RHEL) 8 ve CentOS 8'i aşağıdaki hedef mimarilerde destekliyoruz:
> - x86 (Debian/Ubuntu), x64, ARM32 (Debian/Ubuntu) ve Arm64 (Debian/Ubuntu) C++ gelişimi için
> - x64, ARM32 (Debian/Ubuntu) ve JAVA için ARM64 (Debian/Ubuntu)
> - .NET Çekirdek ve Python için x64

Aşağıdaki kabuk komutlarını çalıştırarak gerekli kitaplıklara sahip olduğundan emin olun:

Ubuntu üzerinde:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libasound2
```

Debian 9'da:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.2 libasound2
```

RHEL/CentOS 8'de:

```sh
sudo yum update
sudo yum install alsa-lib openssl
```

> [!NOTE]
> RHEL/CentOS 8'de [OpenSSL'in Linux için nasıl yapılandırılacağı](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yla ilgili talimatları uygulayın.

* C#: Konuşma SDK NuGet paketimizin en son sürümüne başvuruyapabilir ve kullanabilirsiniz. SDK'ya başvurmak için projenize aşağıdaki paket başvurularını ekleyin:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.8.0" />
  ```

* Java: Konuşma SDK Maven paketimizin en son sürümüne başvuruyapabilir ve kullanabilirsiniz. Maven projenizde, `https://csspeechstorage.blob.core.windows.net/maven/` bağımlılık olarak ek bir depo `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0` ve başvuru olarak ekleyin.

* C++: SDK'yı [.katran paketi](https://aka.ms/csspeech/linuxbinary) olarak indirin ve dosyaları seçtiğiniz bir dizinde boşaltın. Aşağıdaki tabloSDK klasör yapısını gösterir:

  |Yol|Açıklama|
  |-|-|
  |`license.md`|Lisans|
  |`ThirdPartyNotices.md`|Üçüncü taraf bildirimleri|
  |`include`|C ve C++ için üstbilgi dosyaları|
  |`lib/x64`|Uygulamanızla bağlantı için yerel x64 kitaplığı|
  |`lib/x86`|Uygulamanızla bağlantı için yerel x86 kitaplığı|

  Bir uygulama oluşturmak için, gerekli ikilileri (ve kitaplıkları) geliştirme ortamınıza kopyalayın veya taşıyın. Bunları yapı sürecinize gerektiği gibi ekleyin.

# <a name="android"></a>[Android](#tab/android)

Android için Java SDK, gerekli kitaplıkları ve gerekli Android izinlerini içeren bir [AAR (Android Kütüphanesi)](https://developer.android.com/studio/projects/android-library)olarak paketlenmiştir. Maven deposunda paket `https://csspeechstorage.blob.core.windows.net/maven/` `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0`olarak barındırılan.

Android Studio projenizdeki paketi tüketmek için aşağıdaki değişiklikleri yapın:

* Proje düzeyinde build.gradle dosyasında, `repository` bölüme aşağıdakileri ekleyin:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Modül düzeyinde build.gradle dosyasında, `dependencies` bölüme aşağıdakileri ekleyin:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.7.0'
  ```

Java SDK da Konuşma [Cihazları SDK](speech-devices-sdk.md)parçasıdır.

---

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C'deki konuşmayı nasıl tanıyacağınızı görme #](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
