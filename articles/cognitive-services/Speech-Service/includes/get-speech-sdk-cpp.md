---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5a06a0663601c221dd456b9cf4437cb9f32a18f8
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81400036"
---
:::row:::
    :::column span="3":::
        C++ Speech SDK Windows, Linux ve macOS'ta kullanılabilir. Daha fazla bilgi için <a href="https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech" target="_blank">Microsoft.CognitiveServices.Speech <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>'e bakın.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="C++" src="https://docs.microsoft.com/media/logos/logo_Cplusplus.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

#### <a name="c-nuget-package"></a>C++ NuGet paketi

C++ Konuşma SDK aşağıdaki `Install-Package` komutu ile Paket **Yöneticisi'nden** yüklenebilir.

```powershell
Install-Package Microsoft.CognitiveServices.Speech
```

#### <a name="c-binaries-and-header-files"></a>C++ ikilileri ve üstbilgi dosyaları

Alternatif olarak, C++ Konuşma SDK ikili olarak yüklenebilir. SDK'yı <a href="https://aka.ms/csspeech/linuxbinary" target="_blank">.tar paketi <span class="docon docon-navigate-external x-hidden-focus"></span> </a> olarak indirin ve dosyaları seçtiğiniz bir dizinde boşaltın. Bu paketin içeriği (hem x86 hem de x64 hedef mimarileri için üstbilgi dosyalarını içeren) aşağıdaki gibi yapılandırılmıştır:

  | Yol                   | Açıklama                                          |
  |------------------------|------------------------------------------------------|
  | `license.md`           | Lisans                                              |
  | `ThirdPartyNotices.md` | Üçüncü taraf bildirimleri                                  |
  | `include`              | C++ için üstbilgi dosyaları                                 |
  | `lib/x64`              | Uygulamanızla bağlantı için yerel x64 kitaplığı |
  | `lib/x86`              | Uygulamanızla bağlantı için yerel x86 kitaplığı |

  Bir uygulama oluşturmak için, gerekli ikilileri (ve kitaplıkları) geliştirme ortamınıza kopyalayın veya taşıyın. Bunları yapı sürecinize gerektiği gibi ekleyin.

#### <a name="additional-resources"></a>Ek kaynaklar

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp" target="_blank">Windows, Linux ve macOS hızlı başlangıç C++ kaynak kodu<span class="docon docon-navigate-external x-hidden-focus"></span></a>