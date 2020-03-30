---
title: NSG akış günlüklerini okuyun | Microsoft Dokümanlar
description: Bu makalede, NSG akış günlükleri ayrışdırmak için nasıl gösterir
services: network-watcher
documentationcenter: na
author: damendo
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/13/2017
ms.author: damendo
ms.openlocfilehash: 47d927f9f17580767526ec6683e819256fc5e994
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77619928"
---
# <a name="read-nsg-flow-logs"></a>NSG akış günlüklerini okuma

PowerShell ile NSG akış günlükleri girişlerini nasıl okuyabilirsiniz öğrenin.

NSG akış günlükleri [blok lekeleri](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)bir depolama hesabında saklanır. Blok lekeleri daha küçük bloklardan oluşur. Her günlük, her saat oluşturulan ayrı bir blok blob olduğunu. Yeni günlükleri her saat oluşturulur, günlükleri en son verilerle her birkaç dakikada bir yeni girişleri ile güncellenir. Bu makalede, akış günlüklerinin bölümlerini okumayı öğrenirsiniz.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scenario"></a>Senaryo

Aşağıdaki senaryoda, bir depolama hesabında depolanan bir örnek akış günlüğüne sahipsiniz. NSG akış günlüklerinde en son olayları seçişekilde okumayı öğrenirsiniz. Ancak bu makalede PowerShell'i kullanıyorsunuz, ancak makalede tartışılan kavramlar programlama diliyle sınırlı değildir ve Azure Depolama API'leri tarafından desteklenen tüm diller için geçerlidir.

## <a name="setup"></a>Kurulum

Başlamadan önce, hesabınızdaki bir veya birden çok Ağ Güvenlik Grubunda Ağ Güvenliği Grubu Akış Günlüğe Kaydetmeözelliğini etkinleştirmiş olmalıdır. Ağ Güvenliği akış günlüklerini etkinleştirme yönergeleri için aşağıdaki makaleye bakın: [Ağ Güvenlik Grupları için akış günlüğe giriş.](network-watcher-nsg-flow-logging-overview.md)

## <a name="retrieve-the-block-list"></a>Blok listesini alma

Aşağıdaki PowerShell, NSG akış günlüğü blob'u sorgulamak ve [CloudBlockBlob](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblockblob) blok blob'undaki blokları listelemek için gereken değişkenleri ayarlar. Ortamınız için geçerli değerler içerecek şekilde komut dosyasını güncelleştirin.

```powershell
function Get-NSGFlowLogCloudBlockBlob {
    [CmdletBinding()]
    param (
        [string] [Parameter(Mandatory=$true)] $subscriptionId,
        [string] [Parameter(Mandatory=$true)] $NSGResourceGroupName,
        [string] [Parameter(Mandatory=$true)] $NSGName,
        [string] [Parameter(Mandatory=$true)] $storageAccountName,
        [string] [Parameter(Mandatory=$true)] $storageAccountResourceGroup,
        [string] [Parameter(Mandatory=$true)] $macAddress,
        [datetime] [Parameter(Mandatory=$true)] $logTime
    )

    process {
        # Retrieve the primary storage account key to access the NSG logs
        $StorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $storageAccountResourceGroup -Name $storageAccountName).Value[0]

        # Setup a new storage context to be used to query the logs
        $ctx = New-AzStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey

        # Container name used by NSG flow logs
        $ContainerName = "insights-logs-networksecuritygroupflowevent"

        # Name of the blob that contains the NSG flow log
        $BlobName = "resourceId=/SUBSCRIPTIONS/${subscriptionId}/RESOURCEGROUPS/${NSGResourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/${NSGName}/y=$($logTime.Year)/m=$(($logTime).ToString("MM"))/d=$(($logTime).ToString("dd"))/h=$(($logTime).ToString("HH"))/m=00/macAddress=$($macAddress)/PT1H.json"

        # Gets the storage blog
        $Blob = Get-AzStorageBlob -Context $ctx -Container $ContainerName -Blob $BlobName

        # Gets the block blog of type 'Microsoft.Azure.Storage.Blob.CloudBlob' from the storage blob
        $CloudBlockBlob = [Microsoft.Azure.Storage.Blob.CloudBlockBlob] $Blob.ICloudBlob

        #Return the Cloud Block Blob
        $CloudBlockBlob
    }
}

function Get-NSGFlowLogBlockList  {
    [CmdletBinding()]
    param (
        [Microsoft.Azure.Storage.Blob.CloudBlockBlob] [Parameter(Mandatory=$true)] $CloudBlockBlob
    )
    process {
        # Stores the block list in a variable from the block blob.
        $blockList = $CloudBlockBlob.DownloadBlockListAsync()

        # Return the Block List
        $blockList
    }
}


$CloudBlockBlob = Get-NSGFlowLogCloudBlockBlob -subscriptionId "yourSubscriptionId" -NSGResourceGroupName "FLOWLOGSVALIDATIONWESTCENTRALUS" -NSGName "V2VALIDATIONVM-NSG" -storageAccountName "yourStorageAccountName" -storageAccountResourceGroup "ml-rg" -macAddress "000D3AF87856" -logTime "11/11/2018 03:00" 

$blockList = Get-NSGFlowLogBlockList -CloudBlockBlob $CloudBlockBlob
```

Değişken, `$blockList` blob'daki blokların listesini döndürür. Her blok blob en az iki blok içerir.  İlk blok bayt `12` uzunluğuvardır, bu blok json günlüğü açılış parantez içerir. Diğer blok kapanış parantezve `2` bayt uzunluğu vardır.  Gördüğünüz gibi aşağıdaki örnek günlük te yedi girişleri vardır, her biri ayrı bir giriş olmak. Günlükteki tüm yeni girişler son bloktan hemen önce sonuna eklenir.

```
Name                                         Length Committed
----                                         ------ ---------
ZDk5MTk5N2FkNGE0MmY5MTk5ZWViYjA0YmZhODRhYzY=     12      True
NzQxNDA5MTRhNDUzMGI2M2Y1MDMyOWZlN2QwNDZiYzQ=   2685      True
ODdjM2UyMWY3NzFhZTU3MmVlMmU5MDNlOWEwNWE3YWY=   2586      True
ZDU2MjA3OGQ2ZDU3MjczMWQ4MTRmYWNhYjAzOGJkMTg=   2688      True
ZmM3ZWJjMGQ0ZDA1ODJlOWMyODhlOWE3MDI1MGJhMTc=   2775      True
ZGVkYTc4MzQzNjEyMzlmZWE5MmRiNjc1OWE5OTc0OTQ=   2676      True
ZmY2MjUzYTIwYWIyOGU1OTA2ZDY1OWYzNmY2NmU4ZTY=   2777      True
Mzk1YzQwM2U0ZWY1ZDRhOWFlMTNhYjQ3OGVhYmUzNjk=   2675      True
ZjAyZTliYWE3OTI1YWZmYjFmMWI0MjJhNzMxZTI4MDM=      2      True
```

## <a name="read-the-block-blob"></a>Blok blob okuyun

Daha sonra verileri `$blocklist` almak için değişkeni okumanız gerekir. Bu örnekte, engelleme listesini yineler, her bloktaki baytları okur ve bunları bir dizi halinde anlatırız. Verileri almak için [DownloadRangeToByteArray](/dotnet/api/microsoft.azure.storage.blob.cloudblob.downloadrangetobytearray) yöntemini kullanın.

```powershell
function Get-NSGFlowLogReadBlock  {
    [CmdletBinding()]
    param (
        [System.Array] [Parameter(Mandatory=$true)] $blockList,
        [Microsoft.Azure.Storage.Blob.CloudBlockBlob] [Parameter(Mandatory=$true)] $CloudBlockBlob

    )
    # Set the size of the byte array to the largest block
    $maxvalue = ($blocklist | measure Length -Maximum).Maximum

    # Create an array to store values in
    $valuearray = @()

    # Define the starting index to track the current block being read
    $index = 0

    # Loop through each block in the block list
    for($i=0; $i -lt $blocklist.count; $i++)
    {
        # Create a byte array object to story the bytes from the block
        $downloadArray = New-Object -TypeName byte[] -ArgumentList $maxvalue

        # Download the data into the ByteArray, starting with the current index, for the number of bytes in the current block. Index is increased by 3 when reading to remove preceding comma.
        $CloudBlockBlob.DownloadRangeToByteArray($downloadArray,0,$index, $($blockList[$i].Length)) | Out-Null

        # Increment the index by adding the current block length to the previous index
        $index = $index + $blockList[$i].Length

        # Retrieve the string from the byte array

        $value = [System.Text.Encoding]::ASCII.GetString($downloadArray)

        # Add the log entry to the value array
        $valuearray += $value
    }
    #Return the Array
    $valuearray
}
$valuearray = Get-NSGFlowLogReadBlock -blockList $blockList -CloudBlockBlob $CloudBlockBlob
```

Şimdi `$valuearray` dizi her bloğun dize değerini içerir. Girişi doğrulamak için, ikinciyi diziden son değere `$valuearray[$valuearray.Length-2]`doğru çalıştırarak alın. Kapanış ayraç olduğu için son değeri istemezsin.

Bu değerin sonuçları aşağıdaki örnekte gösterilmiştir:

```json
        {
             "time": "2017-06-16T20:59:43.7340000Z",
             "systemId": "5f4d02d3-a7d0-4ed4-9ce8-c0ae9377951c",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/CONTOSORG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/CONTOSONSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_AllowInternetOutBound","flows":[{"mac":"000D3A18077E","flowTuples":["1497646722,10.0.0.4,168.62.32.14,44904,443,T,O,A","1497646722,10.0.0.4,52.240.48.24,45218,443,T,O,A","1497646725,10.
0.0.4,168.62.32.14,44910,443,T,O,A","1497646725,10.0.0.4,52.240.48.24,45224,443,T,O,A","1497646728,10.0.0.4,168.62.32.14,44916,443,T,O,A","1497646728,10.0.0.4,52.240.48.24,45230,443,T,O,A","1497646732,10.0.0.4,168.62.32.14,44922,443,T,O,A","14976
46732,10.0.0.4,52.240.48.24,45236,443,T,O,A","1497646735,10.0.0.4,168.62.32.14,44928,443,T,O,A","1497646735,10.0.0.4,52.240.48.24,45242,443,T,O,A","1497646738,10.0.0.4,168.62.32.14,44934,443,T,O,A","1497646738,10.0.0.4,52.240.48.24,45248,443,T,O,
A","1497646742,10.0.0.4,168.62.32.14,44942,443,T,O,A","1497646742,10.0.0.4,52.240.48.24,45256,443,T,O,A","1497646745,10.0.0.4,168.62.32.14,44948,443,T,O,A","1497646745,10.0.0.4,52.240.48.24,45262,443,T,O,A","1497646749,10.0.0.4,168.62.32.14,44954
,443,T,O,A","1497646749,10.0.0.4,52.240.48.24,45268,443,T,O,A","1497646753,10.0.0.4,168.62.32.14,44960,443,T,O,A","1497646753,10.0.0.4,52.240.48.24,45274,443,T,O,A","1497646756,10.0.0.4,168.62.32.14,44966,443,T,O,A","1497646756,10.0.0.4,52.240.48
.24,45280,443,T,O,A","1497646759,10.0.0.4,168.62.32.14,44972,443,T,O,A","1497646759,10.0.0.4,52.240.48.24,45286,443,T,O,A","1497646763,10.0.0.4,168.62.32.14,44978,443,T,O,A","1497646763,10.0.0.4,52.240.48.24,45292,443,T,O,A","1497646766,10.0.0.4,
168.62.32.14,44984,443,T,O,A","1497646766,10.0.0.4,52.240.48.24,45298,443,T,O,A","1497646769,10.0.0.4,168.62.32.14,44990,443,T,O,A","1497646769,10.0.0.4,52.240.48.24,45304,443,T,O,A","1497646773,10.0.0.4,168.62.32.14,44996,443,T,O,A","1497646773,
10.0.0.4,52.240.48.24,45310,443,T,O,A","1497646776,10.0.0.4,168.62.32.14,45002,443,T,O,A","1497646776,10.0.0.4,52.240.48.24,45316,443,T,O,A","1497646779,10.0.0.4,168.62.32.14,45008,443,T,O,A","1497646779,10.0.0.4,52.240.48.24,45322,443,T,O,A"]}]}
,{"rule":"DefaultRule_DenyAllInBound","flows":[]},{"rule":"UserRule_ssh-rule","flows":[]},{"rule":"UserRule_web-rule","flows":[{"mac":"000D3A18077E","flowTuples":["1497646738,13.82.225.93,10.0.0.4,1180,80,T,I,A","1497646750,13.82.225.93,10.0.0.4,
1184,80,T,I,A","1497646768,13.82.225.93,10.0.0.4,1181,80,T,I,A","1497646780,13.82.225.93,10.0.0.4,1336,80,T,I,A"]}]}]}
        }
```

Bu senaryo, tüm günlüğü ayrıştırmak zorunda kalmadan NSG akış günlüklerinde girişlerin nasıl okunabildiğini bir örnektir. Blok kimliğini kullanarak veya blok blob'da depolanan blokların uzunluğunu izleyerek yazıldığı gibi günlükteki yeni girişleri okuyabilirsiniz. Bu, yalnızca yeni girişleri okumanızı sağlar.

## <a name="next-steps"></a>Sonraki adımlar


[Ziyaret Elastik Yığın kullanın,](network-watcher-visualize-nsg-flow-logs-open-source-tools.md) [Grafana kullanın](network-watcher-nsg-grafana.md)ve NSG akış günlükleri görüntülemek için yollar hakkında daha fazla bilgi edinmek için [Graylog kullanın.](network-watcher-analyze-nsg-flow-logs-graylog.md) Damlaları doğrudan tüketmek ve tüketicilerin çeşitli günlük analizlerine yaymaya açık kaynak azure işlevi yaklaşımı burada bulunabilir: [Azure Ağ İzleyicisi NSG Akış Günlükleri Bağlayıcısı.](https://github.com/Microsoft/AzureNetworkWatcherNSGFlowLogsConnector)

Trafik akışlarınız hakkında bilgi almak için [Azure Trafik Analitiği'ni](https://docs.microsoft.com/azure/network-watcher/traffic-analytics) kullanabilirsiniz. Trafik Analizi, trafik akışınızı sorgulanabilir hale getirmek için [Log Analytics'i](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal) kullanır.

Depolama lekeleri hakkında daha fazla bilgi için ziyaret edin: [Azure İşlevler Blob depolama ciltleri](../azure-functions/functions-bindings-storage-blob.md)
