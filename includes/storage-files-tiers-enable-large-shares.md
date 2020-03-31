---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/27/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 8a2e5defd0672516d52d4f3477641f39eca63368
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597847"
---
Varsayılan olarak, standart dosya paylaşımları yalnızca 5 TiB'ye kadar yayılabilir, ancak paylaşım sınırı 100 TiB'ye yükseltilebilir. Bunu yapmak için, *büyük dosya paylaşımı* özelliği depolama hesabı düzeyinde etkinleştirilmelidir. Tüm premium dosya paylaşımları 100 TiB'nin tam kapasitesine kadar sağlanması için etkinleştirildiğinden, premium depolama hesapları *(FileStorage* depolama hesapları) büyük dosya paylaşımı özelliği bayrağına sahip değildir.

Yalnızca yerel olarak gereksiz veya bölge gereksiz standart depolama hesaplarında büyük dosya paylaşımlarını etkinleştirebilirsiniz. Büyük dosya paylaşımı özelliği bayrağını etkinleştirdikten sonra, artıklık düzeyini coğrafi yedekli veya coğrafi bölge yedekli depolama yla değiştiremezsiniz.

Varolan bir depolama hesabında büyük dosya paylaşımlarını etkinleştirmek için, depolama hesabının içindekiler tablosundaki **Yapılandırma** görünümüne gidin ve büyük dosya paylaşımı rocker anahtarını etkin olarak değiştirin:

![Azure portalında etkinleştirme büyük dosya paylaşımı rocker anahtarının ekran görüntüsü](media/storage-files-tiers-enable-large-shares/enable-lfs-0.png)

PowerShell cmdlet ve [`Set-AzStorageAccount`](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) [`az storage account update`](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update) Azure CLI komutu aracılığıyla 100 TiB dosya paylaşımını da etkinleştirebilirsiniz.

Yeni depolama hesaplarında büyük dosya paylaşımlarını etkinleştirme hakkında daha fazla bilgi edinmek için Azure [dosya paylaşımı oluşturmaya](../articles/storage/files/storage-how-to-create-file-share.md)bakın.