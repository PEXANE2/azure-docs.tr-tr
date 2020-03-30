---
title: Azure İşgünişleri için Azure Hizmet Veri çisü
description: Azure İşgündeşme seçimi tetikleyicileri ve ciltlemelergözlerini Azure ışlarını göndermeyi öğrenin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 44e4114b328701d5de363e91f5562f1daad351c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277420"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure İşgünişleri için Azure Hizmet Veri çisü

Azure İşlevleri, [tetikleyiciler ve bağlamalar](./functions-triggers-bindings.md)aracılığıyla [Azure Hizmet Veri Servisi](https://azure.microsoft.com/services/service-bus) ile tümleşir. Hizmet Veri Servisi ile tümleştirme, tepki veren ve sıra veya konu iletileri gönderen işlevler oluşturmanıza olanak tanır.

| Eylem | Tür |
|---------|---------|
| Hizmet Veri Servisi sırası veya konu iletisi oluşturulduğunda bir işlev çalıştırma | [Tetikleyici](./functions-bindings-service-bus-trigger.md) |
| Azure Hizmet Veri Gönderi iletileri gönderme |[Çıkış bağlama](./functions-bindings-service-bus-output.md) |

## <a name="add-to-your-functions-app"></a>Fonksiyonlar uygulamanıza ekleme

### <a name="functions-2x-and-higher"></a>Fonksiyonlar 2.x ve üzeri

Tetikleyici ve bağlamalarla çalışmak, uygun pakete başvurmanızı gerektirir. NuGet paketi .NET sınıf kitaplıkları için kullanılırken, uzantı paketi diğer tüm uygulama türleri için kullanılır.

| Dil                                        | Tarafından ekle...                                   | Açıklamalar 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | [NuGet paketini]yükleme , sürüm 4.x | |
| C# Script, Java, JavaScript, Python, PowerShell | [Uzantı paketini] kaydetme          | [Azure Araçları uzantısı] Visual Studio Code ile kullanılması önerilir. |
| C# Script (yalnızca Azure portalında çevrimiçi)         | Bağlama ekleme                            | İşlev uygulamanızı yeniden yayımlamak zorunda kalmadan varolan bağlama uzantılarını güncelleştirmek için [bkz.] |

[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus/
[core tools]: ./functions-run-local.md
[uzatma paketi]: ./functions-bindings-register.md#extension-bundles
[Uzantılarınızı güncelleştirin]: ./install-update-binding-extensions-manual.md
[Azure Araçları uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

Fonksiyonlar 1.x uygulamaları otomatik olarak [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2.x bir başvuru var.

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet Veri Servisi sırası veya konu iletisi oluşturulduğunda bir işlev çalıştırma (Tetikleyici)](./functions-bindings-service-bus-trigger.md)
- [Azure İşlerinden Azure Hizmet Veri Gönderi iletileri gönderme (Çıktı bağlama)](./functions-bindings-service-bus-output.md)
