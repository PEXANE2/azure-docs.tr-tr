---
author: baanders
description: Azure dijital TWINS örnekleri 'nde DefaultAzureCredential için yerel kimlik doğrulamasını ayarlamaya yönelik dosya ekleme-giriş
ms.service: digital-twins
ms.topic: include
ms.date: 10/22/2020
ms.author: baanders
ms.openlocfilehash: 35643370d6fd313d42f954a578b73befc025c040
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "94329531"
---
### <a name="set-up-local-azure-credentials"></a>Yerel Azure kimlik bilgilerini ayarlama

Bu örnek [DefaultAzureCredential](/dotnet/api/azure.identity.defaultazurecredential?preserve-view=true&view=azure-dotnet) `Azure.Identity` , yerel makinenizde çalıştırdığınızda Azure dijital TWINS örneğiyle kullanıcıların kimliğini doğrulamak için DefaultAzureCredential (kitaplığın parçası) kullanır. Bir istemci uygulamasının Azure dijital TWINS ile kimlik doğrulaması yapabilme yolları hakkında daha fazla bilgi için bkz. [*nasıl yapılır: yazma uygulaması kimlik doğrulaması kodu*](../articles/digital-twins/how-to-authenticate-client.md).

[!INCLUDE [Azure Digital Twins: local credentials prereq (inner)](digital-twins-local-credentials-inner.md)]