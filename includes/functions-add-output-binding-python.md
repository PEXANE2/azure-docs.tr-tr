---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/23/2019
ms.author: glenga
ms.openlocfilehash: 8bdd8b9d900cc50fdeb34ff7d233ac4d7e17a45c
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "78191084"
---
*Httpexample \\ \_ \_ init \_ \_ . Kopyala* 'yı aşağıdaki kodla eşleşecek şekilde güncelleştirin, `msg` parametreyi işlev tanımına ve `msg.set(name)` deyimin altına ekleyin `if name:` .

:::code language="python" source="~/functions-docs-python/functions-add-output-binding-storage-queue-cli/HttpExample/__init__.py":::

`msg`Parametresi, bir örneğidir [`azure.functions.InputStream class`](/python/api/azure-functions/azure.functions.httprequest) . `set`Yöntemi, kuyruğa bir dize iletisi yazar, bu durumda adı URL sorgu dizesinde işleve geçirilir.
