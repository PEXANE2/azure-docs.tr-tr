---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/18/2020
ms.author: glenga
ms.openlocfilehash: c27f3fea6d2dd8b891220ba06b375b33e6cd950b
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673456"
---
Java projesinde, bağlamalar işlev yöntemine bağlama ek açıklamaları olarak tanımlanır. *Function.json* dosyası daha sonra bu ek açıklamalara göre otomatik olarak oluşturulur.

_Src/main/java_altında işlev kodunuzu konumuna göz atın, *Function.java* proje dosyasını açın `run` ve yöntem tanımına aşağıdaki parametreyi ekleyin:

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="20-21":::

`msg` Parametre, işlev [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding) tamamlandığında çıktı bağlamasına ileti olarak yazılan dizeler koleksiyonunu temsil eden bir türdür. Bu durumda, çıktı adlı `outqueue`bir depolama sırasıdır. Depolama hesabının bağlantı dizesi `connection` yönteme göre ayarlanır. Bağlantı dizesi yerine, Depolama hesabı bağlantı dizesini içeren uygulama ayarını geçersiniz.

`run` Yöntem tanımı şimdi aşağıdaki örnek gibi görünmelidir:  

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="16-22":::