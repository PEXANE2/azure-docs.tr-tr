---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 3705f58a37c109ebe0b774603c60e246fc174f25
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673286"
---
Şimdi, işlev kodunuzdan çıktı bağlama yazmak için yeni `msg` parametreyi kullanabilirsiniz. Çıktı bağlama değeri eklemek `name` için başarı yanıtı önce kod aşağıdaki satırı ekleyin. `msg`

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="34":::

Çıktı bağlama kullandığınızda, kimlik doğrulaması, sıra başvurusu almak veya veri yazmak için Azure Depolama SDK kodunu kullanmanız gerekmez. İşlevler çalışma zamanı ve sıra çıktısı bağlama bu görevleri sizin için yapar.

Yönteminiz `run` şimdi aşağıdaki örnek gibi görünmelidir:

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/main/java/com/function/Function.java" range="17-38":::