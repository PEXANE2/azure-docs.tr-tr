---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 11/08/2019
ms.author: tamram
ms.openlocfilehash: 2ed88d8abb7cbe96093b68d89030e6e464a35541
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "75392390"
---
| Kaynak | Hedef        |
|----------|---------------|
| Tek blob kapsayıcısının en büyük boyutu | Maksimum depolama hesabı kapasitesinden aynı |
| Blok Blobu veya ekleme blobu içindeki maksimum blok sayısı | 50.000 bloklar |
| Blok Blobu içindeki bir bloğun en büyük boyutu | 100 MIB |
| Blok Blobun en büyük boyutu | 50.000 X 100 MIB (yaklaşık 4,75 TiB) |
| Bir ekleme blobundan maksimum blok boyutu | 4 MIB |
| Bir ekleme blobunun maksimum boyutu | 50.000 x 4 MiB (yaklaşık 195 GiB) |
| Sayfa Blobun en büyük boyutu | 8 TiB |
| Blob kapsayıcısı başına en fazla depolanan erişim ilkesi sayısı | 5 |
|Tek bir blob için hedef istek oranı | Saniyede en fazla 500 istek |
|Tek sayfalı blob için hedef işleme | Saniyede 60 MIB 'ye kadar |
|Tek Blok Blobu için hedef aktarım hızı |Depolama hesabı giriş/çıkış sınırlarını artırma<sup>1</sup> |

tek bir blob için <sup>1</sup> aktarım hızı, aşağıdakiler dahil olmak üzere çeşitli faktörlere bağlıdır, ancak bunlarla sınırlı değildir: eşzamanlılık, istek boyutu, performans katmanı, karşıya yükleme için kaynak hızı ve indirmeleri için hedef. [Yüksek verimlilik blok bloblarının](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/)performans geliştirmelerinden faydalanmak için, daha büyük blob 'ları veya blokları karşıya yükleyin. Özellikle, standart depolama hesapları için 4 MiB 'den büyük bir blob veya blok boyutu ile [PUT blobu](/rest/api/storageservices/put-blob) veya [PUT bloğu](/rest/api/storageservices/put-block) işlemini çağırın. Premium Blok Blobu veya Data Lake Storage 2. depolama hesapları için 256 KiB 'den büyük bir blok veya blob boyutu kullanın.
