---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.custom: devx-track-csharp
ms.openlocfilehash: fc4cc5063f72ff3f0db62cde79f7908add86166e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102434601"
---
:::row:::
    :::column span="3":::
        Konuşma SDK 'Sı Windows 10 ve Windows Server 2016 veya sonraki sürümlerini destekler. Önceki **sürümler resmi olarak desteklenmez.** Konuşma SDK 'sının parçalarını Windows 'un önceki sürümleriyle kullanmak önerilmese de bu önerilmez.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Windows" src="https://docs.microsoft.com/media/logos/logo_Windows.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

### <a name="system-requirements"></a>Sistem Gereksinimleri

Windows üzerinde konuşma SDK 'Sı, sistemde <a href="https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads" target="_blank">Visual Studio 2019 Için yeniden dağıtılabilir Microsoft Visual C++ </a> gerektirir.

- <a href="https://aka.ms/vs/16/release/vc_redist.x86.exe" target="_blank">X86 için Install </a>
- <a href="https://aka.ms/vs/16/release/vc_redist.x64.exe" target="_blank">X64 için yüklensin </a>
- <a href="https://aka.ms/vs/16/release/vc_redist.arm64.exe" target="_blank">ARMx64 için Install </a>

### <a name="c"></a>C#

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

Mikrofon girişi için Medya Altyapısı kitaplıklarının yüklenmesi gerekir. Bu kitaplıklar Windows 10 ve Windows Server 2016 ' in bir parçasıdır. Ses giriş cihazı olarak bir mikrofon kullanıldığı sürece, bu kitaplıklar olmadan konuşma SDK 'sını kullanmak mümkündür.

Gerekli konuşma SDK dosyaları, uygulamanızla aynı dizine dağıtılabilir. Bu şekilde, uygulamanız kitaplıklara doğrudan erişim sağlayabilir. Uygulamanızla eşleşen doğru sürümü (x86/x64) seçtiğinizden emin olun.

| Name                                            | İşlev                                             |
|-------------------------------------------------|------------------------------------------------------|
| `Microsoft.CognitiveServices.Speech.core.dll`   | Yerel ve yönetilen dağıtım için gerekli olan temel SDK |
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Yönetilen dağıtım için gerekli                      |

> [!NOTE]
> 1.3.0 sürümünden itibaren `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (önceki sürümlerde sunulan) dosya artık gerekli değildir. İşlevselliği artık çekirdek SDK 'sında tümleşiktir.

> [!IMPORTANT]
> Windows Forms App (.NET Framework) C# projesi için, kitaplıkların projenizin dağıtım ayarlarına eklendiğinden emin olun. Bunu altında kontrol edebilirsiniz `Properties -> Publish Section` . Düğmeye tıklayın `Application Files` ve aşağı kaydırma listesinden ilgili kitaplıkları bulun. Değerin olarak ayarlandığından emin olun `Included` . Proje yayımlandığında/dağıtıldığında, Visual Studio dosyayı içerecektir.

### <a name="c"></a>C++

[!INCLUDE [Get C++ Speech SDK](get-speech-sdk-cpp.md)]

### <a name="python"></a>Python

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

### <a name="java"></a>Java

[!INCLUDE [Get Java Speech SDK](get-speech-sdk-java.md)]
