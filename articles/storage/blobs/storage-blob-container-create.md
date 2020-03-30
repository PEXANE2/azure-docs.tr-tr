---
title: .NET ile blob kapsayıcısı oluşturma veya silme - Azure Depolama
description: .NET istemci kitaplığını kullanarak Azure Depolama hesabınızda bir blob kapsayıcısını nasıl oluşturarak veya silmeyi öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: c95ed6dde3c00c0688ccfd58565fd112427c8899
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79135947"
---
# <a name="create-or-delete-a-container-in-azure-storage-with-net"></a>.NET ile Azure Depolama'da bir kapsayıcı oluşturma veya silme

Azure Depolama'daki blob'lar kapsayıcılar halinde düzenlenir. Bir blob yükleyebilirsiniz önce bir kapsayıcı oluşturmanız gerekir. Bu makalede, [.NET için Azure Depolama istemci kitaplığı](/dotnet/api/overview/azure/storage?view=azure-dotnet)ile kapsayıcılar oluşturmak ve silmek nasıl gösterilmektedir.

## <a name="name-a-container"></a>Bir kapsayıcının adını

Kapsayıcıyı veya lekelerini gidermek için kullanılan benzersiz URI'nin bir parçasını oluşturduğundan, kapsayıcı adı geçerli bir DNS adı olmalıdır. Bir kapsayıcıyı adlandırırken aşağıdaki kurallara uyun:

- Kapsayıcı adları 3 ile 63 karakter uzunluğunda olabilir.
- Kapsayıcı adları bir harf veya sayıyla başlamalıdır ve yalnızca küçük harfler, sayılar ve tire (-) karakterini içerebilir.
- Kapsayıcı adlarında iki veya daha fazla ardışık çizgi karakterine izin verilmez.

Bir kapsayıcının URI'si şu biçimdedir:

`https://myaccount.blob.core.windows.net/mycontainer`

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Kapsayıcı oluşturmak için aşağıdaki yöntemlerden birini arayın:

- [Oluştur](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.create)
- [CreateAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createasync)
- [CreateifNotExists](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createifnotexists)
- [CreateIfNotExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.createifnotexistsasync)

Aynı ada sahip bir kapsayıcı zaten **varsa, Create** ve **CreateAsync** yöntemleri bir özel durum oluşturur.

**CreateIfNotExists** ve **CreateIfNotExistsAsync** yöntemleri kapsayıcı nın oluşturulup oluşturulmadığını belirten bir Boolean değeri döndürür. Aynı ada sahip bir kapsayıcı zaten varsa, bu yöntemler yeni bir kapsayıcı oluşturulmadığını belirtmek için **False** döndürün.

Kapsayıcılar depolama hesabının hemen altında oluşturulur. Bir kabı diğerinin altına yerleştirmek mümkün değildir.

Aşağıdaki örnek, eşzamanlı bir kapsayıcı oluşturur:

```csharp
private static async Task<CloudBlobContainer> CreateSampleContainerAsync(CloudBlobClient blobClient)
{
    // Name the sample container based on new GUID, to ensure uniqueness.
    // The container name must be lowercase.
    string containerName = "container-" + Guid.NewGuid();

    // Get a reference to a sample container.
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    try
    {
        // Create the container if it does not already exist.
        bool result = await container.CreateIfNotExistsAsync();
        if (result == true)
        {
            Console.WriteLine("Created container {0}", container.Name);
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
    }

    return container;
}
```

## <a name="create-the-root-container"></a>Kök kapsayıcısını oluşturma

Kök kapsayıcı, depolama hesabınız için varsayılan bir kapsayıcı görevi görededir. Her depolama hesabının $root olarak adlandırılması gereken bir kök kapsayıcısı *olabilir.* Kök kapsayıcıyı açıkça oluşturmanız veya silmeniz gerekir.

Kök kapsayıcı adını girmeden kök kapsayıcısında depolanan bir blob başvurur. Kök kapsayıcı, depolama hesabı hiyerarşisinin en üst düzeyinde bir blob başvuru sağlar. Örneğin, kök kapsayıcısında bulunan bir blob'a aşağıdaki şekilde başvurulayabilirsiniz:

`https://myaccount.blob.core.windows.net/default.html`

Aşağıdaki örnek, kök kapsayıcısını eşzamanlı olarak oluşturur:

```csharp
private static void CreateRootContainer(CloudBlobClient blobClient)
{
    try
    {
        // Create the root container if it does not already exist.
        CloudBlobContainer container = blobClient.GetContainerReference("$root");
        if (container.CreateIfNotExists())
        {
            Console.WriteLine("Created root container.");
        }
        else
        {
            Console.WriteLine("Root container already exists.");
        }
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
    }
}
```

## <a name="delete-a-container"></a>Kapsayıcı silme

.NET'teki bir kapsayıcıyı silmek için aşağıdaki yöntemlerden birini kullanın:

- [Sil](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.delete)
- [Deleteasync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteasync)
- [DeleteIfExists](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteifexists)
- [DeleteIfExistsAsync](/dotnet/api/microsoft.azure.storage.blob.cloudblobcontainer.deleteifexistsasync)

**Sil** ve **SilAsync** yöntemleri, kapsayıcı yoksa bir özel durum oluşturur.

**DeleteIfExists** ve **DeleteIfExistsAsync** yöntemleri, kapsayıcının silinip silinmediğini belirten bir Boolean değeri döndürer. Belirtilen kapsayıcı yoksa, bu yöntemler kapsayıcının silinmediğini belirtmek için **False** döndürülmez.

Bir kapsayıcıyı sildikten sonra, aynı ada sahip bir kapsayıcıyı en az 30 saniye ve büyük olasılıkla daha uzun süre oluşturamazsınız. Kapsayıcı silinirken, aynı ada sahip bir kapsayıcı oluşturma girişimi HTTP hata kodu 409 (Çakışma) ile başarısız olur. Kapsayıcıdaki diğer işlemler veya içerdiği lekeler, kapsayıcı silinirken HTTP hata kodu 404 (Bulunamadı) ile başarısız olur.

Aşağıdaki örnek, belirtilen kapsayıcıyı siler ve kapsayıcı yoksa özel durumu işler:

```csharp
private static async Task DeleteSampleContainerAsync(CloudBlobClient blobClient, string containerName)
{
    CloudBlobContainer container = blobClient.GetContainerReference(containerName);

    try
    {
        // Delete the specified container and handle the exception.
        await container.DeleteAsync();
    }
    catch (StorageException e)
    {
        Console.WriteLine("HTTP error code {0}: {1}",
                            e.RequestInformation.HttpStatusCode,
                            e.RequestInformation.ErrorCode);
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```

Aşağıdaki örnek, belirtilen bir önek ile başlayan tüm kapsayıcıların nasıl silinecek olduğunu gösterir. Örnek, kapsayıcıda varolan bir kira varsa kirayı bozar.

```csharp
private static async Task DeleteContainersWithPrefixAsync(CloudBlobClient blobClient, string prefix)
{
    Console.WriteLine("Delete all containers beginning with the specified prefix");
    try
    {
        foreach (var container in blobClient.ListContainers(prefix))
        {
            Console.WriteLine("\tContainer:" + container.Name);
            if (container.Properties.LeaseState == LeaseState.Leased)
            {
                await container.BreakLeaseAsync(null);
            }

            await container.DeleteAsync();
        }

        Console.WriteLine();
    }
    catch (StorageException e)
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}
```

[!INCLUDE [storage-blob-dotnet-resources-include](../../../includes/storage-blob-dotnet-resources-include.md)]

## <a name="see-also"></a>Ayrıca bkz.

- [Kapsayıcı işlemi oluşturma](/rest/api/storageservices/create-container)
- [Kapsayıcıyı Silme işlemi](/rest/api/storageservices/delete-container)
