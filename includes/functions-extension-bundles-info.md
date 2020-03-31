---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: 1fc37c6f93fba34944caa7a91c2a89ce5dcdc398
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78201944"
---
::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
> [!TIP]
> Başlangıç sırasında ana bilgisayar, Depolama [bağlama uzantısını](../articles/azure-functions/functions-bindings-storage-queue.md#functions-2x-and-higher) ve diğer Microsoft bağlama uzantılarını karşıdan yükler ve yükler. Bağlama uzantıları varsayılan olarak *ana bilgisayar.json* dosyasında aşağıdaki özelliklere sahip etkinleştirildiğinden bu yükleme gerçekleşir:
>
> ```json
> {
>     "version": "2.0",
>     "extensionBundle": {
>         "id": "Microsoft.Azure.Functions.ExtensionBundle",
>         "version": "[1.*, 2.0.0)"
>     }
> }
> ```
>
> Bağlama uzantılarıyla ilgili herhangi bir hatayla karşılaşırsanız, yukarıdaki özelliklerin *host.json'da*mevcut olup olmadığını kontrol edin.
::: zone-end  