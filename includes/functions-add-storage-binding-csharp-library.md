---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 07/05/2019
ms.author: glenga
ms.openlocfilehash: 5e1a2622df0038141dd5cb05237f93d5e33e0bfb
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78190933"
---
Bir C# sınıf kitaplığı projesinde bağlamalar, işlev yönteminde bağlama öznitelikleri olarak tanımlanır. Işlevler tarafından istenen *function. JSON* dosyası, bu özniteliklere göre otomatik olarak oluşturulur.

*HttpExample.cs* proje dosyasını açın ve `Run` yöntemi tanımına aşağıdaki parametreyi ekleyin:

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="17":::

`msg` parametresi, işlev tamamlandığında çıkış bağlamaya yazılan bir ileti koleksiyonunu temsil eden bir `ICollector<T>` türüdür. Bu durumda, çıktı `outqueue`adlı bir depolama kuyruğudur. Depolama hesabının bağlantı dizesi `StorageAccountAttribute`tarafından ayarlanır. Bu öznitelik, depolama hesabı bağlantı dizesini içeren ayarı belirtir ve sınıfı, yöntemi veya parametre düzeyinde uygulanabilir. Bu durumda, zaten varsayılan depolama hesabını kullandığınızdan `StorageAccountAttribute` atlayabilirsiniz.

Çalıştırma yöntemi tanımı artık aşağıdaki gibi görünmelidir:  

:::code language="csharp" source="~/functions-docs-csharp/functions-add-output-binding-storage-queue-cli/HttpExample.cs" range="14-18":::
