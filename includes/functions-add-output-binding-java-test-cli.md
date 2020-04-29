---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 6a359cdd44cc0c0cfbd93bd23b69a67a641c7fbb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673255"
---
## <a name="update-the-tests"></a>Testleri güncelleştirme

Ayrıca, aynı zamanda bir test kümesi oluşturduğundan, `msg` `run` Yöntem imzasında yeni parametreyi işlemek için bu testleri güncelleştirmeniz gerekir.  

_Src/test/Java_altındaki test kodunuzun konumuna gidin, *function. Java* proje dosyasını açın ve altındaki `//Invoke` kod satırını aşağıdaki kodla değiştirin.

:::code language="java" source="~/functions-quickstart-java/functions-add-output-binding-storage-queue/src/test/java/com/function/FunctionTest.java" range="48-50":::