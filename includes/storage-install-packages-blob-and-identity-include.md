---
title: include dosyası
description: include dosyası
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/26/2019
ms.author: tamram
ms.custom: include
ms.openlocfilehash: de79ea50d12ab322d1e28d0ad650df30ecc0c222
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74806600"
---
## <a name="install-client-library-packages"></a>İstemci kitaplığı paketlerini yükleme

> [!NOTE]
> Burada gösterilen örnekler, Azure Depolama istemci kitaplığı sürüm 12'yi kullanır. Sürüm 12 istemci kitaplığı Azure SDK'nın bir parçasıdır. Azure SDK hakkında daha fazla bilgi için [GitHub'daki](https://github.com/Azure/azure-sdk)Azure SDK deposuna bakın.

Blob depolama paketini yüklemek için NuGet paket yöneticisi konsolundan aşağıdaki komutu çalıştırın:

```powershell
Install-Package Azure.Storage.Blobs
```

Burada gösterilen örnekler, .NET'in Azure AD kimlik bilgileriyle kimlik doğrulaması [için Azure Identity istemci kitaplığı'nın](https://www.nuget.org/packages/Azure.Identity/) en son sürümünü de kullanır. Paketi yüklemek için NuGet paket yöneticisi konsolundan aşağıdaki komutu çalıştırın:

```powershell
Install-Package Azure.Identity
```
