---
title: Toplu eklemeleri en iyileştirme-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, bir PostgreSQL için Azure veritabanı-tek sunucu üzerinde toplu ekleme işlemlerini en iyi hale getirebileceğinizi açıklamaktadır.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: 4d10f06577738364e3f4a0ea40221d37ebfb31c0
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116293"
---
# <a name="optimize-bulk-inserts-and-use-transient-data-on-an-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda toplu eklemeleri ve geçici verileri kullanma-tek sunucu 
Bu makalede, toplu ekleme işlemlerini en iyi hale getirebileceğinizi ve PostgreSQL için Azure veritabanı sunucusu üzerinde geçici verileri nasıl kullanabileceğiniz açıklanır.

## <a name="use-unlogged-tables"></a>Günlüğe ait olmayan tabloları kullanma
Geçici verileri içeren veya büyük veri kümelerini toplu olarak ekleyen iş yükü işlemleriniz varsa, günlüğe kaydedilen tabloları kullanmayı göz önünde bulundurun.

Günlüğe ait olmayan tablolar, toplu eklemeleri iyileştirmek için etkili bir şekilde kullanılabilecek bir PostgreSQL özelliğidir. PostgreSQL, yazma öncesi günlüğü (WAL) kullanır. Varsayılan olarak, kararlılık ve dayanıklılık sağlar. Atomicity, tutarlılık, yalıtım ve dayanıklılık ACID özelliklerini yapar. 

Günlüğe edilmemiş bir tabloya ekleme, PostgreSQL 'in bir g/ç işlemi olan işlem günlüğüne yazmadan eklediği anlamına gelir. Sonuç olarak, bu tablolar sıradan tablolardan önemli ölçüde daha hızlıdır.

Günlüğe edilmemiş bir tablo oluşturmak için aşağıdaki seçenekleri kullanın:
- Sözdizimini kullanarak yeni bir günlüğe edilmemiş tablo oluşturun `CREATE UNLOGGED TABLE <tableName>` .
- Mevcut bir günlüğe kaydedilmiş tabloyu söz dizimini kullanarak günlüğe olmayan bir tabloya dönüştürün `ALTER TABLE <tableName> SET UNLOGGED` .  

İşlemi tersine çevirmek için söz dizimini kullanın `ALTER TABLE <tableName> SET LOGGED` .

## <a name="unlogged-table-tradeoff"></a>Günlüğe edilmemiş tablo zorunluluğunu getirir
Günlüğe eklenmeyen tablolar kilitlenme açısından güvenli değildir. Günlüğe edilmemiş bir tablo, kilitlenmeden sonra otomatik olarak kesilir veya temiz olmayan bir kapatmaya tabidir. Günlüğe eklenmeyen bir tablonun içeriği de bekleme sunucularına çoğaltılmaz. Günlüğe kaydedilmemiş bir tabloda oluşturulan dizinlerin tümü de otomatik olarak günlüğe kaydedilmez. Ekleme işlemi tamamlandıktan sonra, ekleme işleminin dayanıklı olması için tabloyu günlüğe kaydedilmiş olarak dönüştürün.

Günlüğe kaydedilen tablolar kullanıldığında bazı müşteri iş yükleri yaklaşık yüzde 15 oranında performans artışı yaşadı.

## <a name="next-steps"></a>Sonraki adımlar
Geçici verilerin ve büyük toplu eklemelerin kullanımı için iş yükünüzü gözden geçirin. Aşağıdaki PostgreSQL belgelerine bakın:
 
- [Tablo SQL komutları oluşturma](https://www.postgresql.org/docs/current/static/sql-createtable.html)
