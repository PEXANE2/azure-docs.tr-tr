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
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597847"
---
Varsayılan olarak, standart dosya paylaşımları yalnızca 5 TiB 'ye kadar yayılabilir, ancak paylaşım sınırı 100 TiB 'ye artırılabilir. Bunu yapmak için, depolama hesabı düzeyinde *büyük dosya paylaşma* özelliğinin etkinleştirilmesi gerekir. Premium Depolama hesapları (*FileStorage* depolama hesapları), tüm Premium dosya paylaşımları tam 100 TİB kapasitesine sağlanması için zaten etkinleştirildiğinden büyük dosya paylaşım özelliği bayrağına sahip değildir.

Yalnızca yerel olarak yedekli veya bölge yedekli standart depolama hesaplarında büyük dosya paylaşımlarını etkinleştirebilirsiniz. Büyük dosya paylaşma özelliği bayrağını etkinleştirdikten sonra, artıklık düzeyini coğrafi olarak yedekli veya coğrafi bölgeye yedekli depolama olarak değiştiremezsiniz.

Mevcut bir depolama hesabında büyük dosya paylaşımlarını etkinleştirmek için, depolama hesabının İçindekiler tablosundaki **yapılandırma** görünümüne gidin ve büyük dosya paylaşımı rocker anahtarını etkin olarak değiştirin:

![Azure portal büyük dosya paylaşımının rocker anahtarının bir ekran görüntüsü](media/storage-files-tiers-enable-large-shares/enable-lfs-0.png)

Ayrıca, [`Set-AzStorageAccount`](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) PowerShell cmdlet 'i ve [`az storage account update`](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update) Azure CLI komutu aracılığıyla 100 tib dosya paylaşımlarını etkinleştirebilirsiniz.

Yeni depolama hesaplarında büyük dosya paylaşımlarının nasıl etkinleştirileceği hakkında daha fazla bilgi edinmek için bkz. [Azure dosya paylaşımı oluşturma](../articles/storage/files/storage-how-to-create-file-share.md).