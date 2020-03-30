---
title: Fonksiyonlar için Azure Cosmos DB bağlamaları 2.x
description: Azure İşlevlerinde Azure Cosmos DB tetikleyicilerinin ve bağlamalarının nasıl kullanılacağını öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/24/2017
ms.author: cshoe
ms.openlocfilehash: f258a7aff52796a53540706bc8413575d63c9e7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77605766"
---
# <a name="azure-cosmos-db-trigger-and-bindings-for-azure-functions-2x-overview"></a>Azure İşlevleri 2.x genel bakış için Azure Cosmos DB tetikleyici setetik ve bağlamalar

> [!div class="op_single_selector" title1="Kullandığınız Azure İşlevleri çalışma zamanı sürümünü seçin: "]
> * [Sürüm 1](functions-bindings-cosmosdb.md)
> * [Sürüm 2](functions-bindings-cosmosdb-v2.md)

Bu makale kümesi, Azure İşlevler 2.x'teki [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md) bağlamalarıyla nasıl çalışılabildiğini açıklar. Azure Fonksiyonları, Azure Cosmos DB için tetikleyici, giriş ve çıktı bağlamaları destekler.

| Eylem | Tür |
|---------|---------|
| Azure Cosmos DB belgesi oluşturulduğunda veya değiştirildiğinde bir işlev çalıştırma | [Tetikleyici](./functions-bindings-cosmosdb-v2-trigger.md) |
| Azure Cosmos DB belgesini okuma | [Giriş bağlama](./functions-bindings-cosmosdb-v2-input.md) |
| Azure Cosmos DB belgesinde değişiklikleri kaydetme  |[Çıkış bağlama](./functions-bindings-cosmosdb-v2-output.md) |

> [!NOTE]
> Bu başvuru [Azure İşlevler sürüm 2.x](functions-versions.md)içindir.  Bu bağlamaların İşlev 1.x'te nasıl kullanılacağı hakkında bilgi için Azure [İşlevler 1.x için Azure Cosmos DB bağlamalarına](functions-bindings-cosmosdb.md)bakın.
>
> Bu bağlama nın adı documentDB idi. Fonksiyonlar sürüm 2.x'te, tetikleyici, bağlamalar ve paketin tümü Cosmos DB olarak adlandırılır.

## <a name="supported-apis"></a>Desteklenen API'ler

[!INCLUDE [SQL API support only](../../includes/functions-cosmosdb-sqlapi-note.md)]

## <a name="add-to-your-functions-app"></a>Fonksiyonlar uygulamanıza ekleme

### <a name="functions-2x-and-higher"></a>Fonksiyonlar 2.x ve üzeri

Tetikleyici ve bağlamalarla çalışmak, uygun pakete başvurmanızı gerektirir. NuGet paketi .NET sınıf kitaplıkları için kullanılırken, uzantı paketi diğer tüm uygulama türleri için kullanılır.

| Dil                                        | Tarafından ekle...                                   | Açıklamalar 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | [NuGet paketini]yükleme , sürüm 3.x | |
| C# Script, Java, JavaScript, Python, PowerShell | [Uzantı paketini] kaydetme          | [Azure Araçları uzantısı] Visual Studio Code ile kullanılması önerilir. |
| C# Script (yalnızca Azure portalında çevrimiçi)         | Bağlama ekleme                            | İşlev uygulamanızı yeniden yayımlamak zorunda kalmadan varolan bağlama uzantılarını güncelleştirmek için [bkz.] |

[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB
[core tools]: ./functions-run-local.md
[uzatma paketi]: ./functions-bindings-register.md#extension-bundles
[Uzantılarınızı güncelleştirin]: ./install-update-binding-extensions-manual.md
[Azure Araçları uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

Fonksiyonlar 1.x uygulamaları otomatik olarak [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2.x bir başvuru var.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Cosmos DB belgesi oluşturulduğunda veya değiştirildiğinde bir işlev çalıştırma (Tetikleyici)](./functions-bindings-cosmosdb-v2-trigger.md)
- [Azure Cosmos DB belgesini okuma (Giriş bağlama)](./functions-bindings-cosmosdb-v2-input.md)
- [Azure Cosmos DB belgesinde değişiklikleri kaydetme (Çıktı bağlama)](./functions-bindings-cosmosdb-v2-output.md)
