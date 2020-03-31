---
title: Azure Fonksiyonları SignalR Hizmet bağlamaları
description: Azure İşlevleriyle SignalR Hizmet bağlamalarının nasıl kullanılacağını öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/28/2019
ms.author: cshoe
ms.openlocfilehash: 863620ce6f0af33b05ef290ae95ccdc99a53a54d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77523045"
---
# <a name="signalr-service-bindings-for-azure-functions"></a>Azure İşlevleri için SignalR Service bağlamaları

Bu makale kümesi, Azure İşlerinden Sinyal Hizmeti bağlamalarını kullanarak [Azure SinyalR Hizmeti'ne](https://azure.microsoft.com/services/signalr-service/) bağlı istemcilerin gerçek zamanlı iletilerinin nasıl doğrulaşdırılabildiğini ve gönderilebildiğini açıklar. Azure İşlevleri, SignalR Hizmeti’ne yönelik giriş ve çıkış bağlamalarını destekler.

| Eylem | Tür |
|---------|---------|
| Hizmet bitiş noktası URL'sini ve erişim jetonunu döndür | [Giriş bağlama](./functions-bindings-signalr-service-input.md) |
| SignalR Hizmeti mesajları gönder |[Çıkış bağlama](./functions-bindings-signalr-service-output.md) |

## <a name="add-to-your-functions-app"></a>Fonksiyonlar uygulamanıza ekleme

### <a name="functions-2x-and-higher"></a>Fonksiyonlar 2.x ve üzeri

Tetikleyici ve bağlamalarla çalışmak, uygun pakete başvurmanızı gerektirir. NuGet paketi .NET sınıf kitaplıkları için kullanılırken, uzantı paketi diğer tüm uygulama türleri için kullanılır.

| Dil                                        | Tarafından ekle...                                   | Açıklamalar 
|-------------------------------------------------|---------------------------------------------|-------------|
| C#                                              | [NuGet paketini]yükleme , sürüm 3.x | |
| C# Script, Java, JavaScript, Python, PowerShell | [Uzantı paketini] kaydetme          | [Azure Araçları uzantısı] Visual Studio Code ile kullanılması önerilir. |
| C# Script (yalnızca Azure portalında çevrimiçi)         | Bağlama ekleme                            | İşlev uygulamanızı yeniden yayımlamak zorunda kalmadan varolan bağlama uzantılarını güncelleştirmek için [bkz.] |

[NuGet paketi]: https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SignalRService
[core tools]: ./functions-run-local.md
[uzatma paketi]: ./functions-bindings-register.md#extension-bundles
[Uzantılarınızı güncelleştirin]: ./install-update-binding-extensions-manual.md
[Azure Araçları uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.vscode-node-azure-pack

SignalR Hizmeti ve Azure Fonksiyonlarının birlikte nasıl yapılandırılabildiğini ve kullanılacağı hakkında ayrıntılı bilgi için Azure [İşlevleri geliştirme ve Azure SinyalR Hizmeti ile yapılandırmaya](../azure-signalr/signalr-concept-serverless-development-config.md)bakın.

### <a name="annotations-library-java-only"></a>Ek Açıklamalar kitaplığı (yalnızca Java)

Java işlevlerindeki SignalR Hizmeti ek açıklamalarını kullanmak *için, pom.xml* dosyanıza *azure-functions-java-library-signalr* artifakı (sürüm 1.0 veya daha yüksek sürüm) bir bağımlılık eklemeniz gerekir.

```xml
<dependency>
    <groupId>com.microsoft.azure.functions</groupId>
    <artifactId>azure-functions-java-library-signalr</artifactId>
    <version>1.0.0</version>
</dependency>
```

## <a name="next-steps"></a>Sonraki adımlar

- [Hizmet bitiş noktası URL'sini ve erişim belirteci (Giriş bağlama) döndürme](./functions-bindings-signalr-service-input.md)
- [SignalR Hizmeti mesajları gönder (Çıkış bağlama)](./functions-bindings-signalr-service-output.md) 
