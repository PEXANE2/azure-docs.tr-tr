---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/18/2020
ms.author: glenga
ms.openlocfilehash: eb54439f89cc2443eeed2d3b63dfbe7fedb4bf17
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673479"
---
## <a name="update-the-tests"></a>Testleri güncelleştirme

Arketip aynı zamanda bir dizi test oluşturduğundan, yöntem imzasındaki `msg` yeni parametreyi işlemek için bu testleri güncelleştirmeniz `run` gerekir.  

_Src/test/java_altında test kodunuzun konumuna göz atın, *Function.java* proje dosyasını `//Invoke` açın ve aşağıdaki kodla kod satırını değiştirin.

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/test/java/com/function/FunctionTest.java" range="48-50":::
