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
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: c5e12812a4305493be2cdc234946796b21dd26d8
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68558941"
---
# <a name="about-the-speech-sdk"></a>Konuşma SDK 'Sı hakkında

Konuşma yazılım geliştirme seti (SDK), uygulamalarınızın konuşma Hizmetleri işlevlerine erişmesini sağlayarak konuşma özellikli yazılım geliştirmeyi kolaylaştırır. Şu anda SDK 'lar, **konuşmadan metne**, **metinden konuşmaya**, **konuşma çevirisi**, **Amaç tanıma**ve **bot çerçevesinin doğrudan hat konuşma kanalına**erişim sağlar. Yetenekler ve desteklenen platformlar hakkında genel bir bakış belge [giriş sayfasında](https://aka.ms/csspeech)bulunabilir.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>SDK'yı edinme

### <a name="windows"></a>Windows

Windows için aşağıdaki dilleri destekliyoruz:

* C#(UWP ve .NET), C++: Konuşma SDK 'Sı NuGet paketimizin en son sürümünü başvurabilir ve kullanabilirsiniz. Paket, 32-bit ve 64 bit istemci kitaplıkları ve yönetilen (.NET) kitaplıkları içerir. SDK, Visual Studio'da NuGet kullanarak yüklenebilir. Arama **Microsoft.CognitiveServices.Speech**.

* Java: Yalnızca Windows x64 desteği olan konuşma SDK Maven paketinizin en son sürümünü başvurabilir ve kullanabilirsiniz. Maven projenizde ekleyin `https://csspeechstorage.blob.core.windows.net/maven/` bir ek bir depo ve başvuru olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.6.0` bağımlılık olarak.

### <a name="linux"></a>Linux

> [!NOTE]
> Şu anda, bir bılgısayarda yalnızca Ubuntu 16,04, Ubuntu 18,04 ve detik 9 ' u destekliyoruz (.NET Core, C++ Java ve Python için x86 veya x64 geliştirme ve x64 için x64).

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

* C# İÇİN: Konuşma SDK 'Sı NuGet paketimizin en son sürümünü başvurabilir ve kullanabilirsiniz. SDK'ya başvurmak için şu paket başvuruyu projenize ekleyin:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.6.0" />
  ```

* Java: Konuşma SDK Maven paketinizin en son sürümünü başvurabilir ve kullanabilirsiniz. Maven projenizde ekleyin `https://csspeechstorage.blob.core.windows.net/maven/` bir ek bir depo ve başvuru olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.6.0` bağımlılık olarak.

* C++: SDK 'Yı bir [. tar paketi](https://aka.ms/csspeech/linuxbinary) olarak indirin ve seçtiğiniz bir dizindeki dosyaları paketten çıkarın. Aşağıdaki tablo, SDK'sı klasör yapısını gösterir:

  |Yol|Açıklama|
  |-|-|
  |`license.md`|Lisans|
  |`ThirdPartyNotices.md`|Üçüncü taraf bildirimleri|
  |`include`|C ve C++ üstbilgi dosyaları|
  |`lib/x64`|Yerel x64 uygulamanızla bağlama kitaplığı|
  |`lib/x86`|Yerel x86 uygulamanızla bağlama kitaplığı|

  Bir uygulama oluşturmak için kopyalayabilir veya gerekli ikili dosyaların (ve kitaplıkları) geliştirme ortamınıza taşıyabilirsiniz. Yapı işleminiz için gereken şekilde bunları ekleyebilirsiniz.

### <a name="android"></a>Android

Android için Java SDK 'Sı, gerekli kitaplıkları ve gerekli Android izinlerini içeren bir [AAR (Android kitaplığı)](https://developer.android.com/studio/projects/android-library)olarak paketlenmiştir. Maven deponun barındırılan `https://csspeechstorage.blob.core.windows.net/maven/` paketi olarak `com.microsoft.cognitiveservices.speech:client-sdk:1.6.0`.

Android Studio projenizden paketi kullanmak için aşağıdaki değişiklikleri yapın:

* Proje düzeyi build.gradle dosyasına ekleyin `repository` bölümü:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Modül düzeyi build.gradle dosyasına ekleyin `dependencies` bölümü:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.6.0'
  ```

Java SDK'sını da olan parçası [konuşma cihaz SDK'sı](speech-devices-sdk.md).

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [C# ' de Konuşma tanıma öğrenin](quickstart-csharp-dotnet-windows.md)
