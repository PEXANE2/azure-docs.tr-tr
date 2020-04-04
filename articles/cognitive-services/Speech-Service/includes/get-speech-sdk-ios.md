---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: dapine
ms.openlocfilehash: 1f3864b28ba6f5bdbffe6601d059a21328d56a5e
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656482"
---
:::row:::
    :::column span="3":::
        iOS için geliştirme yaparken, iki Konuşma SDK'sı kullanılabilir. Objective-C Speech SDK yerli olarak iOS CocoaPod paketi olarak mevcuttur. Alternatif olarak, .NET Speech SDK ,NET Standard 2.0'ı uygularken Xamarin.iOS ile kullanılabilir.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="iOS" src="https://docs.microsoft.com/media/logos/logo_ios.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

> [!TIP]
> Swift ile Objective-C Speech SDK'yı kullanan ayrıntılar için, <a href="https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_objective-c_into_swift" target="_blank">Bkz. <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>

### <a name="system-requirements"></a>Sistem gereksinimleri

- macOS sürüm 10.3 veya sonrası
- Hedef iOS 9.3 veya sonraki

# <a name="xcode"></a>[Xcode](#tab/ios-xcode)

:::row:::
    :::column span="3":::
        iOS CocoaPod paketi <a href="https://apps.apple.com/us/app/xcode/id497799835" target="_blank">Xcode 9.4.1 (veya daha sonraki) <span class="docon docon-navigate-external x-hidden-focus"></span> </a> entegre geliştirme ortamı (IDE) ile indirilebilir ve kullanılabilir. İlk olarak, <a href="https://aka.ms/csspeech/iosbinary" target="_blank">ikili <span class="docon docon-navigate-external x-hidden-focus"> </span>CocoaPod indirin. </a> Kullanım amacı için aynı dizinde bölme ayıklayın, bir *Podfile* oluşturun ve `pod` bir `target`.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Xcode" src="https://docs.microsoft.com/media/logos/logo_xcode.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

```
platform :ios, '9.3'
use_frameworks!

target 'AppName' do
  pod 'MicrosoftCognitiveServicesSpeech', '~> 1.10.0'
end
```

# <a name="xamarinios"></a>[Xamarin.iOS](#tab/ios-xamarin)

:::row:::
    :::column span="3":::
        Xamarin.iOS, .NET geliştiricileri için eksiksiz iOS SDK’sını sunar. Visual Studio’da C# veya F# kullanarak tamamen yerel iOS uygulamaları oluşturun. Daha fazla bilgi için <a href="https://docs.microsoft.com/xamarin/ios/" target="_blank">Bkz. Xamarin.iOS <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Xamarin" src="https://docs.microsoft.com/media/logos/logo_xamarin.svg" width="60px">
            &nbsp;❤️ &nbsp;        <img alt="iOS" src="https://docs.microsoft.com/media/logos/logo_ios.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

---

#### <a name="additional-resources"></a>Ek kaynaklar

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/objectivec/ios" target="_blank">iOS Konuşma SDK quickstart Objective-C kaynak kodu<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/swift/ios" target="_blank">iOS Konuşma SDK hızlı swift kaynak kodu başlatın<span class="docon docon-navigate-external x-hidden-focus"></span></a>