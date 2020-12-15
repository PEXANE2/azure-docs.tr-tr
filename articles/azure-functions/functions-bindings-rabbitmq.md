---
title: Azure için Azure Kbıbitmq bağlamaları Işlevleri
description: Azure Işlevleri 'nde Azure Kbıbitmq Tetikleyicileri ve bağlamaları gönderme hakkında bilgi edinin.
author: cachai2
ms.assetid: ''
ms.topic: reference
ms.date: 12/11/2020
ms.author: cachai
ms.custom: ''
ms.openlocfilehash: cd6c8de0d9290aab273e25403bb1cb2d8fe07e27
ms.sourcegitcommit: 2ba6303e1ac24287762caea9cd1603848331dd7a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97505768"
---
# <a name="rabbitmq-bindings-for-azure-functions-overview"></a>Azure Işlevlerine yönelik Kbbitmq bağlamaları genel bakış

> [!NOTE]
> Kbbitmq bağlamaları yalnızca **Windows Premium** planlarında tam olarak desteklenmektedir. Tüketim ve Linux Şu anda desteklenmiyor.

Azure Işlevleri [, Tetikleyiciler ve bağlamalar](./functions-triggers-bindings.md)aracılığıyla [Kbbitmq](https://www.rabbitmq.com/) ile tümleşir. Azure Işlevleri kbbitmq bağlama uzantıları, kbbitmq API 'sini Işlevlerle kullanarak ileti göndermenizi ve almanızı sağlar.

| Eylem | Tür |
|---------|---------|
| Bir Kbbitmq iletisi kuyruktan geldiğinde bir işlev Çalıştır | [Tetikleyici](./functions-bindings-rabbitmq-trigger.md) |
| Kbıbitmq iletileri gönderme |[Çıkış bağlama](./functions-bindings-rabbitmq-output.md) |

## <a name="add-to-your-functions-app"></a>Işlevler uygulamanıza ekleme

Bu uzantıyla geliştirmeye başlamak için önce [bir Kbbitmq uç noktası ayarladığınızdan](https://github.com/Azure/azure-functions-rabbitmq-extension/wiki/Setting-up-a-RabbitMQ-Endpoint)emin olun. Kbbitmq hakkında daha fazla bilgi edinmek için Başlarken [sayfasına](https://www.rabbitmq.com/getstarted.html)göz atın.

### <a name="functions-2x-and-higher"></a>İşlevler 2.x ve üzeri

Tetikleyici ve bağlamalarla çalışma, uygun pakete başvurmanız gerekir. Diğer tüm uygulama türleri için Uzantı paketi kullanıldığında, .NET sınıf kitaplıkları için NuGet paketi kullanılır.

| Dil                                        | Ekleme ölçütü...                                   | Açıklamalar 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | [NuGet paketi]yükleniyor, sürüm 4. x | |
| C# betiği, Java, JavaScript, Python, PowerShell | [Uzantı paketi] kaydediliyor          | [Azure Araçları uzantısının] Visual Studio Code ile kullanılması önerilir. |
| C# betiği (yalnızca çevrimiçi-Azure portal)         | Bağlama ekleme                            | İşlev uygulamanızı yeniden yayımlamak zorunda kalmadan mevcut bağlama uzantılarını güncelleştirmek için bkz. [uzantılarınızı güncelleştirme]. |

[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.RabbitMQ
[core tools]: ./functions-run-local.md
[Uzantı paketi]: ./functions-bindings-register.md#extension-bundles
[Uzantılarınızı güncelleştirme]: ./functions-bindings-register.md
[Azure Araçları uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

Kbbitmq bağlama uzantıları, 1. x Işlevleri için desteklenmez. Lütfen 2. x ve üzeri Işlevleri kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir Kbbitmq iletisi oluşturulduğunda bir işlev çalıştırma (tetikleyici)](./functions-bindings-rabbitmq-trigger.md)
- [Azure Işlevlerinden Kbıbitmq iletileri gönderme (çıkış bağlama)](./functions-bindings-rabbitmq-output.md)