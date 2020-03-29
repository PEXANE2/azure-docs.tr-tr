---
title: PostgreSQL için Azure Veritabanı'ndaki TOAST tablo depolama stratejisini kullanarak sorgu süresini optimize edin - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için bir Azure Veritabanı'nda TOAST tablo depolama stratejisi ile sorgu süresi nin nasıl optimize edilebildiğini açıklanmaktadır.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: ac1dc43a2b89bc1cc748947ec08e6ada87edbfcb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "65066992"
---
# <a name="optimize-query-time-with-the-toast-table-storage-strategy"></a>TOAST tablo depolama stratejisi ile sorgu süresini optimize edin 
Bu makalede, büyük boyutlu öznitelik depolama tekniği (TOAST) tablo depolama stratejisi ile sorgu süreleri optimize etmek için nasıl açıklanmaktadır.

## <a name="toast-table-storage-strategies"></a>TOAST tablo depolama stratejileri
TOST'u kullanabilen sütunları diskte depolamak için dört farklı strateji kullanılır. Sıkıştırma ve satır dışı depolama arasındaki çeşitli kombinasyonları temsil ederler. Strateji, veri türü düzeyinde ve sütun düzeyinde ayarlanabilir.
- **Düz** sıkıştırma veya satır dışı depolama önler. Varlena türleri için tek bayt üstbilgi kullanımını devre dışı kılabilir. Düz TOST kullanamazsınız veri türleri sütunları için tek olası stratejidir.
- **Genişletilmiş** hem sıkıştırma hem de hat dışı depolama sağlar. Genişletilmiş TOST kullanabilirsiniz çoğu veri türleri için varsayılandır. Sıkıştırma ilk denenir. Satır hala çok büyükse, satır dışı depolama denemesi yapılır.
- **Harici,** satır dışı depolamaya izin verir, ancak sıkıştırmasağlar. Dış kullanımı, geniş metin ve bytea sütunlarında substring işlemlerini daha hızlı yapar. Bu hız, artan depolama alanının cezasıyla birlikte gelir. Bu işlemler, sıkıştırılmadığında yalnızca satır dışı değerin gerekli bölümlerini almak için optimize edilmiştir.
- **Main** sıkıştırmaya izin verir, ancak satır dışı depolamaya izin vermez. Bu tür sütunlar için çevrimdışı depolama yine de son çare olarak gerçekleştirilir. Satırı bir sayfaya sığacak kadar küçük yapmanın başka bir yolu olmadığında oluşur.

## <a name="use-toast-table-storage-strategies"></a>TOAST tablo depolama stratejilerini kullanın
Sorgularınız TOAST'ı kullanabilen veri türlerine erişecekse, sorgu sürelerini azaltmak için varsayılan Genişletilmiş seçeneği yerine Ana stratejiyi kullanmayı düşünün. Main, hat dışı depolamayı ekarte etmez. Sorgularınız TOAST'ı kullanabilen veri türlerine erişmiyorsa, Genişletilmiş seçeneğini korumak yararlı olabilir. Ana tablonun satırlarının daha büyük bir kısmı paylaşılan arabellek önbelleğine sığar ve bu da performansa yardımcı olur.

Geniş tablolara ve yüksek karakter sayılarına sahip bir şema kullanan bir iş yükünüz varsa, PostgreSQL TOAST tablolarını kullanmayı düşünün. Örnek bir müşteri tablosunda, 255 karaktere yayılan birkaç sütuniçeren 350'den fazla sütun vardı. Toast tablosuana stratejiye dönüştürüldükten sonra, kıyaslama sorgulama süreleri 4203 saniyeden 467 saniyeye düşürüldü. Bu yüzde 89'luk bir gelişme.

## <a name="next-steps"></a>Sonraki adımlar
Önceki özellikler için iş yükünüzü gözden geçirin. 

Aşağıdaki PostgreSQL belgelerini inceleyin: 
- [Bölüm 68, Veritabanı fiziksel depolama](https://www.postgresql.org/docs/current/storage-toast.html) 