---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/24/2020
ms.author: alkohli
ms.openlocfilehash: acaebcea59e765f5544f1bfbd692c6508f66a84a
ms.sourcegitcommit: bdd5c76457b0f0504f4f679a316b959dcfabf1ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "91025227"
---
Depolama hesabına kopyalanmış verilerin boyutuna ilişkin sınırlar aşağıda verilmiştir. Karşıya yüklediğiniz verilerin bu sınırlara uyduğundan emin olun. Bu limitlerle ilgili en güncel bilgiler için bkz. blob depolama ve [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../articles/storage/files/storage-files-scale-targets.md) [için ölçeklenebilirlik ve performans hedefleri](../articles/storage/blobs/scalability-targets.md) .

| Azure depolama hesabına kopyalanmış verilerin boyutu                      | Varsayılan limit          |
|---------------------------------------------------------------------|------------------------|
| Blok Blobu ve Sayfa Blobu                                            | Maksimum sınır, [Azure aboneliği için tanımlanan depolama sınırı](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#storage-limits) ile aynıdır ve Data Box dahil olmak üzere tüm kaynaklardaki verileri içerir. |
| Azure Dosyaları                                                          | Data Box, Data Box sırası oluşturulmadan önce etkinleştirilmişse büyük dosya paylaşımlarını destekler (100TiB). <br> Sipariş oluşturma işleminden önce etkinleştirilmemişse, desteklenen en büyük dosya paylaşma boyutu 5 TiB olur. <br> Premium dosya paylaşımları henüz desteklenmiyor.<br> *StorageAccount_AzureFiles* altındaki tüm klasörler bu sınırı izlemelidir. <br> Daha fazla bilgi için bkz. [büyük dosya paylaşımlarını etkinleştirme ve oluşturma](../articles/storage/files/storage-files-how-to-create-large-file-share.md)      |
