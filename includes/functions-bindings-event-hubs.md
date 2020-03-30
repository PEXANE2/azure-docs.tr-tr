---
author: craigshoemaker
ms.service: azure-functions
ms.topic: include
ms.date: 02/21/2020
ms.author: cshoe
ms.openlocfilehash: 47e9e37676a4afa9ec29393bf970c368f3f9e5be
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77586146"
---
## <a name="add-to-your-functions-app"></a>Fonksiyonlar uygulamanıza ekleme

### <a name="functions-2x-and-higher"></a>Fonksiyonlar 2.x ve üzeri

Tetikleyici ve bağlamalarla çalışmak, uygun pakete başvurmanızı gerektirir. NuGet paketi .NET sınıf kitaplıkları için kullanılırken, uzantı paketi diğer tüm uygulama türleri için kullanılır.

| Dil                                        | Tarafından ekle...                                   | Açıklamalar 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | [NuGet paketini]yükleme , sürüm 3.x | |
| C# Script, Java, JavaScript, Python, PowerShell | [Uzantı paketini] kaydetme          | [Azure Araçları uzantısı] Visual Studio Code ile kullanılması önerilir. |
| C# Script (yalnızca Azure portalında çevrimiçi)         | Bağlama ekleme                            | İşlev uygulamanızı yeniden yayımlamak zorunda kalmadan varolan bağlama uzantılarını güncelleştirmek için [bkz.] |

[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs
[core tools]: ../articles/azure-functions/functions-run-local.md
[uzatma paketi]: ../articles/azure-functions/functions-bindings-register.md#extension-bundles
[Uzantılarınızı güncelleştirin]: ../articles/azure-functions/install-update-binding-extensions-manual.md
[Azure Araçları uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

Fonksiyonlar 1.x uygulamaları otomatik olarak [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2.x bir başvuru var.