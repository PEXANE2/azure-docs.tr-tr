---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 8bdd8b9d900cc50fdeb34ff7d233ac4d7e17a45c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78191084"
---
Aşağıdaki kodu eşleştirmek için *HttpÖrnek\\\_\_init\_\_.py'yi* güncelleştirin, `if name:` işlev tanımına `msg` ve `msg.set(name)` deyimin altına parametre ekleyin.

:::code language="python" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/__init__.py":::

`msg` Parametre bir [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest)örneğidir. Yöntemi `set` sıraya bir dize iletisi yazar, bu durumda ad URL sorgu dizesinde işleve aktarılır.
