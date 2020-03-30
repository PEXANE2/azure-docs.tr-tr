---
title: Azure İşlevleri için Azure Sıra depolama tetikleyicisi ve bağlamalara genel bakış
description: Azure İşlevlerinde Azure Kuyruk depolama tetikleyicisinin ve çıktı bağlamanın nasıl kullanılacağını öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2020
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 71321fa631bafde5b1dbaac2d165cb68fd8116d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277316"
---
# <a name="azure-queue-storage-trigger-and-bindings-for-azure-functions-overview"></a>Azure İşlevleri için Azure Sıra depolama tetikleyicisi ve bağlamalara genel bakış

Azure İşlevleri, yeni Azure Kuyruk depolama iletileri oluşturuldukça çalışabilir ve bir işlev içinde sıra iletileri yazabilir.

| Eylem | Tür |
|---------|---------|
| Sıra depolama verisi değiştikçe bir işlev çalıştırma | [Tetikleyici](./functions-bindings-storage-queue-trigger.md) |
| Sıra depolama iletileri yazma |[Çıkış bağlama](./functions-bindings-storage-queue-output.md) |

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
[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage
[Uzantılarınızı güncelleştirin]: ./install-update-binding-extensions-manual.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

Fonksiyonlar 1.x uygulamaları otomatik olarak [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2.x bir başvuru var.

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Sıra depolama verisi değişiklikleri olarak bir işlev çalıştırma (Tetikleyici)](./functions-bindings-storage-queue-trigger.md)
- [Sıra depolama iletileri yazma (Çıktı bağlama)](./functions-bindings-storage-queue-output.md)
