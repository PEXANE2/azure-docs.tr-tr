---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: 6a9c31eb46a457433834d5940b3f7e190ebe1476
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75469616"
---
Depolama hesabına kopyalanmış verilerin boyutuna ilişkin sınırlar aşağıda verilmiştir. Karşıya yüklediğiniz verilerin bu sınırlara uyduğundan emin olun. Bu limitlerle ilgili en güncel bilgiler için bkz. blob depolama ve [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../articles/storage/files/storage-files-scale-targets.md) [için ölçeklenebilirlik ve performans hedefleri](../articles/storage/blobs/scalability-targets.md) .

| Azure depolama hesabına kopyalanmış verilerin boyutu                      | Varsayılan limit          |
|---------------------------------------------------------------------|------------------------|
| Blok Blobu ve Sayfa Blobu                                            | ABD ve Avrupa için 2 PB.<br>UK 'yi içeren diğer tüm bölgeler için 500 TB.  <br> Bu, Data Box dahil olmak üzere tüm kaynaklardaki verileri içerir.|
| Azure Dosyaları                                                          | paylaşma başına 5 TB.<br> *StorageAccount_AzureFiles* altındaki tüm klasörler bu sınırı izlemelidir.       |
