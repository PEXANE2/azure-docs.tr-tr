---
title: Azure Işlevleri HTTP Tetikleyicileri ve bağlamaları
description: Azure Işlevleri 'nde HTTP Tetikleyicileri ve bağlamaları kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.openlocfilehash: ebf0728184a5fc104e3e1e7d8a199fec328dbdc0
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210183"
---
# <a name="azure-functions-http-triggers-and-bindings-overview"></a>Azure Işlevleri HTTP Tetikleyicileri ve bağlamaları genel bakış

Azure Işlevleri, sunucusuz API 'Ler oluşturmak ve [Web kancalarına](https://en.wikipedia.org/wiki/Webhook)yanıt vermek için http istekleri aracılığıyla çağrılabilir.

| Eylem | Tür |
|---------|---------|
| HTTP isteğinden bir işlev çalıştırma | [Tetikleyici](./functions-bindings-http-webhook-trigger.md) |
| Bir işlevden HTTP yanıtı döndürme |[Çıkış bağlama](./functions-bindings-http-webhook-output.md) |

Bu makaledeki kod, sürüm 2. x ve üzeri Işlevlerde kullanılan .NET Core söz dizimini varsayılan olarak kullanır. 1\. x sözdizimi hakkında daha fazla bilgi için, bkz. [1. x işlevleri şablonları](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates).

## <a name="add-to-your-functions-app"></a>Işlevler uygulamanıza ekleme

### <a name="functions-2x-and-higher"></a>İşlevler 2. x ve üzeri

Tetikleyici ve bağlamalarla çalışma, uygun pakete başvurmanız gerekir. Uzantı diğer tüm uygulama türlerini paketlarken, bir NuGet paketi .NET sınıf kitaplıkları için kullanılır.

| Dil                                        | Ekleme ölçütü...                                   | Açıklamalar 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | [NuGet paketi]yükleniyor, sürüm 3. x | |
| C#Betik, Java, JavaScript, Python, PowerShell | [Uzantı paketi] kaydediliyor          | [Azure Araçları uzantısının](https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack) Visual Studio Code ile kullanılması önerilir. |
| C#Betik (yalnızca çevrimiçi-Azure portal)         | Bağlama ekleme                            | İşlev uygulamanızı yeniden yayımlamak zorunda kalmadan mevcut bağlama uzantılarını güncelleştirmek için bkz. [uzantılarınızı güncelleştirme]. |

[core tools]: ./functions-run-local.md
[Uzantı paketi]: ./functions-bindings-register.md#extension-bundles
[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http
[Uzantılarınızı güncelleştirme]: ./install-update-binding-extensions-manual.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

1\. x uygulamalarının [Microsoft. Azure. WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketine, sürüm 2. x 'e başvuran bir başvurusu otomatik olarak vardır.

## <a name="next-steps"></a>Sonraki adımlar

- [HTTP isteğinden bir işlev çalıştırma](./functions-bindings-http-webhook-trigger.md)
- [Bir işlevden HTTP yanıtı döndürme](./functions-bindings-http-webhook-output.md)
