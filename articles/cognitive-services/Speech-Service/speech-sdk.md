---
title: Konuşma SDK 'Sı-konuşma hizmeti hakkında
titleSuffix: Azure Cognitive Services
description: Konuşma yazılım geliştirme seti (SDK), uygulamalarınızın konuşma hizmeti işlevlerine yerel erişim sağlayarak yazılım geliştirmeyi kolaylaştırır. Bu makalede, SDK'sı için Windows, Linux ve Android hakkında ek ayrıntılar sağlar.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/13/2020
ms.author: dapine
ms.openlocfilehash: 984d2dfe07faa22756b4be167aa86a69806b1a84
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78331102"
---
# <a name="about-the-speech-sdk"></a>Konuşma SDK'sı hakkında

Konuşma Yazılım Geliştirme Seti (SDK), konuşma özellikli yazılım geliştirmeyi kolaylaştıran konuşma hizmeti işleve, uygulamaları erişmenizi sağlar. Şu anda SDK 'lar, **konuşmadan metne**, **metinden konuşmaya**, **konuşma çevirisi**, **Amaç tanıma**ve **bot çerçevesinin doğrudan hat konuşma kanalına**erişim sağlar.

Konuşmayı bir mikrofondan kolayca yakalayabilir, bir akıştan okuyabilir veya ses dosyalarına konuşma SDK 'Sı ile erişebilirsiniz. Konuşma SDK 'Sı, konuşma tanıma için WAV/PCM 16 bit, 16 kHz/8 kHz, tek kanallı ses ' i destekler. Ek ses biçimleri, [konuşmadan metne REST uç noktası](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) veya [toplu iş dökümü hizmeti](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats)kullanılarak desteklenir.

Yetenekler ve desteklenen platformlar hakkında genel bir bakış belge [giriş sayfasında](https://aka.ms/csspeech)bulunabilir.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>SDK'yı edinme

# <a name="windows"></a>[Windows](#tab/windows)

> [!WARNING]
> Konuşma SDK 'Sı Windows 10 veya sonraki sürümlerini destekler. Önceki Windows sürümleri **desteklenmez**.

Windows için aşağıdaki dilleri destekliyoruz:

* C# (UWP ve .NET), C++: başvuru ve bizim konuşma SDK'sı NuGet paketi en son sürümünü kullanın. Paket, 32-bit ve 64 bit istemci kitaplıkları ve yönetilen (.NET) kitaplıkları içerir. SDK, Visual Studio 'da NuGet, [Microsoft. Biliveservices. Speech](https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech)kullanılarak yüklenebilir.

* Java: Başvurabilir ve yalnızca Windows x64 destekleyen bizim konuşma SDK Maven paketini en son sürümünü kullanın. Maven projenizde, ek bir depo olarak `https://csspeechstorage.blob.core.windows.net/maven/` ekleyin ve bağımlılık olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.8.0` başvurun.

# <a name="linux"></a>[Linux](#tab/linux)

> [!NOTE]
> Şu anda, aşağıdaki hedef mimarilerde Ubuntu 16,04, Ubuntu 18,04, detik 9, Red Hat Enterprise Linux (RHEL) 8 ve CentOS 8 ' i destekliyoruz:
> - geliştirme için C++ x86 (DeARM32/Ubuntu), x64, (de/Ubuntu) ve ARM64 (de/Ubuntu)
> - Java için x64, ARM32 (de/Ubuntu) ve ARM64 (de/Ubuntu)
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

RHEL/CentOS 8 ' de:

```sh
sudo yum update
sudo yum install alsa-lib openssl
```

> [!NOTE]
> RHEL/CentOS 8 ' de, [Linux Için OpenSSL 'yi yapılandırma](~/articles/cognitive-services/speech-service/how-to-configure-openssl-linux.md)yönergelerini izleyin.

* C# ' ta: Başvuru ve bizim konuşma SDK'sı NuGet paketi en son sürümünü kullanın. SDK'ya başvurmak için şu paket başvuruyu projenize ekleyin:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.8.0" />
  ```

* Java: Başvurabilir ve bizim konuşma SDK Maven paketini en son sürümünü kullanın. Maven projenizde, ek bir depo olarak `https://csspeechstorage.blob.core.windows.net/maven/` ekleyin ve bağımlılık olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0` başvurun.

* C++: SDK 'Yı bir [. tar paketi](https://aka.ms/csspeech/linuxbinary) olarak indirin ve seçtiğiniz bir dizindeki dosyaları paketten çıkarın. Aşağıdaki tablo, SDK'sı klasör yapısını gösterir:

  |Yol|Açıklama|
  |-|-|
  |`license.md`|Lisans|
  |`ThirdPartyNotices.md`|Üçüncü taraf bildirimleri|
  |`include`|C ve C++ üstbilgi dosyaları|
  |`lib/x64`|Yerel x64 uygulamanızla bağlama kitaplığı|
  |`lib/x86`|Yerel x86 uygulamanızla bağlama kitaplığı|

  Bir uygulama oluşturmak için kopyalayabilir veya gerekli ikili dosyaların (ve kitaplıkları) geliştirme ortamınıza taşıyabilirsiniz. Yapı işleminiz için gereken şekilde bunları ekleyebilirsiniz.

# <a name="android"></a>[Android](#tab/android)

Android için Java SDK 'Sı, gerekli kitaplıkları ve gerekli Android izinlerini içeren bir [AAR (Android kitaplığı)](https://developer.android.com/studio/projects/android-library)olarak paketlenmiştir. Paket `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0`olarak `https://csspeechstorage.blob.core.windows.net/maven/` bir Maven deposunda barındırılır.

Android Studio projenizden paketi kullanmak için aşağıdaki değişiklikleri yapın:

* Proje düzeyi Build. Gradle dosyasında aşağıdakini `repository` bölümüne ekleyin:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Modül düzeyi Build. Gradle dosyasında aşağıdakini `dependencies` bölümüne ekleyin:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.7.0'
  ```

Java SDK 'sı Ayrıca [konuşma cihazları SDK 'sının](speech-devices-sdk.md)bir parçasıdır.

---

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [Bkz. konuşmayı tanımaC#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
