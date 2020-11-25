---
author: baanders
description: Azure dijital TWINS örnekleri 'nde DefaultAzureCredential için yerel kimlik doğrulamasını ayarlamaya yönelik dosya ekleme-giriş
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 35643370d6fd313d42f954a578b73befc025c040
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011268"
---
### <a name="set-up-local-azure-credentials"></a>Yerel Azure kimlik bilgilerini ayarlama

Bu örnek [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) `Azure.Identity` , yerel makinenizde çalıştırdığınızda Azure dijital TWINS örneğiyle kullanıcıların kimliğini doğrulamak için DefaultAzureCredential (kitaplığın parçası) kullanır. Bir istemci uygulamasının Azure dijital TWINS ile kimlik doğrulaması yapabilme yolları hakkında daha fazla bilgi için bkz. [*nasıl yapılır: yazma uygulaması kimlik doğrulaması kodu*](../articles/digital-twins/how-to-authenticate-client.md).

[!INCLUDE [Azure Digital Twins: local credentials prereq (inner)](digital-twins-local-credentials-inner.md)]