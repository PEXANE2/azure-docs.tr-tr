---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 5e1a2622df0038141dd5cb05237f93d5e33e0bfb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78190933"
---
C# sınıfı kitaplık projesinde, bağlamalar işlev yöntemine bağlama öznitelikleri olarak tanımlanır. *Function.json* dosyası nın işlevleri tarafından gerekli olması, bu özniteliklere dayalı olarak otomatik olarak oluşturulur.

*proje* HttpExample.cs dosyasını açın ve yöntem `Run` tanımına aşağıdaki parametreyi ekleyin:

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="17":::

`msg` Parametre, işlev `ICollector<T>` tamamlandığında çıktı bağlamasına yazılan iletiler koleksiyonunu temsil eden bir türdür. Bu durumda, çıktı adlı `outqueue`bir depolama sırasıdır. Depolama hesabının `StorageAccountAttribute`bağlantı dizesi . Bu öznitelik, Depolama hesabı bağlantı dizesini içeren ve sınıf, yöntem veya parametre düzeyinde uygulanabilen ayarı gösterir. Bu durumda, varsayılan depolama `StorageAccountAttribute` hesabını zaten kullandığınızdan atlayabilirsiniz.

Çalıştır yöntemi tanımı şimdi aşağıdaki gibi görünmelidir:  

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="14-18":::
