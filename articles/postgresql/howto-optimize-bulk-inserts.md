---
title: Toplu uçları optimize edin - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için bir Azure Veritabanı'nda toplu ekleme işlemlerini nasıl optimize edebilirsiniz.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 4c4bac16917be0064ebb111328753d378d462a2a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74770144"
---
# <a name="optimize-bulk-inserts-and-use-transient-data-on-an-azure-database-for-postgresql---single-server"></a>PostgreSQL - Single Server için bir Azure Veritabanı'nda toplu eklemeleri optimize edin ve geçici verileri kullanın 
Bu makalede, toplu ekleme işlemlerini nasıl optimize edebileceğiniz ve PostgreSQL sunucusu için bir Azure Veritabanı'nda geçici verileri nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="use-unlogged-tables"></a>Günlüğe kaydedilmemiş tabloları kullanma
Geçici veriler içeren veya büyük veri kümelerini toplu olarak ekleyen iş yükü işlemleriniz varsa, günlüğe kaydedilmemiş tablolar kullanmayı düşünün.

Günlüğe kaydedilmemiş tablolar, toplu uçları optimize etmek için etkili bir şekilde kullanılabilen bir PostgreSQL özelliğidir. PostgreSQL, Write-Ahead Logging (WAL) kullanır. Varsayılan olarak atomiklik ve dayanıklılık sağlar. Atomiklik, tutarlılık, izolasyon ve dayanıklılık ACID özelliklerini içerir. 

Kaydedilmemiş bir tabloya eklemek, PostgreSQL'in kendisi bir G/Ç işlemi olan işlem günlüğüne yazmadan ekler yaptığı anlamına gelir. Sonuç olarak, bu tablolar sıradan tablolardan çok daha hızlıdır.

Günlüğe kaydedilmemiş bir tablo oluşturmak için aşağıdaki seçenekleri kullanın:
- Sözdizimini `CREATE UNLOGGED TABLE <tableName>`kullanarak yeni bir günlüğe kaydedilmemiş tablo oluşturun.
- Varolan günlüğe kaydedilmiş bir tabloyu sözdizimini `ALTER TABLE <tableName> SET UNLOGGED`kullanarak günlüğe kaydedilmemiş tabloya dönüştürün.  

İşlemi tersine çevirmek için `ALTER TABLE <tableName> SET LOGGED`sözdizimini kullanın.

## <a name="unlogged-table-tradeoff"></a>Günlüğe kaydedilmemiş masa tradeoff
Günlüğe kaydedilmemiş masalar çarpışma için güvenli değildir. Günlüğe kaydedilmemiş bir tablo, bir kilitlenmeden veya temiz olmayan bir kapatmaya tabi tutulduktan sonra otomatik olarak kesilir. Günlüğe kaydedilmemiş bir tablonun içeriği de bekleme sunucularına çoğaltılmaz. Günlüğe kaydedilmemiş bir tabloda oluşturulan tüm dizinler de otomatik olarak günlüğe kaydedilir. Ekleme işlemi tamamlandıktan sonra, ekleme nin dayanıklı olması için tabloyu günlüğe kaydedin.

Bazı müşteri iş yükleri, günlüğe kaydedilmemiş tablolar kullanıldığında yaklaşık yüzde 15 ila yüzde 20 performans artışı yaşamıştır.

## <a name="next-steps"></a>Sonraki adımlar
Geçici veri ve büyük toplu eklerin kullanımı için iş yükünüzü gözden geçirin. Aşağıdaki PostgreSQL belgelerine bakın:
 
- [Tablo SQL komutları oluşturma](https://www.postgresql.org/docs/current/static/sql-createtable.html)
