---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/09/2020
ms.author: glenga
ms.openlocfilehash: 1fc37c6f93fba34944caa7a91c2a89ce5dcdc398
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "78201944"
---
::: zone pivot="programming-language-python,programming-language-javascript,programming-language-powershell,programming-language-typescript"  
> [!TIP]
> Başlangıç sırasında, ana bilgisayar [depolama bağlama uzantısını](../articles/azure-functions/functions-bindings-storage-queue.md#functions-2x-and-higher) ve diğer Microsoft bağlama uzantılarını indirir ve yükler. Bu yükleme, bağlama uzantıları aşağıdaki özelliklerle dosyadaki *host.js* varsayılan olarak etkinleştirildiğinden oluşur:
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
> Bağlama uzantılarıyla ilgili herhangi bir hatayla karşılaşırsanız, yukarıdaki özelliklerin *host.js*' de bulunduğundan emin olun.
::: zone-end  