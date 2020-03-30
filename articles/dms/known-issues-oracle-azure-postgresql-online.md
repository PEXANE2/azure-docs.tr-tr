---
title: "Bilinen sorunlar: PostgreSQL için Oracle'dan Azure Veritabanı'na geçiş"
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak PostgreSQL-Single sunucusu için Oracle'dan Azure Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar ve geçiş sınırlamaları hakkında bilgi edinin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/20/2020
ms.openlocfilehash: fcebc7eb170239e5d7efd8a32599a6e782f630bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80235254"
---
# <a name="known-issuesmigration-limitations-with-online-migrations-from-oracle-to-azure-db-for-postgresql-single-server"></a>PostgreSQL-Single sunucusu için Oracle'dan Azure DB'ye çevrimiçi geçişlerle ilgili bilinen sorunlar/geçiş sınırlamaları

PostgreSQL-Single sunucusu için Oracle'dan Azure Veritabanı'na çevrimiçi geçişlerle ilgili bilinen sorunlar ve sınırlamalar aşağıdaki bölümlerde açıklanmıştır.

## <a name="oracle-versions-supported-as-a-source-database"></a>Kaynak veritabanı olarak desteklenen Oracle sürümleri

Azure Veritabanı Geçiş Hizmeti şu zamana bağlanmayı destekler:

- Oracle sürüm 10g, 11g ve 12c.
- Oracle Enterprise, Standart, Express ve Personal Edition.

Azure Veritabanı Geçiş Hizmeti, çok kiracılı kapsayıcı veritabanlarına (CDB) bağlanmayı desteklemez.

## <a name="postgresql-versions-supported-as-a-target-database"></a>Hedef veritabanı olarak desteklenen PostgreSQL sürümleri

Azure Veritabanı Geçiş Hizmeti, PostgreSQL-Single sunucu sürümü 9.5, 9.6, 10 ve 11 için Azure Veritabanı'na geçişleri destekler. PostgreSQL-Single sunucusu için Azure Veritabanı'ndaki sürüm desteği hakkındaki güncel bilgiler için [Desteklenen PostgreSQL veritabanı sürümleri](https://docs.microsoft.com/azure/postgresql/concepts-supported-versions) makalesine bakın.

## <a name="datatype-limitations"></a>Datatype sınırlamaları

Aşağıdaki veri türleri **geçirilmez:**

- Bdosya
- Rowıd
- REF
- Urowıd
- ANYDATA
- SDO_GEOMETRY
- İç içe tablolar
- Kullanıcı tanımlı veri türleri
- Notlar
- Sanal sütunlar
- ROWID sütununa dayalı somutlaştırılmış görünümler

Ayrıca, boş BLOB/CLOB sütunları hedefte NULL'a eşlenir.

## <a name="lob-limitations"></a>LOB sınırlamaları

- Sınırlı boyutlu LOB modu etkinleştirildiğinde, Oracle kaynağındaki boş LOB'lar NULL değerleri olarak çoğaltılır.
- Uzun nesne adları (30'dan fazla bayt) desteklenmez.
- UZUN ve UZUN RAW sütundaki veriler 64k'ı geçemez. 64k'ın ötesindeki tüm veriler kesilir.
- Yalnızca Oracle 12'de, LOB sütunlarında yapılan değişiklikler desteklenmez (geçirilir).
- XMLTYPE ve LOB sütunlarına UPDATEs desteklenmez (geçirilmiştir).

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

- Müşteriler Oracle'a bağlanmak için SYSDBA'yı kullanmalıdır.
- Bölüm/alt bölümleme işlemlerinden (ADD, DROP, EXCHANGE ve TRUNCATE) kaynaklanan veri değişiklikleri geçirilmez ve aşağıdaki hatalara neden olabilir:
  - ADD işlemleri için, eklenen verilerdeki güncelleştirmeler ve silmeler "0 satır etkilenen" uyarısını döndürebilir.
  - DROP ve TRUNCATE işlemleri için yeni ekler "yinelenenler" hatalarına neden olabilir.
  - EXCHANGE işlemleri için hem "0 satır etkilenen" uyarısı hem de "yinelenenler" hataları oluşabilir.
- Adlarında keskofeleri bulunan tablolar çoğaltılamaz.
