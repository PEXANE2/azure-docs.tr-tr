---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/30/2020
ms.author: tamram
ms.openlocfilehash: fe493ca4a2edf16bfda405e8b2c5581c0f745616
ms.sourcegitcommit: 67e8e1caa8427c1d78f6426c70bf8339a8b4e01d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2020
ms.locfileid: "91665629"
---
| Kaynak | Hedef | Hedef (Önizleme) |
|-|-|-|
| Tek blob kapsayıcısının en büyük boyutu | Maksimum depolama hesabı kapasitesinden aynı |  |
| Blok Blobu veya ekleme blobu içindeki maksimum blok sayısı | 50.000 bloklar |  |
| Blok Blobu içindeki bir bloğun en büyük boyutu | 100 MIB | 4000 MIB (Önizleme) |
| Blok Blobun en büyük boyutu | 50.000 X 100 MIB (yaklaşık 4,75 TiB) | 50.000 X 4000 MIB (yaklaşık 190,7 TiB) (Önizleme) |
| Bir ekleme blobundan maksimum blok boyutu | 4 MIB |  |
| Bir ekleme blobunun maksimum boyutu | 50.000 x 4 MiB (yaklaşık 195 GiB) |  |
| Sayfa Blobun en büyük boyutu | 8 TiB<sup>2</sup> |  |
| Blob kapsayıcısı başına en fazla depolanan erişim ilkesi sayısı | 5 |  |
| Tek bir blob için hedef istek oranı | Saniyede en fazla 500 istek |  |
| Tek sayfalı blob için hedef işleme | Saniyede 60 MIB 'ye kadar<sup>2</sup> |  |
| Tek Blok Blobu için hedef aktarım hızı | Depolama hesabı giriş/çıkış sınırlarını artırma<sup>1</sup> |  |

tek bir blob için <sup>1</sup> aktarım hızı, aşağıdakiler dahil olmak üzere çeşitli faktörlere bağlıdır, ancak bunlarla sınırlı değildir: eşzamanlılık, istek boyutu, performans katmanı, karşıya yükleme için kaynak hızı ve indirmeleri için hedef. [Yüksek verimlilik blok bloblarının](https://azure.microsoft.com/blog/high-throughput-with-azure-blob-storage/)performans geliştirmelerinden faydalanmak için, daha büyük blob 'ları veya blokları karşıya yükleyin. Özellikle, standart depolama hesapları için 4 MiB 'den büyük bir blob veya blok boyutu ile [PUT blobu](/rest/api/storageservices/put-blob) veya [PUT bloğu](/rest/api/storageservices/put-block) işlemini çağırın. Premium Blok Blobu veya Data Lake Storage 2. depolama hesapları için 256 KiB 'den büyük bir blok veya blob boyutu kullanın.

<sup>2</sup> sayfa Blobları henüz **hiyerarşik ad alanı** ayarı olan hesaplarda desteklenmez.

Aşağıdaki tabloda hizmet sürümü tarafından izin verilen en yüksek blok ve BLOB boyutları açıklanmaktadır.

| Hizmet sürümü | Maksimum blok boyutu (put bloğu aracılığıyla) | En büyük BLOB boyutu (put blok listesi aracılığıyla) | Tek yazma işlemi aracılığıyla en büyük BLOB boyutu (put blobu aracılığıyla) |
|-|-|-|-|
| Sürüm 2019-12-12 ve üzeri | 4000 MIB (Önizleme) | Yaklaşık 190,7 TiB (4000 MIB X 50.000 bloklar) (Önizleme) | 5000 MIB (Önizleme) |
| Sürüm 2016-05-31 ile sürüm 2019-07-07 | 100 MIB | Yaklaşık 4,75 TiB (100 MiB X 50.000 blok) | 256 MIB |
| 2016-05-31 ' den önceki sürümler | 4 MIB | Yaklaşık 195 GiB (4 MIB X 50.000 blok) | 64 MIB |
