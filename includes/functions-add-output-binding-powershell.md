---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: 753a3d16792e0dbaffe318cd188ea3f0d40bd5cb
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "78191085"
---
`Push-OutputBinding`Çıkış bağlamayı kullanarak bir metni kuyruğa yazmak için cmdlet 'ini kullanan kodu ekleyin `msg` . Deyimdeki Tamam durumunu ayarlamadan önce bu kodu ekleyin `if` .

:::code language="powershell" range="18-19" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::

Bu noktada, işleviniz aşağıdaki gibi görünmelidir:

:::code language="powershell" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::
