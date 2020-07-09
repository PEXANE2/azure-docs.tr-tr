---
title: PostgreSQL için Azure veritabanı 'nda tablo depolama stratejisini kullanarak sorgu süresini iyileştirin-tek sunucu
description: Bu makalede, bir PostgreSQL için Azure veritabanı-tek sunucu üzerinde sorgu süresinin BILDIRIM tablosu depolama stratejisi ile nasıl iyileştirileceği açıklanır.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: 3f1fa0affb821b00d4f5529841533e854e634377
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116191"
---
# <a name="optimize-query-time-with-the-toast-table-storage-strategy"></a>Sorgu süresini, BILDIRIM tablosu depolama stratejisi ile iyileştirin 
Bu makalede, sorgu sürelerinin büyük-öznitelik depolama tekniği (BILDIRIM) tablosu depolama stratejisi ile nasıl iyileştirileceği açıklanır.

## <a name="toast-table-storage-strategies"></a>BILDIRIM tablosu depolama stratejileri
Sütunları, BILDIRIM kullanan diskte depolamak için dört farklı strateji kullanılır. Bunlar, sıkıştırma ve satır dışı depolama arasındaki çeşitli birleşimleri temsil eder. Strateji, veri türü düzeyinde ve sütun düzeyinde ayarlanabilir.
- **Düz** sıkıştırma ya da satır dışı depolamayı önler. Bu, varlena türleri için tek baytlık üst bilgilerin kullanımını devre dışı bırakır. Düz, veri türleri için BILDIRIM kullanmıyorum olabilecek tek stratejidir.
- **Genişletilmiş** , sıkıştırma ve satır dışı depolamaya izin verir. Genişletilmiş, BILDIRIM kullanan çoğu veri türü için varsayılandır. Önce sıkıştırma denenir. Satır hala çok büyükse satır dışı depolama denenir.
- **Dış** , satır dışı depolamaya izin veriyor ancak sıkıştırma değil. External kullanımı, geniş metin ve bytea sütunları daha hızlı alt dize işlemleri yapar. Bu hız, daha fazla depolama alanı ile birlikte gelir. Bu işlemler, sıkıştırılmamışsa yalnızca satır dışı değerin gerekli kısımlarını getirmek için iyileştirilmiştir.
- **Main** , sıkıştırmaya izin veriyor ancak satır dışı depolamaya izin vermez. Satır dışı depolama, yalnızca son çare olarak bu gibi sütunlar için de gerçekleştirilir. Satırı sayfaya sığacak kadar küçük yapmanın başka bir yolu olmadığında gerçekleşir.

## <a name="use-toast-table-storage-strategies"></a>BILDIRIM tablosu depolama stratejilerini kullanma
Sorgularınız, BILDIRIM kullanan veri türlerine erişebilmişse, sorgu sürelerini azaltmak için varsayılan genişletilmiş seçenek yerine ana stratejiyi kullanmayı göz önünde bulundurun. Main, satır dışı depolamayı değil. Sorgularınız, BILDIRIM kullanan veri türlerine erişmezse, genişletilmiş seçeneği korumak faydalı olabilir. Ana tablodaki satırların daha büyük bir kısmı, performansa yardımcı olan paylaşılan arabellek önbelleğine sığar.

Geniş tablolar ve yüksek karakter sayıları içeren bir şema kullanan bir iş yükünüz varsa, PostgreSQL BILDIRIM tablolarını kullanmayı göz önünde bulundurun. Örnek Müşteri tablosunda, 255 karakterden oluşan birkaç sütun içeren 350 sütundan daha fazla sütun vardı. BILDIRIM tablosu ana stratejisine dönüştürüldükten sonra, kıyaslama sorgusu süresi 4203 saniyeden 467 saniyeye indirilir. Bu, yüzde 89 bir iyileştirmedir.

## <a name="next-steps"></a>Sonraki adımlar
Önceki özellikler için iş yükünüzü gözden geçirin. 

Aşağıdaki PostgreSQL belgelerini gözden geçirin: 
- [Bölüm 68, veritabanı fiziksel depolaması](https://www.postgresql.org/docs/current/storage-toast.html) 