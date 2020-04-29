---
title: Azure Işlevlerine genel bakış için Azure kuyruk depolama tetikleyicisi ve bağlamaları
description: Azure 'da Azure kuyruk depolama tetikleyicisi ve çıkış bağlamayı nasıl kullanacağınızı anlayın.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2020
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 71321fa631bafde5b1dbaac2d165cb68fd8116d4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79277316"
---
# <a name="azure-queue-storage-trigger-and-bindings-for-azure-functions-overview"></a>Azure Işlevlerine genel bakış için Azure kuyruk depolama tetikleyicisi ve bağlamaları

Azure Işlevleri yeni Azure kuyruğu depolama iletileri oluşturulduğu ve bir işlev içinde sıra iletilerini yazacağı için çalıştırılabilir.

| Eylem | Tür |
|---------|---------|
| Bir işlevi kuyruk depolama veri değişiklikleri olarak çalıştırma | [Tetikleyici](./functions-bindings-storage-queue-trigger.md) |
| Sıra depolama iletilerini yaz |[Çıkış bağlama](./functions-bindings-storage-queue-output.md) |

## <a name="add-to-your-functions-app"></a>Işlevler uygulamanıza ekleme

### <a name="functions-2x-and-higher"></a>İşlevler 2. x ve üzeri

Tetikleyici ve bağlamalarla çalışma, uygun pakete başvurmanız gerekir. Diğer tüm uygulama türleri için Uzantı paketi kullanıldığında, .NET sınıf kitaplıkları için NuGet paketi kullanılır.

| Dil                                        | Ekleme ölçütü...                                   | Açıklamalar 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | [NuGet paketi]yükleniyor, sürüm 3. x | |
| C# betiği, Java, JavaScript, Python, PowerShell | [Uzantı paketi] kaydediliyor          | [Azure Araçları uzantısının](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) Visual Studio Code ile kullanılması önerilir. |
| C# betiği (yalnızca çevrimiçi-Azure portal)         | Bağlama ekleme                            | İşlev uygulamanızı yeniden yayımlamak zorunda kalmadan mevcut bağlama uzantılarını güncelleştirmek için bkz. [uzantılarınızı güncelleştirme]. |

[core tools]: ./functions-run-local.md
[Uzantı paketi]: ./functions-bindings-register.md#extension-bundles
[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage
[Uzantılarınızı güncelleştirme]: ./install-update-binding-extensions-manual.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

1. x uygulamalarının [Microsoft. Azure. WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketine, sürüm 2. x 'e başvuran bir başvurusu otomatik olarak vardır.

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Bir işlevi kuyruk depolama veri değişiklikleri (tetikleyici) olarak çalıştırma](./functions-bindings-storage-queue-trigger.md)
- [Yazma kuyruğu depolama iletileri (çıkış bağlaması)](./functions-bindings-storage-queue-output.md)
