---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 82c45919892721d689bd90b7480158c4eea16c03
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87374787"
---
MacOS için geliştirme yaparken, kullanılabilir üç konuşma SDK 'Sı vardır.

- Amaç-C konuşma SDK 'Sı yerel olarak bir CocoaPod paketi olarak kullanılabilir
- .NET konuşma SDK 'Sı, .NET Standard 2,0 uyguladığı **Xamarin. Mac** ile birlikte kullanılabilir
- Python konuşma SDK 'Sı, PyPI modülü olarak kullanılabilir

> [!TIP]
> Swift ile amaç-C konuşma SDK 'sını kullanan Ayrıntılar için bkz. <a href="https://developer.apple.com/documentation/swift/imported_c_and_objective-c_apis/importing_objective-c_into_swift" target="_blank">hedefi Swift <span class="docon docon-navigate-external x-hidden-focus"></span> 'a alma-c </a>.

### <a name="system-requirements"></a>Sistem gereksinimleri

- MacOS sürüm 10,13 veya üzeri

# <a name="xcode"></a>[Xcode](#tab/mac-xcode)

:::row:::
    :::column span="3":::
        MacOS CocoaPod paketi, <a href="https://apps.apple.com/us/app/xcode/id497799835" target="_blank">Xcode 9.4.1 (veya üzeri) <span class="docon docon-navigate-external x-hidden-focus"></span> </a> tümleşik GELIŞTIRME ortamı (IDE) ile indirilebilir ve kullanılabilir. İlk olarak, <a href="https://aka.ms/csspeech/macosbinary" target="_blank">CocoaPod <span class="docon docon-navigate-external x-hidden-focus"></span> ikili dosyasını indirin </a>. Kendisini amaçlanan kullanım için aynı dizinde ayıklayın, *pod dosyası* oluşturun ve `pod` olarak listeleyin `target` .
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
  pod 'MicrosoftCognitiveServicesSpeech', '~> 1.13.0'
end
```

# <a name="xamarinmac"></a>[Xamarin.Mac](#tab/mac-xamarin)

:::row:::
    :::column span="3":::
        Xamarin.Mac, C# kullanarak yerel Mac uygulamaları oluşturmaya yönelik tüm macOS SDK’sını .NET geliştiricilerinin kullanımına sunar. Daha fazla bilgi için bkz. <a href="https://docs.microsoft.com/xamarin/mac/" target="_blank">Xamarin. <span class="docon docon-navigate-external x-hidden-focus"></span> Mac </a>.
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

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/objectivec/macos" target="_blank">macOS konuşma SDK hızlı başlangıç hedefi-C kaynak kodu<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/swift/macos" target="_blank">macOS konuşma SDK hızlı başlangıç Swift kaynak kodu<span class="docon docon-navigate-external x-hidden-focus"></span></a>