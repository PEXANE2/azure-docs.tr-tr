---
title: Azure Işlevleri için Azure Blob depolama tetikleyicisi ve bağlamaları
description: Azure Işlevleri 'nde Azure Blob depolama tetikleyiciyi ve bağlamalarını kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/13/2020
ms.author: cshoe
ms.openlocfilehash: eead2d6ae2860bf06f1b878c1bb1692d74c06df6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87055972"
---
# <a name="azure-blob-storage-bindings-for-azure-functions-overview"></a>Azure Işlevlerine yönelik Azure Blob depolama bağlamaları genel bakış

Azure Işlevleri [, Tetikleyiciler ve bağlamalar](./functions-triggers-bindings.md)aracılığıyla [Azure depolama](../storage/index.yml) ile tümleşir. BLOB depolama ile tümleştirme, blob verilerinde değişikliklere tepki veren işlevler ve okuma ve yazma değerlerini oluşturmanızı sağlar.

| Eylem | Tür |
|---------|---------|
| BLOB depolama veri değişiklikleri olarak bir işlev çalıştırma | [Tetikleyici](./functions-bindings-storage-blob-trigger.md) |
| Bir işlevde BLOB depolama verilerini okuma | [Giriş bağlama](./functions-bindings-storage-blob-input.md) |
| Bir işlevin BLOB depolama verilerini yazmasına izin ver |[Çıkış bağlama](./functions-bindings-storage-blob-output.md) |

## <a name="add-to-your-functions-app"></a>Işlevler uygulamanıza ekleme

### <a name="functions-2x-and-higher"></a>İşlevler 2.x ve üzeri

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

- [BLOB depolama verileri değiştiğinde bir işlev Çalıştır](./functions-bindings-storage-blob-trigger.md)
- [Bir işlev çalıştığında BLOB depolama verilerini okuma](./functions-bindings-storage-blob-input.md)
- [Bir işlevden BLOB depolama verileri yazma](./functions-bindings-storage-blob-output.md)
