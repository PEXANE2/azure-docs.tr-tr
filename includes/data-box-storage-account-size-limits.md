---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: 3e1f9c225a57e7d41f85c2a92dac989453057c51
ms.sourcegitcommit: 4499035f03e7a8fb40f5cff616eb01753b986278
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2020
ms.locfileid: "82736986"
---
Depolama hesabına kopyalanmış verilerin boyutuna ilişkin sınırlar aşağıda verilmiştir. Karşıya yüklediğiniz verilerin bu sınırlara uyduğundan emin olun. Bu limitlerle ilgili en güncel bilgiler için bkz. blob depolama ve [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../articles/storage/files/storage-files-scale-targets.md) [için ölçeklenebilirlik ve performans hedefleri](../articles/storage/blobs/scalability-targets.md) .

| Azure depolama hesabına kopyalanmış verilerin boyutu                      | Varsayılan limit          |
|---------------------------------------------------------------------|------------------------|
| Blok Blobu ve Sayfa Blobu                                            | ABD ve Avrupa için 2 PB.<br>UK 'yi içeren diğer tüm bölgeler için 500 TB.  <br> Bu, Data Box dahil olmak üzere tüm kaynaklardaki verileri içerir.|
| Azure Dosyaları                                                          | Standart dosya paylaşımlarının en büyük boyutu 100TiB *, 5 TB, Premium dosya paylaşımları paylaşım başına 100 Tib.<br> *StorageAccount_AzureFiles* altındaki tüm klasörler bu sınırı izlemelidir.       |
