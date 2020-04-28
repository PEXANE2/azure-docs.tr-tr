---
title: .NET ile güvenli TLS 'yi etkinleştirme
titleSuffix: Azure Storage
description: Azure depolama için .NET istemci kitaplığı 'nı kullanarak TLS 1,2 ' i etkinleştirmeyi öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/25/2018
ms.author: tamram
ms.reviewer: fryu
ms.subservice: common
ms.openlocfilehash: 81c9a8fe9513f1f8fc65ad64b34f0fb04383569b
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "75371811"
---
# <a name="enable-secure-tls-for-azure-storage-client"></a>Azure Depolama istemcisi için güvenli TLS'yi etkinleştirme

Aktarım Katmanı Güvenliği (TLS) ve Güvenli Yuva Katmanı (SSL), bir bilgisayar ağı üzerinden iletişim güvenliği sağlayan şifreleme protokolleridir. Güvenlik açığı bulunan SSL 1,0, 2,0 ve 3,0. RFC tarafından yasaklanmış olmaları gerekir. TLS 1,0, güvenli olmayan blok şifresi (DES CBC ve RC2 CBC) ve akış şifresi (RC4) kullanımı için güvenli hale gelir. PCI Council Ayrıca daha yüksek TLS sürümlerine geçiş için önerilir. Daha fazla ayrıntı için [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)görebilirsiniz.

Azure Storage, 2015 sonrasında SSL 3,0 ' i durdurdu ve genel HTTPs uç noktalarında TLS 1,2 kullandığından, TLS 1,0 ve TLS 1,1 yine de geriye dönük uyumluluk için desteklenmektedir.

Azure depolama 'ya güvenli ve uyumlu bir bağlantı sağlamak için, Azure depolama hizmetini çalıştırmak üzere istek göndermeden önce istemci tarafında TLS 1,2 veya daha yeni bir sürümü etkinleştirmeniz gerekir.

## <a name="enable-tls-12-in-net-client"></a>.NET istemcisinde TLS 1.2'yi etkinleştirme

İstemcisinin TLS 1,2 anlaşmasına sahip olması için, işletim sisteminin ve .NET Framework sürümünün her ikisi de TLS 1,2 ' i desteklemelidir. [TLS 1,2 desteğiyle ilgili](https://docs.microsoft.com/dotnet/framework/network-programming/tls#support-for-tls-12)daha fazla ayrıntı görüntüleyin.

Aşağıdaki örnekte, .NET istemcisinde TLS 1,2 nasıl etkinleştirileceği gösterilmektedir.

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

Aşağıdaki örnek, PowerShell istemcinizdeki TLS 1,2 ' i nasıl etkinleştireceğinizi göstermektedir.

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

## <a name="verify-tls-12-connection"></a>TLS 1,2 bağlantısını doğrulama

Fiddler 'i kullanarak TLS 1,2 'in gerçekten kullanıldığını doğrulayabilirsiniz. İstemci ağ trafiği yakalamaya başlamak için Fiddler 'ı açın ve ardından yukarıdaki örneği yürütün. Daha sonra, örneğin, bağlantının yaptığı bağlantıda TLS sürümünü bulabilirsiniz.

Aşağıdaki ekran görüntüsü, doğrulama için bir örnektir.

![Fiddler 'da TLS sürümünün doğrulanması ekran görüntüsü](./media/storage-security-tls/storage-security-tls-verify-in-fiddler.png)

## <a name="see-also"></a>Ayrıca bkz.

* [Aktarım Katmanı Güvenliği (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security#SSL_1.0.2C_2.0_and_3.0)
* [TLS üzerinde PCI uyumluluğu](https://blog.pcisecuritystandards.org/migrating-from-ssl-and-early-tls)
* [Java istemcisinde TLS 'yi etkinleştirme](https://www.java.com/en/configure_crypto.html)
