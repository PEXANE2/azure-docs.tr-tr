---
title: .NET ile hesap SAS oluşturma
titleSuffix: Azure Storage
description: .NET istemci kitaplığını kullanarak paylaşılan erişim imzası (SAS) hesabını nasıl oluşturursunuz öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 9da27cef7bafa94715a42db86fc5a5675a049eb1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137878"
---
# <a name="create-an-account-sas-with-net"></a>.NET ile hesap SAS oluşturma

[!INCLUDE [storage-auth-sas-intro-include](../../../includes/storage-auth-sas-intro-include.md)]

Bu makalede, [.NET için Azure Depolama istemci kitaplığı](/dotnet/api/overview/azure/storage?view=azure-dotnet)ile bir hesap SAS oluşturmak için depolama hesabı anahtarının nasıl kullanılacağı nı gösterir.

## <a name="create-an-account-sas"></a>Hesap SAS’si oluşturma

Bir kapsayıcı için hesap SAS oluşturmak için [CloudStorageAccount.GetSharedAccessSignature](/dotnet/api/microsoft.azure.storage.cloudstorageaccount.getsharedaccesssignature) yöntemini arayın.

Aşağıdaki kod örneği, Blob ve Dosya hizmetleri için geçerli olan bir hesap SAS oluşturur ve istemciye hizmet düzeyi API'lerine erişmek için okuma, yazma ve liste izinleri verir. SAS hesabı protokolü HTTPS ile sınırlandırırken, istek HTTPS ile yapılmalıdır. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```csharp
static string GetAccountSASToken()
{
    // To create the account SAS, you need to use Shared Key credentials. Modify for your account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=<storage-account>;AccountKey=<account-key>";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create a new access policy for the account.
    SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
        {
            Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
            Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
            ResourceTypes = SharedAccessAccountResourceTypes.Service,
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Protocols = SharedAccessProtocol.HttpsOnly
        };

    // Return the SAS token.
    return storageAccount.GetSharedAccessSignature(policy);
}
```

## <a name="use-an-account-sas-from-a-client"></a>İstemciden gelen bir hesap SAS kullanma

Blob hizmetiiçin hizmet düzeyi API'lerine erişmek için SAS hesabını kullanmak için, depolama hesabınız için SAS ve Blob depolama bitiş noktasını kullanarak bir Blob hizmet istemcisi nesnesi oluşturun. Açı parantezindeki yer tutucu değerlerini kendi değerlerinizle değiştirmeyi unutmayın:

```csharp
static void UseAccountSAS(string sasToken)
{
    // Create new storage credentials using the SAS token.
    StorageCredentials accountSAS = new StorageCredentials(sasToken);
    // Use these credentials and the account name to create a Blob service client.
    CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "<storage-account>", endpointSuffix: null, useHttps: true);
    CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

    // Now set the service properties for the Blob client created with the SAS.
    blobClientWithSAS.SetServiceProperties(new ServiceProperties()
    {
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        Logging = new LoggingProperties()
        {
            LoggingOperations = LoggingOperations.All,
            RetentionDays = 14,
            Version = "1.0"
        }
    });

    // The permissions granted by the account SAS also permit you to retrieve service properties.
    ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni verme](storage-sas-overview.md)
- [Hesap SAS’si oluşturma](/rest/api/storageservices/create-account-sas)
