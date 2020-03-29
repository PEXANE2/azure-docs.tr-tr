---
title: .NET ile güvenli TLS'yi etkinleştirin
titleSuffix: Azure Storage
description: Azure Depolama için .NET istemci kitaplığını kullanarak TLS 1.2'yi nasıl etkinleştireceklerini öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/25/2018
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 81c9a8fe9513f1f8fc65ad64b34f0fb04383569b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75371811"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Azure Depolama istemcisi için güvenli TLS'yi etkinleştirme

Aktarım Katmanı Güvenliği (TLS) ve Güvenli Soket katmanı (SSL), bir bilgisayar ağı üzerinden iletişim güvenliği sağlayan şifreleme protokolleridir. SSL 1.0, 2.0 ve 3.0'ın savunmasız olduğu tespit edilmiştir. RFC tarafından yasaklandılar. TLS 1.0 güvensiz Blok şifresi (DES CBC ve RC2 CBC) ve Stream şifresi (RC4) kullanmak için güvensiz hale gelir. PCI konseyi de daha yüksek TLS sürümlerine geçiş önerdi. Daha fazla bilgi [için, Taşıma Katmanı Güvenliği 'ni (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)görebilirsiniz.

Azure Depolama, 2015 yılından bu yana SSL 3.0'ı durdurdu ve ortak HTTP uç noktalarında TLS 1.2 kullanıyor, ancak TLS 1.0 ve TLS 1.1 geriye dönük uyumluluk için hala desteklenmeye devam ediyor.

Azure Depolama'ya güvenli ve uyumlu bağlantı sağlamak için, Azure Depolama hizmetini çalıştırmak için istekler göndermeden önce istemci tarafında TLS 1.2 veya daha yeni sürümü etkinleştirmeniz gerekir.

## <a name="enable-tls-12-in-net-client"></a>.NET istemcisinde TLS 1.2'yi etkinleştirme

Müşterinin TLS 1.2 ile pazarlık edebilmek için, işletim sistemi ve .NET Framework sürümünün TLS 1.2'yi desteklemesi gerekir. [TLS 1.2 için Destek'te](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12)daha fazla ayrıntıya bakın.

Aşağıdaki örnekte TLS 1.2'nin .NET istemcinizde nasıl etkinleştirilen olduğu gösterilmektedir.

```csharp

    static void EnableTls12()
    {
        // Enable TLS 1.2 before connecting to Azure Storage
        System.Net.ServicePointManager.SecurityProtocol = System.Net.SecurityProtocolType.Tls12;

        // Connect to Azure Storage
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName={yourstorageaccount};AccountKey={yourstorageaccountkey};EndpointSuffix=core.windows.net");
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        CloudBlobContainer container = blobClient.GetContainerReference("foo");
        container.CreateIfNotExists();
    }

```

## <a name="enable-tls-12-in-powershell-client"></a>PowerShell istemcisinde TLS 1.2'yi etkinleştirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)] 

Aşağıdaki örnek, PowerShell istemcinizde TLS 1.2'yi nasıl etkinleştirilen gösterir.

```powershell
# Enable TLS 1.2 before connecting to Azure Storage
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12;

$resourceGroup = "{YourResourceGroupName}"
$storageAccountName = "{YourStorageAccountNme}"
$prefix = "foo"

# Connect to Azure Storage
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroup -Name $storageAccountName
$ctx = $storageAccount.Context
$listOfContainers = Get-AzStorageContainer -Context $ctx -Prefix $prefix
$listOfContainers
```

## <a name="verify-tls-12-connection"></a>TLS 1.2 bağlantısını doğrulayın

TLS 1.2'nin gerçekten kullanIlip kullanılmadığını doğrulamak için Fiddler'ı kullanabilirsiniz. İstemci ağ trafiğini yakalamaya başlamak için Fiddler'ı açın ve ardından örnek üzerinde çalıştırın. Ardından TLS sürümünü, örneğin yaptığı bağlantıda bulabilirsiniz.

Aşağıdaki ekran görüntüsü doğrulama için bir örnektir.

![Fiddler'da TLS sürümünü doğrulama ekran görüntüsü](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Ayrıca bkz.

* [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
* [TLS'de PCI uyumluluğu](https://blog.pcisecuritystandards.org/migrating-from-ssl-and-early-tls)
* [Java istemcisinde TLS'yi etkinleştirme](https://www.java.com/en/configure_crypto.html)
