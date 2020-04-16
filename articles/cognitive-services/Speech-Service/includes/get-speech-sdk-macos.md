---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 57de7f1e7c37fec66cda666d3f144e52849a026f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81399933"
---
macOS için geliştirme yaparken, üç Konuşma SDK'sı mevcuttur.

- Objective-C Speech SDK, CocoaPod paketi olarak kullanılabilir
- .NET Speech SDK ,NET Standart 2.0 uygular gibi **Xamarin.Mac** ile kullanılabilir
- Python Konuşma SDK pypi modülü olarak kullanılabilir

> [!TIP]
> Swift ile Objective-C Speech SDK'yı kullanan ayrıntılar için, <a href="https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_objective-c_into_swift" target="_blank">Bkz. <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>

### <a name="system-requirements"></a>Sistem gereksinimleri

- MacOS sürüm 10.13 veya sonrası

# <a name="xcode"></a>[Xcode](#tab/mac-xcode)

:::row:::
    :::column span="3":::
        macOS CocoaPod paketi <a href="https://apps.apple.com/us/app/xcode/id497799835" target="_blank">Xcode 9.4.1 (veya daha sonraki) <span class="docon docon-navigate-external x-hidden-focus"></span> </a> entegre geliştirme ortamı (IDE) ile indirilebilir ve kullanılabilir. İlk olarak, <a href="https://aka.ms/csspeech/macosbinary" target="_blank">ikili <span class="docon docon-navigate-external x-hidden-focus"> </span>CocoaPod indirin. </a> Kullanım amacı için aynı dizinde bölme ayıklayın, bir *Podfile* oluşturun ve `pod` bir `target`.
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

target 'MyApp' do
  pod 'MicrosoftCognitiveServicesSpeech', '~> 1.11.0'
end
```

# <a name="xamarinmac"></a>[Xamarin.Mac](#tab/mac-xamarin)

:::row:::
    :::column span="3":::
        Xamarin.Mac, C# kullanarak yerel Mac uygulamaları oluşturmaya yönelik tüm macOS SDK’sını .NET geliştiricilerinin kullanımına sunar. Daha fazla bilgi için <a href="https://docs.microsoft.com/xamarin/mac/" target="_blank">Xamarin.Mac <span class="docon docon-navigate-external x-hidden-focus"> </span>adresine </a>bakın.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Xamarin" src="https://docs.microsoft.com/media/logos/logo_xamarin.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

# <a name="python"></a>[Python](#tab/mac-python)

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

---

#### <a name="additional-resources"></a>Ek kaynaklar

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/objectivec/macos" target="_blank">macOS Konuşma SDK quickstart Objective-C kaynak kodu<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/swift/macos" target="_blank">macOS Konuşma SDK hızlı swift kaynak kodu başlatın<span class="docon docon-navigate-external x-hidden-focus"></span></a>