---
author: IEvangelist
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: dapine
ms.openlocfilehash: dea6a1afaa2348fc5054bee20c534936dcafe5b5
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656702"
---
:::row:::
    :::column span="3":::
        Speech SDK, Windows 10 ve Windows Server 2016 veya sonraki sürümleri destekler. Önceki sürümler resmi olarak **desteklenmez.** Tavsiye edilmese de, Konuşma SDK'sının bölümlerini Windows'un önceki sürümleriyle kullanmak mümkündür.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Windows" src="https://docs.microsoft.com/media/logos/logo_Windows.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

### <a name="system-requirements"></a>Sistem gereksinimleri

Windows'daki Speech SDK, sistemde <a href="https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads" target="_blank">Visual Studio <span class="docon docon-navigate-external x-hidden-focus"></span> 2019 için Yeniden Dağıtılabilen Microsoft Visual C++</a> özelliğini gerektirir.

- <a href="https://aka.ms/vs/16/release/vc_redist.x86.exe" target="_blank">x86 için yükleyin<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://aka.ms/vs/16/release/vc_redist.x64.exe" target="_blank">x64 için yükleyin<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://aka.ms/vs/16/release/vc_redist.arm64.exe" target="_blank">ARMx64 için yükleyin<span class="docon docon-navigate-external x-hidden-focus"></span></a>

### <a name="c"></a>C#

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

Mikrofon girişi için Medya Vakfı kitaplıkları yüklenmelidir. Bu kitaplıklar Windows 10 ve Windows Server 2016'nın bir parçasıdır. Ses giriş aygıtı olarak mikrofon kullanılmaması sürece, bu kitaplıklar olmadan Speech SDK'yı kullanmak mümkündür.

Gerekli Konuşma SDK dosyaları uygulamanızla aynı dizinde dağıtılabilir. Bu şekilde uygulamanız kitaplıklara doğrudan erişebilir. Uygulamanızla eşleşen doğru sürümü (x86/x64) seçtiğinizden emin olun.

| Adı                                            | İşlev                                             |
|-------------------------------------------------|------------------------------------------------------|
| `Microsoft.CognitiveServices.Speech.core.dll`   | Çekirdek SDK, yerel ve yönetilen dağıtım için gerekli |
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Yönetilen dağıtım için gerekli                      |

> [!NOTE]
> Sürüm 1.3.0 ile başlayarak dosya `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (önceki sürümlerde sevk) artık gerekli değildir. İşlevsellik artık çekirdek SDK entegre edilmiştir.

> [!IMPORTANT]
> Windows Forms App (.NET Framework) C# projesi için kitaplıkların projenizin dağıtım ayarlarına dahil olduğundan emin olun. Bunu altında `Properties -> Publish Section`kontrol edebilirsiniz. `Application Files` Düğmeye tıklayın ve aşağı kaydırma listesinden ilgili kitaplıkları bulun. Değerin `Included`' e ayarlandıkundan emin olun Visual Studio, proje yayımlandığında/dağıtıldığında dosyayı içerir.

### <a name="c"></a>C++

[!INCLUDE [Get C++ Speech SDK](get-speech-sdk-cpp.md)]

### <a name="python"></a>Python

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

### <a name="java"></a>Java

[!INCLUDE [Get Java Speech SDK](get-speech-sdk-java.md)]
