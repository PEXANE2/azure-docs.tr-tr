---
title: Azure dosya paylaşımı – Azure dosya paylaşımından dosyalar silinemedi
description: Azure Dosya Paylaşımı'ndan dosyaların silinmemenizi tanımlayın ve sorun giderin.
author: v-miegge
ms.topic: troubleshooting
ms.author: kartup
manager: dcscontentpm
ms.date: 10/25/2019
ms.service: storage
ms.subservice: common
services: storage
tags: ''
ms.openlocfilehash: d3a3763a8964810626bcdc47da230a9ee406f1f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74196474"
---
# <a name="azure-file-share--failed-to-delete-files-from-azure-file-share"></a>Azure dosya paylaşımı – Azure dosya paylaşımından dosyalar silinemedi

Azure Dosya Paylaşımı'ndan dosyaların silinmemesi birkaç belirti olabilir:

**Belirti 1:**

Aşağıdaki iki sorundan biri nedeniyle azure dosya paylaşımında bir dosyayı silmek için başarısız oldu:

* Silme için işaretlenmiş dosya
* Belirtilen kaynak bir Kobİ istemcisi tarafından kullanılıyor olabilir

**Belirti 2:**

Bu komutu işlemek için yeterli kota yok

## <a name="cause"></a>Nedeni

Hata 1816, dosya paylaşımının monte edildiği bilgisayarda, bir dosya için izin verilen eşzamanlı açık tutamaçların üst sınırına ulaştığınızda oluşur. Daha fazla bilgi için [Azure Depolama performansı ve ölçeklenebilirlik denetim listesine](https://docs.microsoft.com/azure/storage/blobs/storage-performance-checklist)bakın.

## <a name="resolution"></a>Çözüm

Bazı tutamaçları kapatarak eşzamanlı açık tutamaçların sayısını azaltın.

## <a name="prerequisite"></a>Önkoşul

### <a name="install-the-latest-azure-powershell-module"></a>En son Azure PowerShell modüllerini yükleyin

* [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps)

### <a name="connect-to-azure"></a>Azure'a bağlanın:

```
# Connect-AzAccount
```

### <a name="select-the-subscription-of-the-target-storage-account"></a>Hedef depolama hesabının aboneliğini seçin:

```
# Select-AzSubscription -subscriptionid "SubscriptionID"
```

### <a name="create-context-for-the-target-storage-account"></a>Hedef depolama hesabı için bağlam oluşturma:

```
$Context = New-AzStorageContext -StorageAccountName "StorageAccountName" -StorageAccountKey "StorageAccessKey"
```

### <a name="get-the-current-open-handles-of-the-file-share"></a>Dosya paylaşımının geçerli açık tutamaçlarını alın:

```
# Get-AzStorageFileHandle -Context $Context -ShareName "FileShareName" -Recursive
```

## <a name="example-result"></a>Örnek sonuç:

|Gidiş|Yol|ClientIp|ClientPort|Açık Zaman|LastReconnectTime|FileId|Parentıd|SessionId|
|---|---|---|---|---|---|---|---|---|
|259101229083|---|10.222.10.123|62758|2019-10-05|12:16:50Z|0|0|9507758546259807489|
|259101229131|---|10.222.10.123|62758|2019-10-05|12:36:20Z|0|0|9507758546259807489|
|259101229137|---|10.222.10.123|62758|2019-10-05|12:36:53Z|0|0|9507758546259807489|
|259101229136|Yeni klasör/test.zip|10.222.10.123|62758|2019-10-05|12:36:29Z|13835132822072852480|9223446803645464576|9507758546259807489|
|259101229135|test.zip|37.222.22.143|62758|2019-10-05|12:36:24Z|11529250230440558592|0|9507758546259807489|

### <a name="close-an-open-handle"></a>Açık tutamacı kapatın:

Açık bir tutamacı kapatmak için aşağıdaki komutu kullanın:

```
# Close-AzStorageFileHandle -Context $Context -ShareName "FileShareName" -Path 'New folder/test.zip' -CloseAll
```

## <a name="next-steps"></a>Sonraki adımlar

* [Windows'da Azure Dosyaları Sorun Giderme](storage-troubleshoot-windows-file-connection-problems.md)
* [Linux'ta Azure Dosyalarını Sorun Giderme](storage-troubleshoot-linux-file-connection-problems.md)
* [Azure Dosya Eşitleme ile ilgili sorunları giderme](storage-sync-files-troubleshoot.md)