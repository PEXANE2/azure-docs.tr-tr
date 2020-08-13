---
title: İstemci uygulaması için Aktarım Katmanı Güvenliği 'ni (TLS) yapılandırma
titleSuffix: Azure Storage
description: Bir istemci uygulamasını, en düşük Aktarım Katmanı Güvenliği (TLS) sürümünü kullanarak Azure depolama ile iletişim kuracak şekilde yapılandırın.
services: storage
author: tamram
ms.service: storage
ms.topic: how-to
ms.date: 07/08/2020
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: d4bead0ad73bd1315ce114c08320c6673ab0f42d
ms.sourcegitcommit: faeabfc2fffc33be7de6e1e93271ae214099517f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88185164"
---
# <a name="configure-transport-layer-security-tls-for-a-client-application"></a>İstemci uygulaması için Aktarım Katmanı Güvenliği 'ni (TLS) yapılandırma

Azure depolama hesabı, güvenlik nedeniyle istemcilerin istekleri göndermek için en düşük Aktarım Katmanı Güvenliği (TLS) sürümünü kullanmasını gerektirebilir. İstemci, gerekli en düşük sürümden daha düşük bir TLS sürümü kullanıyorsa Azure depolama 'ya yapılan çağrılar başarısız olur. Örneğin, bir depolama hesabı TLS 1,2 gerektiriyorsa, TLS 1,1 kullanan bir istemci tarafından gönderilen istek başarısız olur.

Bu makalede, bir istemci uygulamasının belirli bir TLS sürümünü kullanmak üzere nasıl yapılandırılacağı açıklanır. Bir Azure depolama hesabı için gerekli en düşük TLS sürümünü yapılandırma hakkında bilgi için, bkz. [depolama hesabı için gereken minimum Aktarım Katmanı Güvenliği (TLS) sürümünü yapılandırma](transport-layer-security-configure-minimum-version.md).

## <a name="configure-the-client-tls-version"></a>İstemci TLS sürümünü yapılandırma

Bir istemcinin belirli bir TLS sürümü ile istek gönderebilmesi için, işletim sisteminin bu sürümü desteklemesi gerekir.

Aşağıdaki örneklerde, istemcisinin TLS sürümünün PowerShell veya .NET üzerinden 1,2 olarak nasıl ayarlanacağı gösterilmektedir. İstemci tarafından kullanılan .NET Framework TLS 1,2 ' i desteklemelidir. Daha fazla bilgi için bkz. [TLS 1,2 Için destek](/dotnet/framework/network-programming/tls#support-for-tls-12).

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Aşağıdaki örnek, bir PowerShell istemcisinde TLS 1,2 ' i nasıl etkinleştireceğinizi göstermektedir:

```powershell
# Set the TLS version used by the PowerShell client to TLS 1.2.
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

# Create a new container.
$storageAccount = Get-AzStorageAccount -ResourceGroupName $rgName -Name $accountName
$ctx = $storageAccount.Context
New-AzStorageContainer -Name "sample-container" -Context $ctx
```

# <a name="net-v12"></a>[.NET V12](#tab/dotnet)

Aşağıdaki örnek, Azure depolama istemci kitaplığı 'nın 12. sürümünü kullanarak bir .NET istemcisinde TLS 1,2 ' i nasıl etkinleştireceğinizi göstermektedir:

:::code language="csharp" source="~/azure-storage-snippets/blobs/howto/dotnet/dotnet-v12/Networking.cs" id="Snippet_ConfigureTls12":::

# <a name="net-v11"></a>[.NET v11](#tab/dotnet11)

Aşağıdaki örnek, Azure depolama istemci kitaplığı 'nın 11. sürümünü kullanarak bir .NET istemcisinde TLS 1,2 ' i nasıl etkinleştireceğinizi göstermektedir:

```csharp
static void EnableTls12()
{
    // Enable TLS 1.2 before connecting to Azure Storage
    System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

    // Add your connection string here.
    string connectionString = "";

    // Connect to Azure Storage and create a new container.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");
    container.CreateIfNotExists();
}
```

---

## <a name="verify-the-tls-version-used-by-a-client"></a>İstemci tarafından kullanılan TLS sürümünü doğrulama

İstemci tarafından bir istek göndermek için belirtilen TLS sürümünün kullanıldığını doğrulamak için [Fiddler](https://www.telerik.com/fiddler) veya benzer bir aracı kullanabilirsiniz. İstemci ağ trafiğini yakalamaya başlamak için Fiddler 'ı açın ve ardından önceki bölümde örneklerden birini yürütün. Aşağıdaki görüntüde gösterildiği gibi, isteği göndermek için doğru TLS sürümünün kullanıldığını doğrulamak için Fiddler izlemesine bakın.

:::image type="content" source="media/transport-layer-security-configure-client-version/fiddler-trace-tls-version.png" alt-text="İstekte kullanılan TLS sürümünü gösteren Fiddler izlemesini gösteren ekran görüntüsü":::

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı için gerekli minimum Aktarım Katmanı Güvenliği (TLS) sürümünü yapılandırın](transport-layer-security-configure-minimum-version.md)
- [BLOB depolama için güvenlik önerileri](../blobs/security-recommendations.md)
