---
title: Azure İşlevleri HTTP tetikleyicileri ve bağlamalar
description: Azure İşlevleri'nde HTTP tetikleyicilerini ve bağlamalarını kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.openlocfilehash: 29b5e9c7673b4a730a41bf7cf2b1c4a2a86209ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77462114"
---
# <a name="azure-functions-http-triggers-and-bindings-overview"></a>Azure İşlevleri HTTP tetikler ve bağlamalara genel bakış

Azure Fonksiyonları, sunucusuz API'ler oluşturmak ve [webhook'lara](https://en.wikipedia.org/wiki/Webhook)yanıt vermek için HTTP istekleri aracılığıyla çağrılabilir.

| Eylem | Tür |
|---------|---------|
| BIR HTTP isteğinden bir işlev çalıştırma | [Tetikleyici](./functions-bindings-http-webhook-trigger.md) |
| Bir işlevden bir HTTP yanıtı döndürme |[Çıkış bağlama](./functions-bindings-http-webhook-output.md) |

Bu makaledeki kod, Fonksiyonlar sürüm 2.x ve üstü kullanılan .NET Core sözdizimini varsayılan olarak kullanır. 1.x sözdizimi hakkında bilgi için [1.x işlevleri şablonlarına](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates)bakın.

## <a name="add-to-your-functions-app"></a>Fonksiyonlar uygulamanıza ekleme

### <a name="functions-2x-and-higher"></a>Fonksiyonlar 2.x ve üzeri

Tetikleyici ve bağlamalarla çalışmak, uygun pakete başvurmanızı gerektirir. NuGet paketi .NET sınıf kitaplıkları için kullanılırken, uzantı paketi diğer tüm uygulama türleri için kullanılır.

| Dil                                        | Tarafından ekle...                                   | Açıklamalar 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | [NuGet paketini]yükleme , sürüm 3.x | |
| C# Script, Java, JavaScript, Python, PowerShell | [Uzantı paketini] kaydetme          | [Azure Araçları uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) Visual Studio Code ile kullanılması önerilir. |
| C# Script (yalnızca Azure portalında çevrimiçi)         | Bağlama ekleme                            | İşlev uygulamanızı yeniden yayımlamak zorunda kalmadan varolan bağlama uzantılarını güncelleştirmek için [bkz.] |

[core tools]: ./functions-run-local.md
[uzatma paketi]: ./functions-bindings-register.md#extension-bundles
[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http
[Uzantılarınızı güncelleştirin]: ./install-update-binding-extensions-manual.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

Fonksiyonlar 1.x uygulamaları otomatik olarak [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2.x bir başvuru var.

## <a name="next-steps"></a>Sonraki adımlar

- [BIR HTTP isteğinden bir işlev çalıştırma](./functions-bindings-http-webhook-trigger.md)
- [Bir işlevden bir HTTP yanıtı döndürme](./functions-bindings-http-webhook-output.md)
