---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: 1fc37c6f93fba34944caa7a91c2a89ce5dcdc398
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78201944"
---
::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
> [!TIP]
> Başlangıç sırasında, ana bilgisayar [depolama bağlama uzantısını](../articles/azure-functions/functions-bindings-storage-queue.md#functions-2x-and-higher) ve diğer Microsoft bağlama uzantılarını indirir ve yükler. Bağlama uzantıları *Host. JSON* dosyasında aşağıdaki özelliklerle varsayılan olarak etkinleştirildiğinden, bu yükleme gerçekleşir:
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
> Bağlama uzantıları ile ilgili herhangi bir hatayla karşılaşırsanız, yukarıdaki özelliklerin *Host. JSON*içinde bulunduğundan emin olun.
::: zone-end  