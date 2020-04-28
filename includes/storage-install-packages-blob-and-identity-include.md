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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74806600"
---
## <a name="install-client-library-packages"></a>İstemci kitaplığı paketlerini yükler

> [!NOTE]
> Burada gösterilen örneklerde Azure Storage istemci kitaplığı sürüm 12 kullanılır. Sürüm 12 istemci kitaplığı, Azure SDK 'sının bir parçasıdır. Azure SDK hakkında daha fazla bilgi için [GitHub](https://github.com/Azure/azure-sdk)'DAKI Azure SDK deposuna bakın.

BLOB depolama paketini yüklemek için NuGet paket yöneticisi konsolundan aşağıdaki komutu çalıştırın:

```powershell
Install-Package Azure.Storage.Blobs
```

Burada gösterilen örneklerde, Azure AD kimlik bilgileriyle kimlik doğrulaması yapmak için [.net Için Azure Identity istemci kitaplığı](https://www.nuget.org/packages/Azure.Identity/) 'nın en son sürümünü de kullanabilirsiniz. Paketi yüklemek için NuGet paket yöneticisi konsolundan aşağıdaki komutu çalıştırın:

```powershell
Install-Package Azure.Identity
```
