---
author: baanders
description: Azure dijital TWINS örnekleri içindeki DefaultAzureCredential için dosya ekleme-Note
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 35996595158994308a78f4d1197dcdc8f00af618
ms.sourcegitcommit: d6a739ff99b2ba9f7705993cf23d4c668235719f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92494475"
---
>[!NOTE]
> Bu örnek [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) `Azure.Identity` , yerel makinenizde çalıştırdığınızda Azure dijital TWINS örneğiyle kullanıcıların kimliğini doğrulamak için DefaultAzureCredential (kitaplığın parçası) kullanır. Bu tür bir kimlik doğrulamasıyla, örnek yerel bir [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) veya Visual Studio/Visual Studio Code oturum açma gibi yerel ortamınızda Azure kimlik bilgilerini arar.
>
> `DefaultAzureCredential`Ve diğer kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz. [*nasıl yapılır: yazma uygulaması kimlik doğrulaması kodu*](../articles/digital-twins/how-to-authenticate-client.md).