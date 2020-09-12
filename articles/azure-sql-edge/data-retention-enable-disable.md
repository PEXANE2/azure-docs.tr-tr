---
title: Veri saklama ilkelerini etkinleştirme ve devre dışı bırakma-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de veri saklama ilkelerini etkinleştirme ve devre dışı bırakma hakkında bilgi edinin (Önizleme)
keywords: SQL Edge, veri saklama
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/04/2020
ms.openlocfilehash: 9787f2cfa87a16d9e7dd1753e4389977c6753b81
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89550726"
---
# <a name="enable-and-disable-data-retention-policies"></a>Veri saklama ilkelerini etkinleştirme ve devre dışı bırakma

Bu konu, bir veritabanı ve tablo için veri saklama ilkelerinin nasıl etkinleştirileceğini ve devre dışı bırakılacağını açıklar. 

## <a name="enable-data-retention-for-a-database"></a>Veritabanı için veri bekletmesini etkinleştirme

Aşağıdaki örnekte, [alter database](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options)kullanılarak veri bekletmenin nasıl etkinleştirileceği gösterilmektedir.

> [!NOTE]
> Azure SQL Edge 'de (Önizleme) veri saklama özelliğini etkinleştirmek için, TF 12825 ' i başlangıç seçeneği olarak etkinleştirin veya DBCC TRACEON komutunu kullanın. Bir MSSQL. conf dosyası kullanarak izleme bayraklarını etkinleştirme hakkında daha fazla bilgi için bkz. [MSSQL. conf dosyası kullanarak yapılandırma](configure.md#configure-by-using-an-mssqlconf-file). 

```sql
ALTER DATABASE [<DatabaseName>] SET DATA_RETENTION  ON;
```

## <a name="check-if-data-retention-is-enabled-for-a-database"></a>Veri saklama 'nin bir veritabanı için etkin olup olmadığını denetleyin

Bir veritabanı için veri saklama özelliğinin etkinleştirilip etkinleştirilmediğini denetlemek için aşağıdaki komut kullanılabilir
```sql
SELECT is_data_retention_enabled, name
FROM sys.databases;
```

## <a name="enable-data-retention-for-a-table"></a>Tablo için veri bekletmesini etkinleştirme

Verilerin otomatik olarak Temizlenebileceği her tablo için veri saklama özelliğinin etkinleştirilmesi gerekir. Veritabanında ve tabloda veri saklama etkinleştirildiğinde, bir arka plan sistem görevi, artık kullanılmayan (eski) satırları tanımlamak ve silmek için tabloyu düzenli olarak tarar. Veri saklama, [tablo oluşturma kullanılarak](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) tablo oluşturma sırasında veya [alter table](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql)kullanılarak etkinleştirilebilir.

Aşağıdaki örnek, tablo [Oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql)kullanılarak tablo için veri bekletmenin nasıl etkinleştirileceğini gösterir. 

```sql
CREATE TABLE [dbo].[data_retention_table] 
(
[dbdatetime2] datetime2(7), 
[product_code] int, 
[value] char(10),  
CONSTRAINT [pk_current_data_retention_table] PRIMARY KEY CLUSTERED ([product_code])
) WITH (DATA_DELETION = ON ( FILTER_COLUMN = [dbdatetime2], RETENTION_PERIOD = 1 day ) )
```

`WITH (DATA_DELETION = ON ( FILTER_COLUMN = [dbdatetime2], RETENTION_PERIOD = 1 day ) )`Create Table komutunun bölümü tablodaki veri bekletmesini ayarlar. Komut aşağıdaki gerekli parametreleri kullanır 

- DATA_DELETION-verilerin bekletilmesinin açık veya kapalı olduğunu gösterir.
- FILTER_COLUMN-tablodaki sütununda, satırlar kullanımdan kalktı durumunda değil, bu sütun için kullanılacak ad. Filtre sütunu yalnızca aşağıdaki veri türlerine sahip bir sütun olabilir 
    - Tarih
    - Girişin
    - Tarih-Saat
    - DateTime2
    - DateTimeOffset
- RETENTION_PERIOD-bir tamsayı değer ve ardından bir birim tanımlayıcısı. İzin verilen birimler gün, hafta, ay ve yıl.

Aşağıdaki örnekte, [alter table](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql)kullanılarak tablo için veri bekletmenin nasıl etkinleştirileceği gösterilmektedir.  

```sql
Alter Table [dbo].[data_retention_table]
SET (DATA_DELETION = On (FILTER_COLUMN = [timestamp], RETENTION_PERIOD = 1 day))
```

## <a name="check-if-data-retention-is-enabled-for-a-table"></a>Bir tablo için veri saklama özelliğinin etkinleştirilip etkinleştirilmediğini denetleyin

Aşağıdaki komut, veri saklama özelliğinin etkinleştirildiği tabloları denetlemek için kullanılabilir

```sql
select name, data_retention_period, data_retention_period_unit from sys.tables
```

Data_retention_period =-1 değeri ve sonsuz olarak data_retention_period_unit, tabloda veri saklama alanının belirlenmediğini gösterir.

Aşağıdaki sorgu, veri saklama için filter_column olarak kullanılan sütunu belirlemek için kullanılabilir. 

```sql
Select name from sys.columns
where is_data_deletion_filter_column =1 
and object_id = object_id(N'dbo.data_retention_table', N'U')
```

## <a name="corelating-db-and-table-data-retention-settings"></a>VERITABANı ve tablo verilerini bekletme ayarlarını birlikte ilişkilendirme

Veritabanındaki ve tablodaki veri saklama ayarı, eski satırlar için oto temizleme 'nin tablolarda çalışacağını ve bu tabloda birlikte kullanılacağını tespit etmek için kullanılır. 

|Veritabanı seçeneği | Tablo seçeneği | Davranış |
|----------------|--------------|----------|
| KAPALI | KAPALI | Veri bekletme ilkesi devre dışı bırakıldı ve eski kayıtların otomatik ve el ile temizlenmesi devre dışı bırakıldı.|
| KAPALI | AÇIK  | Tablo için veri bekletme ilkesi etkinleştirilir, ancak eski kayıtlar otomatik ve el ile temizlik devre dışı bırakılır. |
| AÇIK | KAPALI | Veri bekletme ilkesi veritabanı düzeyinde etkindir. Ancak, seçenek tablo düzeyinde devre dışı bırakıldığı için, eski satır için bekletme tabanlı temizlik yoktur.|
| AÇIK | AÇIK | Veri saklama ilkesi hem veritabanı hem de tablolar için etkinleştirilmiştir. Eski kayıtları otomatik/el ile temizleme etkin |

## <a name="disable-data-retention-on-a-table"></a>Tabloda veri bekletmeyi devre dışı bırakma 

Veri saklama bir tabloda [alter table](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql)kullanılarak devre dışı bırakılabilir. Aşağıdaki komut, bir tabloda veri bekletmesini devre dışı bırakmak için kullanılabilir.

```sql
Alter Table [dbo].[data_retention_table]
Set (DATA_DELETION = OFF)
```

## <a name="disable-data-retention-on-a-database"></a>Veritabanında veri bekletmesini devre dışı bırakma

Veri saklama bir tabloda [alter database](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-options)kullanılarak devre dışı bırakılabilir. Aşağıdaki komut bir veritabanında veri bekletmesini devre dışı bırakmak için kullanılabilir.

```sql
ALTER DATABASE <DatabaseName> SET DATA_RETENTION  OFF;
```

## <a name="next-steps"></a>Sonraki adımlar
- [Veri saklama ve otomatik veri temizleme](data-retention-overview.md)
- [Geçmiş verilerini bekletme ilkesiyle yönetme](data-retention-cleanup.md)
