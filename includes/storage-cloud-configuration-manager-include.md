---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 5102866cdda51ef545fd95b32946cb17c6e40a3c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "72038183"
---
[.NET için Microsoft Azure Yapılandırma Yöneticisi Kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager/), yapılandırma dosyasından bağlantı dizesini ayrıştırmak için bir sınıf sağlar. [Cloudconfigurationmanager](https://msdn.microsoft.com/library/azure/mt634650.aspx) sınıfı yapılandırma ayarlarını ayrıştırır. Masaüstünde, bir mobil cihazda, bir Azure sanal makinesinde veya Azure bulut hizmetinde çalışan istemci uygulamalarına yönelik ayarları ayrıştırır.

Pakete başvurmak için `CloudConfigurationManager` aşağıdaki `using` yönergeleri ekleyin:

```csharp
using Microsoft.Azure; //Namespace for CloudConfigurationManager
using Microsoft.Azure.Storage;
```

Burada, yapılandırma dosyasından bir bağlantı dizesinin nasıl alındığını gösteren bir örnek bulunmaktadır:

```csharp
// Parse the connection string and return a reference to the storage account.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));
```

Azure Yapılandırma Yöneticisi'ni kullanmak isteğe bağlıdır. .NET Framework [ConfigurationManager sınıfı](/dotnet/api/system.configuration.configurationmanager)gıbı bir API de kullanabilirsiniz.
