---
title: Konuşma SDK 'Sı-konuşma hizmeti hakkında
titleSuffix: Azure Cognitive Services
description: Konuşma yazılım geliştirme seti (SDK), uygulamalarınızın konuşma hizmeti işlevlerine yerel erişim sağlayarak yazılım geliştirmeyi kolaylaştırır. Bu makalede, Windows, Linux ve Android SDK hakkında ek ayrıntılar sağlanmaktadır.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: ee505cc78c16d85640c7e13541272f1bafe30061
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71799995"
---
# <a name="about-the-speech-sdk"></a>Konuşma SDK 'Sı hakkında

Konuşma yazılım geliştirme seti (SDK), uygulamalarınızın konuşma Hizmetleri işlevlerine erişmesini sağlayarak konuşma özellikli yazılım geliştirmeyi kolaylaştırır. Şu anda SDK 'lar, **konuşmadan metne**, **metinden konuşmaya**, **konuşma çevirisi**, **Amaç tanıma**ve **bot çerçevesinin doğrudan hat konuşma kanalına**erişim sağlar. Yetenekler ve desteklenen platformlar hakkında genel bir bakış belge [giriş sayfasında](https://aka.ms/csspeech)bulunabilir.

[!INCLUDE [Speech SDK Platforms](../../../includes/cognitive-services-speech-service-speech-sdk-platforms.md)]

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

## <a name="get-the-sdk"></a>SDK 'Yı edinme

### <a name="windows"></a>Windows

Windows için aşağıdaki dilleri destekliyoruz:

* C#(UWP ve .NET) C++: konuşma SDK 'sı NuGet paketimizin en son sürümünü başvurabilir ve kullanabilirsiniz. Pakette 32-bit ve 64 bit istemci kitaplıkları ve yönetilen (.NET) kitaplıkları bulunur. SDK, Visual Studio 'da NuGet kullanılarak yüklenebilir. **Microsoft. Biliveservices. Speech**için arama yapın.

* Java: yalnızca Windows x64 destekleyen konuşma SDK Maven paketimizin en son sürümünü kullanabilirsiniz. Maven projenizde, ek bir depo olarak `https://csspeechstorage.blob.core.windows.net/maven/` ve bir bağımlılık olarak-1 @no__t başvurusu ekleyin.

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

* C#: Konuşma SDK 'Sı NuGet paketimizin en son sürümünü başvurabilir ve kullanabilirsiniz. SDK 'ya başvurmak için, aşağıdaki paket başvurusunu projenize ekleyin:

  ```xml
  <PackageReference Include="Microsoft.CognitiveServices.Speech" Version="1.7.0" />
  ```

* Java: konuşma SDK Maven paketinizin en son sürümünü başvuru yapabilir ve kullanabilirsiniz. Maven projenizde, ek bir depo olarak `https://csspeechstorage.blob.core.windows.net/maven/` ve bir bağımlılık olarak-1 @no__t başvurusu ekleyin.

* C++: SDK 'Yı bir [. tar paketi](https://aka.ms/csspeech/linuxbinary) olarak indirin ve seçtiğiniz bir dizindeki dosyaları paketten çıkarın. Aşağıdaki tabloda SDK klasör yapısı gösterilmektedir:

  |Yol|Açıklama|
  |-|-|
  |`license.md`|Lisans|
  |`ThirdPartyNotices.md`|Üçüncü taraf bildirimleri|
  |`include`|C ve için üst bilgi dosyalarıC++|
  |`lib/x64`|Uygulamanızla bağlantı için yerel x64 kitaplığı|
  |`lib/x86`|Uygulamanızla bağlantı için yerel x86 kitaplığı|

  Bir uygulama oluşturmak için gerekli ikili dosyaları (ve kitaplıkları) geliştirme ortamınıza kopyalayın veya taşıyın. Bunları yapı sürecinizi gerektiği şekilde ekleyin.

### <a name="android"></a>Android

Android için Java SDK 'Sı, gerekli kitaplıkları ve gerekli Android izinlerini içeren bir [AAR (Android kitaplığı)](https://developer.android.com/studio/projects/android-library)olarak paketlenmiştir. @No__t-0 ' daki bir Maven deposunda barındırılıyor `com.microsoft.cognitiveservices.speech:client-sdk:1.7.0`.

Android Studio projenizden paketi kullanmak için aşağıdaki değişiklikleri yapın:

* Proje düzeyi Build. Gradle dosyasında, `repository` bölümüne aşağıdakini ekleyin:

  ```gradle
  maven { url 'https://csspeechstorage.blob.core.windows.net/maven/' }
  ```

* Modül düzeyi Build. Gradle dosyasında, `dependencies` bölümüne aşağıdakini ekleyin:

  ```gradle
  implementation 'com.microsoft.cognitiveservices.speech:client-sdk:1.7.0'
  ```

Java SDK 'sı Ayrıca [konuşma cihazları SDK 'sının](speech-devices-sdk.md)bir parçasıdır.

[!INCLUDE [Get the samples](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Konuşma deneme aboneliğinizi alın](https://azure.microsoft.com/try/cognitive-services/)
* [Bkz. konuşmayı tanımaC#](quickstart-csharp-dotnet-windows.md)
