---
author: baanders
description: Azure dijital TWINS örnekleri içindeki DefaultAzureCredential için dosya ekleme-Note
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 8aa3cbb2a037e49a694192c9852eeae0215c089c
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102211797"
---
>[!NOTE]
> Bu örnek [](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) `Azure.Identity` , yerel makinenizde çalıştırdığınızda Azure dijital TWINS örneğiyle kullanıcıların kimliğini doğrulamak için DefaultAzureCredential (kitaplığın parçası) kullanır. Bu tür bir kimlik doğrulamasıyla, örnek yerel bir [Azure CLI](/cli/azure/install-azure-cli) veya Visual Studio/Visual Studio Code oturum açma gibi yerel ortamınızda Azure kimlik bilgilerini arar.
>
> `DefaultAzureCredential`Ve diğer kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz. [*nasıl yapılır: yazma uygulaması kimlik doğrulaması kodu*](../articles/digital-twins/how-to-authenticate-client.md).