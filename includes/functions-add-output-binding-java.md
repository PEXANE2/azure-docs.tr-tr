---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/18/2020
ms.author: glenga
ms.openlocfilehash: c27f3fea6d2dd8b891220ba06b375b33e6cd950b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673456"
---
Java projesinde bağlamalar, işlev yönteminde bağlama ek açıklamaları olarak tanımlanır. Dosyadaki *function.js* bu ek açıklamaları temel alınarak otomatik olarak oluşturulur.

_Src/Main/Java_altındaki işlev kodunuzun konumuna gidin, *function. Java* proje dosyasını açın ve Yöntem tanımına aşağıdaki parametreyi ekleyin `run` :

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="20-21":::

`msg`Parametresi [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding) , işlev tamamlandığında çıkış bağlamaya ileti olarak yazılmış dizeler koleksiyonunu temsil eden bir türdür. Bu durumda, çıktı adlı bir depolama kuyruğudur `outqueue` . Depolama hesabı için bağlantı dizesi yöntemi tarafından ayarlanır `connection` . Bağlantı dizesinin kendisi yerine, depolama hesabı bağlantı dizesini içeren uygulama ayarını geçirirsiniz.

`run`Yöntem tanımı şimdi aşağıdaki örnekteki gibi görünmelidir:  

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="16-22":::