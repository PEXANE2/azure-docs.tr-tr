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
ms.openlocfilehash: 327e86642041a607ada7c1173bb053b12a41832c
ms.sourcegitcommit: a43a59e44c14d349d597c3d2fd2bc779989c71d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2020
ms.locfileid: "96011772"
---
Varsayılan olarak, standart dosya paylaşımları yalnızca 5 TiB 'ye kadar yayılabilir, ancak paylaşım sınırı 100 TiB 'ye artırılabilir. Bunu yapmak için, depolama hesabı düzeyinde *büyük dosya paylaşma* özelliğinin etkinleştirilmesi gerekir. Premium Depolama hesapları (*FileStorage* depolama hesapları), tüm Premium dosya paylaşımları tam 100 TİB kapasitesine sağlanması için zaten etkinleştirildiğinden büyük dosya paylaşım özelliği bayrağına sahip değildir.

Yalnızca yerel olarak yedekli veya bölge yedekli standart depolama hesaplarında büyük dosya paylaşımlarını etkinleştirebilirsiniz. Büyük dosya paylaşma özelliği bayrağını etkinleştirdikten sonra, artıklık düzeyini coğrafi olarak yedekli veya coğrafi bölgeye yedekli depolama olarak değiştiremezsiniz.

Mevcut bir depolama hesabında büyük dosya paylaşımlarını etkinleştirmek için, depolama hesabının İçindekiler tablosundaki **yapılandırma** görünümüne gidin ve büyük dosya paylaşımı rocker anahtarını etkin olarak değiştirin:

![Azure portal büyük dosya paylaşımının rocker anahtarının bir ekran görüntüsü](media/storage-files-tiers-enable-large-shares/enable-lfs-0.png)

Ayrıca, [`Set-AzStorageAccount`](/powershell/module/az.storage/set-azstorageaccount) PowerShell cmdlet 'i ve [`az storage account update`](/cli/azure/storage/account#az-storage-account-update) Azure CLI komutu aracılığıyla 100 tib dosya paylaşımlarını da etkinleştirebilirsiniz. Büyük dosya paylaşımlarının etkinleştirilmesi hakkında ayrıntılı yönergeler için bkz. [Enable ve, büyük dosya paylaşımlarını oluşturma](../articles/storage/files/storage-files-how-to-create-large-file-share.md).

Yeni depolama hesaplarında dosya paylaşımları oluşturma hakkında daha fazla bilgi edinmek için bkz. [Azure dosya paylaşımı oluşturma](../articles/storage/files/storage-how-to-create-file-share.md).