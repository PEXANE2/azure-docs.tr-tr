---
title: Azure İşlevler için Azure Olay Izgara bağlamaları
description: Azure İşlevlerinde Olay Ağı olaylarının nasıl işleyeceğinizi öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/14/2020
ms.author: cshoe
ms.custom: fasttrack-edit
ms.openlocfilehash: 21654a3b325e8b8f0a3e49ee64b7624c8540d0d5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77461088"
---
# <a name="azure-event-grid-bindings-for-azure-functions"></a>Azure İşlevler için Azure Olay Izgara bağlamaları

Bu başvuru, Azure İşlevlerinde [Olay Kılavuzu olaylarının](../event-grid/overview.md) nasıl işleyeceğini açıklar. Olay Izgara iletilerinin bir HTTP bitiş noktasında nasıl işleyeceğiniz hakkında ayrıntılı bilgi için, [olayları bir HTTP bitiş noktasına alın'a](../event-grid/receive-events.md)bakın.

Olay Ağı, yayıncılarda meydana gelen olaylar hakkında sizi bilgilendirmek için HTTP istekleri gönderen bir Azure *hizmetidir.* Yayımcı, olayı oluşturan hizmet veya kaynaktır. Örneğin, Bir Azure blob depolama hesabı bir yayımcı ve [blob yükleme veya silme bir olaydır.](../storage/blobs/storage-blob-event-overview.md) Bazı [Azure hizmetleri, Etkinlikleri Olay Ağı'nda yayımlamak için yerleşik destek ekidir.](../event-grid/overview.md#event-sources)

Olay *işleyicileri* olayları alır ve işlenir. Azure İşlevler, [Olay Ağı olaylarını işlemek için yerleşik desteği olan](../event-grid/overview.md#event-handlers)birkaç Azure hizmetinden biridir. Bu başvuruda, olay Grid'den bir olay alındığı zaman bir işlevi çağırmak ve olayları olay [ızgarası özel konusuna](../event-grid/post-to-custom-topic.md)göndermek için çıktı bağlamayı kullanmak için bir Olay Izgara tetikleyicisi kullanmayı öğrenirsiniz.

İsterseniz, Olay Izgara Olayları işlemek için bir HTTP tetikleyici kullanabilirsiniz; bkz. [Olayları bir HTTP bitiş noktasına alın.](../event-grid/receive-events.md) Şu anda, etkinlik [CloudEvents şemasında](../event-grid/cloudevents-schema.md#azure-functions)teslim edildiğinde Azure İşlevler uygulaması için Olay Izgara tetikleyicisi kullanamazsınız. Bunun yerine, bir HTTP tetikleyicikullanın.

| Eylem | Tür |
|---------|---------|
| Olay Izgara olayı gönderildiğinde bir işlev çalıştırma | [Tetikleyici](./functions-bindings-event-grid-trigger.md) |
| Olay Izgara olayı gönderir |[Çıkış bağlama](./functions-bindings-event-grid-output.md) |

Bu başvurudaki kod, Fonksiyonlar sürüm 2.x ve üstü sürümlerinde kullanılan .NET Core sözdizimini varsayılan olarak kullanır. 1.x sözdizimi hakkında bilgi için [1.x işlevleri şablonlarına](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates)bakın.

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
[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid
[Uzantılarınızı güncelleştirin]: ./install-update-binding-extensions-manual.md
[Azure Tools extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

### <a name="functions-1x"></a>İşlevler 1.x

Fonksiyonlar 1.x uygulamaları otomatik olarak [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi, sürüm 2.x bir başvuru var.

## <a name="next-steps"></a>Sonraki adımlar
* [Olay Izgara olayı gönderildiğinde bir işlev çalıştırma](./functions-bindings-event-grid-trigger.md)
* [Olay Izgara olayı gönderme](./functions-bindings-event-grid-trigger.md)
