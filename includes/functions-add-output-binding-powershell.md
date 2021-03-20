---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: 753a3d16792e0dbaffe318cd188ea3f0d40bd5cb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "78191085"
---
`Push-OutputBinding`Çıkış bağlamayı kullanarak bir metni kuyruğa yazmak için cmdlet 'ini kullanan kodu ekleyin `msg` . Deyimdeki Tamam durumunu ayarlamadan önce bu kodu ekleyin `if` .

:::code language="powershell" range="18-19" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::

Bu noktada, işleviniz aşağıdaki gibi görünmelidir:

:::code language="powershell" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::
