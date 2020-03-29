---
title: .NET - Azure Depolama ile depolanmış bir erişim ilkesi tanımlama
description: .NET istemci kitaplığını kullanarak depolanmış bir erişim ilkesini nasıl tanımlayın öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 08/06/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 272d676d0a5a55262b1c68d0bae9a9ab229df72c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68990747"
---
# <a name="define-a-stored-access-policy-with-net"></a>.NET ile depolanmış bir erişim ilkesi tanımlama

Depolanan erişim ilkesi, sunucu tarafındaki hizmet düzeyi paylaşılan erişim imzaları (SAS) üzerinde ek bir denetim düzeyi sağlar. Depolanmış bir erişim ilkesinin tanımlanması, paylaşılan erişim imzalarını gruplandırmaya ve ilke tarafından bağlı paylaşılan erişim imzaları için ek kısıtlamalar sağlamaya hizmet eder. Bir SAS'ın başlangıç saatini, son kullanma saatini veya izinlerini değiştirmek veya yayımlandıktan sonra iptal etmek için depolanmış bir erişim ilkesini kullanabilirsiniz.
  
 Aşağıdaki depolama kaynakları depolanan erişim ilkelerini destekler:  
  
- Blob kaplar  
- Dosya paylaşımları  
- Kuyruklar  
- Tablolar  
  
> [!NOTE]
> Kapsayıcıda depolanan bir erişim ilkesi, kapsayıcının kendisine veya içerdiği lekelere izin veren paylaşılan bir erişim imzasıyla ilişkilendirilebilir. Benzer şekilde, bir dosya paylaşımında depolanan bir erişim ilkesi, paylaşımın kendisine veya içerdiği dosyalara izin veren paylaşılan bir erişim imzasıyla ilişkilendirilebilir.  
>
> Depolanan erişim ilkeleri yalnızca bir hizmet SAS için desteklenir. Saklanan erişim ilkeleri hesap SAS veya kullanıcı delegasyonu SAS için desteklenmez.  

## <a name="create-a-stored-access-policy"></a>Depolanmış erişim ilkesi oluşturma

Aşağıdaki kod, bir kapsayıcıüzerinde depolanmış bir erişim ilkesi oluşturur. Kapsayıcıda veya blobs üzerinde bir hizmet SAS için kısıtlamalar belirtmek için erişim ilkesini kullanabilirsiniz.

```csharp
private static async Task CreateStoredAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new stored access policy and define its constraints.
    // The access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When the start time for the SAS is omitted, the start time is assumed to be the time when Azure Storage receives the request.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get the container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

## <a name="see-also"></a>Ayrıca bkz.

- [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni verme](storage-sas-overview.md)
- [Depolanan erişim ilkesi tanımlama](/rest/api/storageservices/define-stored-access-policy)

