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
ms.openlocfilehash: 0adf1280fa50e9ee594f3025dff70786f5ba2199
ms.sourcegitcommit: 57eb9acf6507d746289efa317a1a5210bd32ca2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2019
ms.locfileid: "74666180"
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

Azure depolama 'dan Azure kimlik istemci kitaplığı ile kimlik doğrulama hakkında daha fazla bilgi edinmek için bkz. Azure **kimlik kitaplığıyla kimlik doğrulama** başlıklı Bölüm [Azure Active Directory ve Azure kaynakları için Yönetilen kimlikler ile blob 'lara ve kuyruklara erişim yetkisi verme](/azure/storage/common/storage-auth-aad-msi?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json#authenticate-with-the-azure-identity-library).