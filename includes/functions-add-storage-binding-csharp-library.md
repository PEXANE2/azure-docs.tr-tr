---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 5e1a2622df0038141dd5cb05237f93d5e33e0bfb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78190933"
---
C# sınıf kitaplığı projesinde bağlamalar, işlev yönteminde bağlama öznitelikleri olarak tanımlanır. Işlevler tarafından istenen *function. JSON* dosyası, bu özniteliklere göre otomatik olarak oluşturulur.

*HttpExample.cs* proje dosyasını açın ve `Run` Yöntem tanımına aşağıdaki parametreyi ekleyin:

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="17":::

`msg` Parametresi, işlev tamamlandığında `ICollector<T>` çıkış bağlamaya yazılan bir ileti koleksiyonunu temsil eden bir türdür. Bu durumda, çıktı adlı `outqueue`bir depolama kuyruğudur. Depolama hesabının bağlantı dizesi tarafından ayarlanır `StorageAccountAttribute`. Bu öznitelik, depolama hesabı bağlantı dizesini içeren ayarı belirtir ve sınıfı, yöntemi veya parametre düzeyinde uygulanabilir. Bu durumda, zaten varsayılan depolama hesabını `StorageAccountAttribute` kullandığınızdan atlayabilirsiniz.

Çalıştırma yöntemi tanımı artık aşağıdaki gibi görünmelidir:  

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="14-18":::
