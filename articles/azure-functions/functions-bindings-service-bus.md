---
title: Azure Işlevleri için Azure Service Bus bağlamaları
description: Azure Işlevlerinde Azure Service Bus Tetikleyicileri ve bağlamaları gönderme hakkında bilgi edinin.
author: craigshoemaker
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.topic: reference
ms.date: 02/19/2020
ms.author: cshoe
ms.openlocfilehash: 44e4114b328701d5de363e91f5562f1daad351c6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79277420"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure Işlevleri için Azure Service Bus bağlamaları

Azure Işlevleri [, Tetikleyiciler ve bağlamalar](./functions-triggers-bindings.md)aracılığıyla [Azure Service Bus](https://azure.microsoft.com/services/service-bus) ile tümleşir. Service Bus tümleştirme, kuyruk veya konu iletilerine tepki veren ve gönderen işlevler oluşturmanızı sağlar.

| Eylem | Tür |
|---------|---------|
| Service Bus kuyruğu veya konu iletisi oluşturulduğunda bir işlev Çalıştır | [Tetikleyici](./functions-bindings-service-bus-trigger.md) |
| Azure Service Bus iletileri gönder |[Çıkış bağlama](./functions-bindings-service-bus-output.md) |

## <a name="add-to-your-functions-app"></a>Işlevler uygulamanıza ekleme

### <a name="functions-2x-and-higher"></a>İşlevler 2. x ve üzeri

Tetikleyici ve bağlamalarla çalışma, uygun pakete başvurmanız gerekir. Diğer tüm uygulama türleri için Uzantı paketi kullanıldığında, .NET sınıf kitaplıkları için NuGet paketi kullanılır.

| Dil                                        | Ekleme ölçütü...                                   | Açıklamalar 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | [NuGet paketi]yükleniyor, sürüm 4. x | |
| C# betiği, Java, JavaScript, Python, PowerShell | [Uzantı paketi] kaydediliyor          | [Azure Araçları uzantısının] Visual Studio Code ile kullanılması önerilir. |
| C# betiği (yalnızca çevrimiçi-Azure portal)         | Bağlama ekleme                            | İşlev uygulamanızı yeniden yayımlamak zorunda kalmadan mevcut bağlama uzantılarını güncelleştirmek için bkz. [uzantılarınızı güncelleştirme]. |

[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus/
[core tools]: ./functions-run-local.md
[Uzantı paketi]: ./functions-bindings-register.md#extension-bundles
[Uzantılarınızı güncelleştirme]: ./install-update-binding-extensions-manual.md
[Azure Araçları uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

1. x uygulamalarının [Microsoft. Azure. WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketine, sürüm 2. x 'e başvuran bir başvurusu otomatik olarak vardır.

## <a name="next-steps"></a>Sonraki adımlar

- [Service Bus kuyruğu veya konu iletisi oluşturulduğunda bir işlev çalıştırma (tetikleyici)](./functions-bindings-service-bus-trigger.md)
- [Azure Işlevlerinden Azure Service Bus iletileri gönderme (çıkış bağlama)](./functions-bindings-service-bus-output.md)
