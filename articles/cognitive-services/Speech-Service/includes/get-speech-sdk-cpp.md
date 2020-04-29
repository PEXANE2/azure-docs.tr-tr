---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 5a06a0663601c221dd456b9cf4437cb9f32a18f8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81400036"
---
:::row:::
    :::column span="3":::
        C++ konuşma SDK 'Sı Windows, Linux ve macOS 'ta kullanılabilir. Daha fazla bilgi için bkz. <a href="https://www.nuget.org/packages/Microsoft.CognitiveServices.Speech" target="_blank">Microsoft. Biliveservices. <span class="docon docon-navigate-external x-hidden-focus"> </span>Speech </a>.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="C++" src="https://docs.microsoft.com/media/logos/logo_Cplusplus.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

#### <a name="c-nuget-package"></a>C++ NuGet paketi

C++ konuşma SDK 'Sı, aşağıdaki `Install-Package` komutla **paket yöneticisinden** yüklenebilir.

```powershell
Install-Package Microsoft.CognitiveServices.Speech
```

#### <a name="c-binaries-and-header-files"></a>C++ ikili dosyaları ve üst bilgi dosyaları

Alternatif olarak, C++ konuşma SDK ikililerini de yüklenebilir. SDK 'yı bir <a href="https://aka.ms/csspeech/linuxbinary" target="_blank">. tar paketi <span class="docon docon-navigate-external x-hidden-focus"></span> </a> olarak indirin ve seçtiğiniz bir dizindeki dosyaları paketten çıkarın. Bu paketin içeriği (hem x86 hem de x64 hedef mimarilerinin üst bilgi dosyalarını içerir) aşağıdaki gibi yapılandırılır:

  | Yol                   | Açıklama                                          |
  |------------------------|------------------------------------------------------|
  | `license.md`           | Lisans                                              |
  | `ThirdPartyNotices.md` | Üçüncü taraf bildirimleri                                  |
  | `include`              | C++ için üst bilgi dosyaları                                 |
  | `lib/x64`              | Uygulamanızla bağlantı için yerel x64 kitaplığı |
  | `lib/x86`              | Uygulamanızla bağlantı için yerel x86 kitaplığı |

  Bir uygulama oluşturmak için gerekli ikili dosyaları (ve kitaplıkları) geliştirme ortamınıza kopyalayın veya taşıyın. Bunları yapı sürecinizi gerektiği şekilde ekleyin.

#### <a name="additional-resources"></a>Ek kaynaklar

- <a href="https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/quickstart/cpp" target="_blank">Windows, Linux ve macOS hızlı başlangıç C++ kaynak kodu<span class="docon docon-navigate-external x-hidden-focus"></span></a>