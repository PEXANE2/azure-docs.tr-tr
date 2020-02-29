---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 8bdd8b9d900cc50fdeb34ff7d233ac4d7e17a45c
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/29/2020
ms.locfileid: "78191084"
---
*Httpexample\\\_\_init\_\_. Kopyala* ' yı aşağıdaki kodla eşleşecek şekilde güncelleştirin, `msg` parametresini işlev tanımına ve `msg.set(name)` `if name:` deyimin altına ekleyin.

:::code language="python" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/__init__.py":::

`msg` parametresi [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest)bir örneğidir. `set` yöntemi sıraya bir dize iletisi yazar, bu durumda adı URL sorgu dizesinde işleve geçirilir.
