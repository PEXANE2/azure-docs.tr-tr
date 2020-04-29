---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: 753a3d16792e0dbaffe318cd188ea3f0d40bd5cb
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78191085"
---
Çıkış bağlamayı kullanarak bir metni `Push-OutputBinding` kuyruğa yazmak için cmdlet 'ini kullanan kodu ekleyin. `msg` `if` Deyimdeki Tamam durumunu ayarlamadan önce bu kodu ekleyin.

:::code language="powershell" range="18-19" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::

Bu noktada, işleviniz aşağıdaki gibi görünmelidir:

:::code language="powershell" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::
